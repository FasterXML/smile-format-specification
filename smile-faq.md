# Smile Format: FAQ

## Where did the name come from?

Name came from the half-serious idea of using a smiley as the "magic cookie", part of short
header that can be used to detect Smile format encoded documents.

## What is the purpose of "Safe [binary] Encoding"?

The main idea is to allow use of specific marker, `0xFF` for framing
purpose -- that is, as a document separator, to allow "blind"
splitting of a stream of Smile-encoded documents into chunks of
documents.
"Blind" here means that splitter can start at ANY byte position,
looking for split marker, without having to consider document
structure or token boundaries: it just "blindly" focuses on finding
next (or, previous) `0xFF` and knows that MUST be a document boundary.

Currently (version 1.0) byte value of `0xFE` is also avoided if (but
only if!) using Safe Binary Encoding setting.

Note: if not writing Binary data at all, this setting has no
relevance (and byte values of `0xFF` and `0xFE` are not used at all either).
