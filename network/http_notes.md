#HTTP notes

## HTTP Headers
### Transfer Codings
```
transfer-coding: "chunked"

--- encoded data
4\r\n
Wiki\r\n
5\r\n
pedia\r\n
E\r\n
 in\r\n
\r\n
chunks.\r\n
0\r\n
\r\n
```

* Keepalive with chunked transfer encoding

HTTP 1.1 introduced a chunked transfer coding that defines a last-chunk bit.[10] The last-chunk bit is set at the end of each response so that the client knows where the next response begins.

However, a persistent connection with an HTTP/1.0 client cannot make use of the chunked transfer-coding, and therefore MUST use a Content-Length for marking the ending boundary of each message.

-----------

* [3.6. Transfer Codings](https://greenbytes.de/tech/webdav/rfc2616.html#transfer.codings)
* [HTTP persistent connection](https://en.wikipedia.org/wiki/HTTP_persistent_connection)
* [HTTP Streaming (or Chunked vs Store & Forward)](https://gist.github.com/CMCDragonkai/6bfade6431e9ffb7fe88)
* [Chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding)
* [Enabling nginx Chunked Transfer Encoding](https://serverfault.com/questions/159313/enabling-nginx-chunked-transfer-encoding/187573#187573)

## QUIC
[The Road to QUIC](https://blog.cloudflare.com/the-road-to-quic/)
