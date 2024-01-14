

# HTTP Summary

> https://httpwg.org/specs/rfc9110.html

## Headers

### General Headers

These headers apply to both requests and responses but do not relate to the data in the body. 

- **Cache-Control**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control

  - The **`Cache-Control`** HTTP header field holds *directives* (instructions) — in both requests and responses — that control [caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) in browsers and shared caches (e.g. Proxies, CDNs).

- **Connection** ==PROHIBITED IN http/2 & http/3==

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Connection

  - The **`Connection`** general header controls whether the network connection stays open after the current transaction finishes. If the value sent is `keep-alive`, the connection is persistent and not closed, allowing for subsequent requests to the same server to be done.

- **Date**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Date

  - The **`Date`** general HTTP header contains the date and time at which the message originated.

    ```http
    Date: <day-name>, <day> <month> <year> <hour>:<minute>:<second> GMT
    ```

- **Pragma** ==DEPRECATED== 

- **Trailer**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Trailer

  - The **Trailer** response header allows the sender to include additional fields at the end of chunked messages in order to supply metadata that might be dynamically generated while the message body is sent, such as a message integrity check, digital signature, or post-processing status.

    ```http
    Trailer: header-names
    ```

- **Transfer-Encoding** ==HTTP2 disallows this, other than the http2 specific trailers== 

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding

  - The **`Transfer-Encoding`** header specifies the form of encoding used to safely transfer the [payload body](https://developer.mozilla.org/en-US/docs/Glossary/Payload_body) to the user.
  - `Transfer-Encoding` is a [hop-by-hop header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#hop-by-hop_headers), that is applied to a message between two nodes, not to a resource itself. Each segment of a multi-node connection can use different `Transfer-Encoding` values. If you want to compress data over the whole connection, use the end-to-end [`Content-Encoding`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding) header instead.

- **Upgrade** ==HTTP2 explicitly disallows this==

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Upgrade

  - The HTTP 1.1 (only) `Upgrade` header can be used to upgrade an already established client/server connection to a different protocol (over the same transport protocol). For example, it can be used by a client to upgrade a connection from HTTP 1.1 to HTTP 2.0, or an HTTP or HTTPS connection into a WebSocket.

    ```http
    GET /index.html HTTP/1.1
    Host: www.example.com
    Connection: upgrade
    Upgrade: example/1, foo/2
    ```

- **Via**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Via

  - The **`Via`** general header is added by proxies, both forward and reverse, and can appear in the request or response headers. It is used for tracking message forwards, avoiding request loops, and identifying the protocol capabilities of senders along the request/response chain.

- **Warning** ==DEPRECATED==

### Request Headers

These headers contain more information about the resource to be fetched or about the client itself.

- **Accept**

  - The **`Accept`** request HTTP header indicates which content types, expressed as [MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types), the client is able to understand. The server uses [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) to select one of the proposals and informs the client of the choice with the [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) response header. Browsers set required values for this header based on  the context of the request. For example, a browser uses different values in a request when fetching a CSS stylesheet, image, video, or a script.

    ```http
    Accept: <MIME_type>/<MIME_subtype>
    Accept: <MIME_type>/*
    Accept: */*
    
    // Multiple types, weighted with the quality value syntax:
    Accept: text/html, application/xhtml+xml, application/xml;q=0.9, image/webp, */*;q=0.8
    ```

- **Accept-Charset** ==IGNORE== 

- **Accept-Encoding**

  - The **`Accept-Encoding`** request HTTP header  indicates the content encoding (usually a compression algorithm) that  the client can understand. The server uses [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) to select one of the proposals and informs the client of that choice with the [`Content-Encoding`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding) response header.

    ```http
    Accept-Encoding: gzip
    Accept-Encoding: compress
    Accept-Encoding: deflate
    Accept-Encoding: br
    Accept-Encoding: identity
    Accept-Encoding: *
    
    // Multiple algorithms, weighted with the quality value syntax:
    Accept-Encoding: deflate, gzip;q=1.0, *;q=0.5
    ```

- **Accept-Language**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language

  - The **`Accept-Language`** request HTTP header indicates the natural language and locale that the client prefers. The server uses [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) to select one of the proposals and informs the client of the choice with the [`Content-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language) response header. Browsers set required values for this header according to their active user interface language. Users rarely change it, and such changes are not recommended because they may lead to [fingerprinting](https://developer.mozilla.org/en-US/docs/Glossary/Fingerprinting).

  ```http
  Accept-Language: <language>
  Accept-Language: *
  
  // Multiple types, weighted with the quality value syntax:
  Accept-Language: fr-CH, fr;q=0.9, en;q=0.8, de;q=0.7, *;q=0.5
  ```

- **Authorization**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization

  - The HTTP **`Authorization`** request header can be used to provide credentials that authenticate a user agent with a server, allowing access to a protected resource.

    ```http
    Authorization: <auth-scheme> <authorization-parameters>
    ```

- **Cookie**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cookie

  - The **`Cookie`** HTTP request header contains stored [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) associated with the server (i.e. previously sent by the server with the [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) header or set in JavaScript using [`Document.cookie`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)).

- **Expect**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expect

  - The **`Expect`** HTTP request header indicates expectations that need to be met by the server to handle the request successfully.

    ```http
    Expect: 100-continue
    ```

- **From**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/From

  - The **`From`** request header contains an Internet email address for a human user who controls the requesting user agent.

  - If you are running a robotic user agent (e.g. a crawler), the `From` header must be sent, so you can be contacted if problems occur on servers, such as if the robot is sending excessive, unwanted, or invalid requests.

    ```http
    From: <email>
    ```

- **Host**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Host

  - The **`Host`** request header specifies the host and port number of the server to which the request is being sent.

    ```http
    Host: <host>:<port>
    ```

- If-Match

- If-Modified-Since

- If-None-Match

- If-Range

- If-Unmodified-Since

- **Max-Forwards**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Max-Forwards

  - https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Max-Forwards

- Proxy-Authorization

- Range

- **Referer**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer

  - The **`Referer`** HTTP request header contains the absolute or partial address from which a resource has been requested. The `Referer` header allows a server to identify referring pages that people are visiting from or where requested resources are being used. This data can be used for analytics, logging, optimised caching, and more.

- **TE** ==HTTP2, TE only accepted if the trailers value is set.==

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/TE

  - The **`TE`** request header specifies the transfer encodings the user agent is willing to accept. (you could informally call it `Accept-Transfer-Encoding`, which would be more intuitive).

- **User-Agent**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent

  - The **User-Agent** [request header](https://developer.mozilla.org/en-US/docs/Glossary/Request_header) is a characteristic string that lets servers and network peers identify the application, operating system, vendor, and/or version of the requesting [user agent](https://developer.mozilla.org/en-US/docs/Glossary/User_agent).

    ```http
    User-Agent: Mozilla/5.0 (<system-information>) <platform> (<platform-details>) <extensions>
    ```

### Response Headers

These headers hold additional information about the server's response.

- **Accept-Ranges**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Ranges

  - The **`Accept-Ranges`** HTTP response header is a marker used  by the server to advertise its support for partial requests from the client for file downloads. The value of this field  indicates the unit that can be used to define a range.
  - Basically partial downloads.

- **Age**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Age

  - The **`Age`** header contains the time in seconds the object was in a proxy cache.

- **ETag**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag

  - The **`ETag`** (or **entity tag**) HTTP response header is an identifier for a  specific version of a resource. It lets caches be more efficient and save bandwidth, as  a web server does not need to resend a full response if the content was not changed.  Additionally, etags help to prevent simultaneous updates of a resource from overwriting  each other (["mid-air collisions"](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#avoiding_mid-air_collisions)).

- **Location**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Location

  - The **`Location`** response header indicates the URL to  redirect a page to. It only provides a meaning when served with a  `3xx` (redirection) or `201` (created) status response.

- **Proxy-Authenticate**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Proxy-Authenticate

  - The HTTP **`Proxy-Authenticate`** response header defines the  authentication method that should be used to gain access to a resource behind a  [proxy server](https://developer.mozilla.org/en-US/docs/Glossary/Proxy_server). It authenticates the request to the proxy server, allowing  it to transmit the request further.

- **Retry-After**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After

  - The **`Retry-After`** response HTTP header indicates how long  the user agent should wait before making a follow-up request. There are three main cases  this header is used:

- **Server**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server

  - The **`Server`** header describes the  software used by the origin server that handled the request — that is, the server that  generated the response.

- **Set-Cookie** ==Browsers block fronted js from accessing this header==

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie

  - The **`Set-Cookie`** HTTP response header is  used to send a cookie from the server to the user agent, so that the  user agent can send it back to the server later.  To send multiple cookies, multiple **`Set-Cookie`** headers should be sent in the same response.

    ```http
    Set-Cookie: <cookie-name>=<cookie-value>
    ```

- Vary

- **WWW-Authenticate**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/WWW-Authenticate

  - The HTTP **`WWW-Authenticate`** response header defines the [HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication) methods ("challenges") that might be used to gain access to a specific resource.

### Entity Headers

These headers provide information about the body of the resource, like its length or MIME type.

- **Allow**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Allow

  - The **`Allow`** header lists the set of methods supported by a resource.

    ```http
    Allow: GET, POST, HEAD
    ```

- **Content-Encoding**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding

  - The **`Content-Encoding`** [representation header](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) lists any encodings that have been applied to the representation (message payload), and in what order.  This lets the recipient know how to decode the representation in order to obtain the original payload format.  Content encoding is mainly used to compress the message data without losing information about the origin media type.

    ```http
    Content-Encoding: gzip // servers should recognise x-gzip as an alias
    Content-Encoding: compress
    Content-Encoding: deflate
    Content-Encoding: br
    
    // Multiple, in the order in which they were applied
    Content-Encoding: deflate, gzip
    ```

- **Content-Language**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language

  - The **`Content-Language`** [representation header](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) is used to **describe the language(s) intended for the audience**, so users can differentiate it according to their own preferred language.

    ```http
    Content-Language: de-DE
    Content-Language: en-US
    Content-Language: de-DE, en-CA
    ```

- **Content-Length**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length

  - The **`Content-Length`** header indicates the **size of the message body**, in bytes, sent to the recipient.

    ```http
    Content-Length: <length> // length in decimal number of octets (8 bit bytes)
    ```

- **Content-Location**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location

  - The **`Content-Location`** header indicates an alternate location for the returned data. The principal use is to indicate the URL of a resource transmitted as the result of [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation).

    ```http
    Content-Location: <url>
    ```

- Content-MD5

- Content-Range

- **Content-Type**

- **Expires** 

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires

  - The **`Expires`** HTTP header contains the date/time after which the  response is considered expired.
  - ==**Note:** If there is a [`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) header    with the `max-age` or `s-maxage` directive in the response,    the `Expires` header is ignored.==

- Last-Modified

### WebSockets

Specific to the WebSocket protocol.

- Sec-WebSocket-Accept
- Sec-WebSocket-Extensions
- Sec-WebSocket-Key
- Sec-WebSocket-Protocol
- Sec-WebSocket-Version

### Security-related Headers

These headers are used to enforce security policies.

- Content-Security-Policy
- Strict-Transport-Security
- X-Content-Type-Options
- X-Frame-Options
- X-XSS-Protection
- X-Powered-By
- X-UA-Compatible

### CORS (Cross-Origin Resource Sharing) Headers

These headers are used in controlling access for resources from different origins.

- **Access-Control-Allow-Origin**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin

  - The **`Access-Control-Allow-Origin`** response header indicates whether the response can be shared with requesting code from the given [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin).

    ```http
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Origin: <origin>
    Access-Control-Allow-Origin: null
    ```

- **Access-Control-Allow-Credentials**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials

  - The **`Access-Control-Allow-Credentials`** response header tells browsers whether to expose the response to the frontend JavaScript code when the  request's credentials mode ([`Request.credentials`](https://developer.mozilla.org/en-US/docs/Web/API/Request/credentials)) is `include`.

- **Access-Control-Allow-Headers**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Headers

  - The **`Access-Control-Allow-Headers`** response header is used in response to a [preflight request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) which includes the [`Access-Control-Request-Headers`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Request-Headers) to indicate which HTTP headers can be used during the actual request.

- **Access-Control-Allow-Methods**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Methods

  - The **`Access-Control-Allow-Methods`** response header  specifies one or more methods allowed when accessing a resource in response to a  [preflight request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request).

- Access-Control-Expose-Headers

- Access-Control-Max-Age

- Access-Control-Request-Headers

- Access-Control-Request-Method

- **Origin**

  > https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin

  - The **`Origin`** request header indicates the [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin) (scheme, hostname, and port) that *caused* the request. For example, if a user agent needs to request resources included in a page, or fetched by scripts that it executes, then the origin of the page may be included in the request.

### Miscellaneous

- Link
- Refresh
- Content-Disposition

## HTTP Semantics

### MIME Types

> https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types

- A **media type** (also known as a **Multipurpose Internet Mail Extensions or MIME type**) indicates the nature and format of a document, file, or assortment of bytes.  MIME types are defined and standardised in IETF's [RFC 6838](https://datatracker.ietf.org/doc/html/rfc6838).

### Content Negotiation

> https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation

- In [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP), **content negotiation** is the mechanism that is used for serving different [representations](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) of a resource to the same URI to help the user agent specify which  representation is best suited for the user (for example, which document  language, which image format, or which content encoding).

### Representation Header

> https://developer.mozilla.org/en-US/docs/Glossary/Representation_header

- A **representation header** is an [HTTP header](https://developer.mozilla.org/en-US/docs/Glossary/HTTP_header) that describes the particular *representation* of the resource sent in an HTTP message body.

- Representations are different forms of a particular resource. For example, the same data might be formatted as a particular media type such as XML or JSON, localized to a particular written language or geographical region, and/or compressed or otherwise encoded for transmission. The underlying resource is the same in each case, but its representation is different.

## Misc

### Hop-by-hop headers

- These headers are meaningful only for a single transport-level connection, and *must not* be retransmitted by proxies or cached. Note that only hop-by-hop headers may be set using the [`Connection`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Connection) header.

### Preflight request

>  https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request

- A CORS preflight request is a [CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS) request that checks to see if the CORS protocol is understood and a server is aware using specific methods and headers.