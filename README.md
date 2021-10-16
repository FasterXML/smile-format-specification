# Smile Data Format

"Smile" is a binary data format that defines a binary equivalent of standard
[JSON](http://en.wikipedia.org/wiki/JSON) data format.

Format was specified in 2010 by [Jackson](../../../jackson) JSON processor development team.
First compliant implementation was included as a Jackson backend for Jackson version 1.6,
released in September 2010.

##  Specification

Design documentation includes:

* [Smile Format Specification](smile-specification.md)  describes format itself; how it works and what a compliant parser/generator implementations needs to do.
* [Smile Format Design Goals](smile-design-goals.md) explains rationale for design decisions concerning specification.

## Community

* [Smile format Google group](http://groups.google.com/group/smile-format-discussion)

## Documentation

* [SmileFAQ](smile-faq.md)
* [Smile Wikipedia entry](https://en.wikipedia.org/wiki/Smile_(data_interchange_format))
* User documentation:
    * [Understanding Smile data format](https://medium.com/code-with-ayush/understanding-smile-a-data-format-based-on-json-29972a37d376)

## Implementations

Smile Codecs

* Java
    * [Jackson](../../../jackson) provides Smile support through [jackson-dataformat-smile](../../../jackson-dataformats-binary) modules) format codec
        * Full support: including streaming access, data binding and tree model (100% parity with textual JSON)
        * *NEW* (2017) [Jackson 2.9](https://github.com/FasterXML/jackson/wiki/Jackson-Release-2.9) added "non-blocking" ((aka "asynchronous") decoding for JSON and Smile format backends
    * [Protostuff](http://github.com/protostuff/protostuff) project supports Smile both as a low-level data format, and as format used for its RPC implementation
* JVM, other:
    * [Clojure](http://clojure.org) 
        * [Cheshire](https://github.com/dakrone/cheshire) library offers support via Jackson `jackson-dataformat-smile`
* C
    * [libsmile](https://github.com/pierre/libsmile) is a small C-library for reading and writing Smile data.
* Go
    * [go-smile](https://github.com/zencoder/go-smile) Smile decoder written in Golang.
* Javascript
    * [smile-js](https://github.com/ngyewch/smile-js) Smile decoder written in Javascript
* Python
    * [PySmile](https://github.com/jhosmer/PySmile) Python codec
 * Rust
    * [serde-smile](https://github.com/sfackler/serde-smile) Serde serializer and deserializer written in Rust.

Frameworks, Systems that use Smile codec (encoder and decoder)

* [Elastic Search](http://www.elastic.co) uses Smile as transport format supports access using Smile encoding.
* [Apache Solr](http://lucene.apache.org/solr) can use Smile as the response format with the `wt=smile` parameter.


