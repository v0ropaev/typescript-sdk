---
'@modelcontextprotocol/server-legacy': patch
---

`SSEServerTransport` now sends the message endpoint in the form it was configured with. It took `new URL(endpoint, dummyBase).pathname`, which turns `'messages'` into `'/messages'`, so every endpoint went out as an absolute path. A client resolves that against the URL it opened the stream on, and an absolute-path reference replaces that URL's whole path (RFC 3986 §5.3), so a server reached through a prefix it does not know about, for example `https://host/subpath/sse` behind a proxy, had its POSTs sent to `https://host/messages`. Configuring a path-relative endpoint is the only way the server can name the endpoint without knowing the prefix, and until now it had no effect. An endpoint given as an absolute path or as a full URL is unchanged.
