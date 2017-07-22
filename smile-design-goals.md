# Smile Format: design goals

Since there is no standard binary representation of JSON data model -- not even real proposals (closest thing is
[BSON](http://bsonspec.org/), which despite its name, is NOT fully JSON compatible) --
[Jackson](../../../jackson) JSON processor team decided to specify a format that is just that.
Work started in 2010, and an initial version was completed by the same year, as well as first compliant parser and
generator implementation as part of Jackson 1.6 release.

This document covers design rationale for the data format; format itself is explained on
[Smile Format specification](smile-format-specification.md)

## 0. Background

One of opportunities for improving both space- and time efficiency of data transfer with JSON-based tools would be defining a binary format that has same logical information model -- similar to various binary XML efforts, compared to canonical textual XML -- and that could thereby be accessed through existing JSON APIs, offering equivalent set of functionality.

## 1. Goals

Design goals for the desired format are:

 * MUST support all logical JSON events; should not add extensions beyond what existing Jackson API offers.
 * SHOULD be reasonably space-efficient (compact); to reduce "fluff"; especially to degree this can help processing efficiency.
 * SHOULD be both efficient to read (deserialize) AND write (serialize): many binary formats make significant sacrifices in write speed.
 * SHOULD avoid overly complex structures and algorithms
 * SHOULD avoid fragility:
  * Keep enough redundancy to allow for some self-consistency checks; specifically, there should be byte sequences that are invalid, to make detection of invalid content possible.
  * Add explicit format version number (nothing drastic, even a nibble will do)
 * MUST support auto-detection: Use a unique header ("magic cookie")
 * SHOULD allow simple concatenation of content; that is, concatenation of valid properly nested sequences (that is, no open start object or start array markers) must be legal format in itself
  * Aimed to allow chunked content output
 * SHOULD allow proper streaming: that is, amount of required buffering can not exceed some fixed constant, size of which is related to low-level buffering, not to length of content to encoded. Note, however, that in cases where Jackson API itself imposes limits (case for embedded binary data; as well as for String values to output), implementation can make use of this existing limitation
  * This specifically prevents pervasive use of length-prefix for Strings: to know byte-length of a Java String, one would need to either do additional passes (first to calculate length, second to encode), or to buffer encoded output in memory.
* SHOULD be usable with [[http://en.wikipedia.org/wiki/Web_socket | WebSockets]] to degree feasible
  * Means that effort should be made to avoid use of byte 0xFF (end marker) -- for some content (binary data), it may be necessary to introduce separate "safe" (or "framing"?) mode to use additional escaping?
  * Perhaps there should be a way to use String-encoding for numbers to allow for "Web Sockets safe" mode?
* SHOULD support simple framing -- ability to separate binary-encoded content segments from each other AND efficiently scan to find these boundaries _without_ having to parse/decode contents. Framing is easy to do with textual JSON by using something as simple as linefeed, since linefeeds are always quoted in String values, as long as no indentation is used.

## 2. Non-goals

To further help design the format, following things were explicitly considered to be non-goals:

* SHOULD NOT over-optimize for minimal compactness: that's what compression can be used for.
* SHOULD NOT extend model beyond basic JSON data types; with exception of natively supporting binary content (which for textual JSON is supported using Base64 encoded text).
* NEED NOT design for random-access: low-level Jackson APIs are designed for sequential access; can implement "low-hanging fruits" if that makes sense.
* NEED NOT support extensive configurability: only settings that are considered obviously useful (or requested) should be added.

Guidelines derived from above:

* If goals of space-efficiency (compactness) and time-efficiency (processing speed) conflict, prefer time-efficiency over space-efficiency.
