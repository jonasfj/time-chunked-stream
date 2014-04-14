time-chunked-stream - Time-based Streaming Chunking
===================================================

A node.js deplux stream that buffers input for a while before outputting it.
This is useful if you have an readable-stream producing many small chunks that
you want to pipe to a writable-stream and the writable-stream only handles large
chunks efficiently.

Consider the following setup:
  * `streamIn`, produces a lot of small chunks (e.g. output from a subprocess),
  * `streamOut`, takes a long time to write a single chunk regardless of size
    (e.g. sends the chunk over network to a database and commits)

Now, `streamIn.pipe(streamOut)` would be a bad idea, because `streamOut` has a
large overhead for each chunk regardless of size. Instead using
`streamIn.pipe(new TimeChunkedStream({timeout: 5000})).pipe(streamOut)` would
buffer the chunks for 5 seconds and then write them to `streamOut`.

This is mainly useful for logging and thing where you want to commit and the
commit overhead is significant. If you want to optimizing file-writes or cases
with plenty of data consider using
[buffered-stream](https://www.npmjs.org/package/buffered-stream) instead.

License
-------
The `time-chunked-stream` library is released on the MIT license, see the
`LICENSE` for complete license.
