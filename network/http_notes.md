#HTTP notes

## HTTP Headers
### Transfer Codings
```
transfer-coding: "chunked"
```

* Keepalive with chunked transfer encoding

HTTP 1.1 introduced a chunked transfer coding that defines a last-chunk bit.[10] The last-chunk bit is set at the end of each response so that the client knows where the next response begins.

However, a persistent connection with an HTTP/1.0 client cannot make use of the chunked transfer-coding, and therefore MUST use a Content-Length for marking the ending boundary of each message.

-----------

* [3.6. Transfer Codings](https://greenbytes.de/tech/webdav/rfc2616.html#transfer.codings)
* [HTTP persistent connection](https://en.wikipedia.org/wiki/HTTP_persistent_connection)


## QUIC
[The Road to QUIC](https://blog.cloudflare.com/the-road-to-quic/)
