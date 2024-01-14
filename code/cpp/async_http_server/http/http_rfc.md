# HTTP/1.1 Protocol

### Introduction

- HTTP messages are either requests or responses.
- Servers listen for requests, interpret them, and respond with one or more response messages.
- Clients create request messages to convey specific intentions and interpret received responses.
- HTTP/1.1 request and response semantics are defined based on RFC7230 architecture.
- HTTP offers a uniform interface for interacting with resources, regardless of their type or nature.
- HTTP semantics involve request method intentions, extensions in request header fields, status codes for machine-readable responses, and control data and resource metadata in response header fields.
- The document also defines representation metadata for payload interpretation, request header fields influencing content selection, and content negotiation algorithms.

##### Resources

- HTTP defines the target of a request as a "resource."
- HTTP does not impose limits on the nature of a resource, only providing an interface for interacting with them.
- Resources are identified by Uniform Resource Identifiers (URIs) as per [RFC7230].
- When constructing an HTTP/1.1 request, the client sends the target URI in various forms, as defined in [RFC7230].
- Upon receiving a request, the server reconstructs an effective request URI for the target resource [RFC7230].
- HTTP separates resource identification from request semantics, placing request semantics in the request method and some request-modifying header fields.
- If there's a conflict between the method semantics and any implied by the URI, the method semantics take precedence [RFC7230].

##### Representation

- HTTP uses the concept of "representation" to abstractly represent the current or desired state of a resource during communication.
- A "representation" in HTTP is information that reflects a past, current, or desired state of a resource. It's conveyed in a format suitable for communication via the HTTP protocol.
- A representation comprises representation metadata and a potentially unlimited stream of representation data.
- An origin server can have multiple representations for a resource, each reflecting its current state. It selects the most appropriate representation for a given request, often using content negotiation.
- This "selected representation" is crucial for evaluating conditional requests and constructing the payload for 200 (OK) and 304 (Not Modified) responses to GET requests.

##### Representation metadata

- Representation header fields offer metadata about a representation in a message.
- They specify how to interpret data in the payload body if present.
- In a response to a HEAD request, they describe what data would have been in the payload body if it were a GET request.

<img src="./images/image-20231206220133507.png" alt="image-20231206220133507" style="zoom:80%;" />

##### Media Type

- HTTP utilizes Internet media types for content description and negotiation.
- Media types include type and subtype, followed by optional parameters.
- Parameters can be case-insensitive, and their sensitivity depends on the parameter's definition.
- Parameter values can be transmitted as tokens or within quoted-strings.
- Media types should be registered with IANA following specified procedures.
- Media type parameters do not permit white space around the "=" character.

![image-20231206220558754](./images/image-20231206220558754.png)

##### Charset

- HTTP uses charset names to indicate or negotiate the character encoding scheme of a textual representation [RFC6365]. A charset is identified by a case-insensitive token.

##### Canonicalisation and Text Defaults

- Internet media types are registered with a canonical form for interoperability.
- HTTP representations should ideally be in canonical form, similar to MIME.
- MIME's canonical form uses **CRLF** for text line breaks, but HTTP allows CR or LF if consistent.
- HTTP text media is flexible with line breaks, not restricted to specific octets (e.g., 13 and 10).
- Flexibility applies to text within a "text" media type but not "multipart" types or non-payload HTTP elements.
- If a representation is content-encoded, it should be in the specified form before encoding.

##### Content-Type

- The "Content-Type" header field in HTTP indicates the media type of the representation enclosed in the message payload.

- The media type defines both the data format and how it should be processed by the recipient, considering message semantics and content codings.

- Media types are defined in Section 3.1.1.1.

- When sending a message with a payload body, a sender should include a Content-Type header field, unless the sender doesn't know the intended media type.

- If the Content-Type header is missing, the recipient may assume a media type of **"application/octet-stream"** or inspect the data to determine its type.

- Sometimes, origin servers may not correctly configure Content-Type, leading clients to examine the payload's content and potentially override the specified type.

- This content examination can pose security risks like "privilege escalation," and implementers are encouraged to provide a way to disable content sniffing when used.

  <img src="./images/image-20231206221901749.png" alt="image-20231206221901749" style="zoom:80%;" />

##### Content-Coding

- Content coding values in HTTP indicate encoding transformations that can be applied to a representation.
- Content codings are used to compress or transform a representation without losing its underlying media type or information.
- Typically, the representation is stored in a coded form, transmitted as such, and decoded by the recipient.
- Content-coding values are case-insensitive and should be registered within the "HTTP Content Coding Registry" (Section 8.4).
- They are used in the Accept-Encoding (Section 5.3.4) and Content-Encoding (Section 3.1.2.2) header fields.
- This specification defines the following content-coding values:
  - compress (and x-compress): Refer to Section 4.2.1 of [RFC7230].
  - deflate: Refer to Section 4.2.2 of [RFC7230].
  - gzip (and x-gzip): Refer to Section 4.2.3 of [RFC7230].

##### Content-Encoding

- The "Content-Encoding" header field in HTTP specifies the encoding transformations applied to a representation beyond what's inherent in its media type.
- It indicates which decoding mechanisms are needed to obtain data in the media type mentioned in the "Content-Type" header field.
- Content-Encoding is mainly used for compressing a representation's data without losing its original media type identity.
- It's represented as a list of one or more content codings (e.g., "Content-Encoding: gzip").
- The sender must generate a Content-Encoding header that lists the content codings in the order they were applied if any encodings have been used.
- Additional encoding parameters can be specified using other header fields not defined in this specification.
- Unlike Transfer-Encoding, which relates to message transfer, Content-Encoding characterizes the representation itself, defining it in terms of its coded form.
- Typically, the representation is decoded just before rendering or similar usage.
- If the media type inherently includes encoding (e.g., always compressed data format), it may not be restated in Content-Encoding, unless applied a second time.
- Origin servers may choose to publish the same data with different coding information in Content-Type or Content-Encoding to influence user agent behavior.
- An origin server can respond with a 415 (Unsupported Media Type) status code if the representation in the request message has an unacceptable content coding.

##### Language Tags

- A language tag, as defined in RFC5646, identifies a natural human language for communication.
- It excludes computer languages.
- HTTP uses language tags in Accept-Language and Content-Language header fields.
- Accept-Language uses a broader language-range production.
- Content-Language uses the language-tag production.
- A language tag consists of case-insensitive subtags separated by hyphens.
- Typically, it starts with a primary language subtag and can have additional subtags for refinement.
- Whitespace is not allowed in a language tag.
- Examples of language tags include fr, en-US, es-419, az-Arab, x-pig-latin, man-Nkoo-GN.

##### Content-Language

- Content-Language header describes the intended audience's natural language for a representation.
- It may not include all languages used within the representation.
- Language tags are defined in Section 3.1.3.1.
- Its main purpose is to let users identify representations in their preferred language.
- If not specified, content is assumed to be intended for all language audiences.
- Multiple languages can be listed for content meant for various audiences.
- However, the presence of multiple languages in a representation doesn't necessarily mean it's for multiple audiences.
- Example: "Content-Language: da" for Danish-literate audience or "Content-Language: mi, en" for Maori and English versions.
- Example: "A First Lesson in Latin" meant for English-literate audience would have "Content-Language: en."

##### Content-Location

- The "Content-Location" header field points to a URI that identifies a specific resource corresponding to the representation in the message's payload.
- It does not replace the effective Request URI but serves as representation metadata.
- If Content-Location refers to the same URI as the effective request URI in a 2xx response, the payload is considered the current representation at the message origination date (similar to a GET request).
- For state-changing requests like PUT or POST, it indicates that the server's response contains the new representation of the resource.
- If Content-Location points to a different URI, it implies a different resource corresponding to the enclosed representation, potentially subject to content negotiation.
- In a 201 response, an identical Content-Location and Location field-value indicates the payload is a current representation of the newly created resource.
- Otherwise, Content-Location indicates the payload is a representation reporting on the requested action's status, and the same report is available for future access with GET at the given URI.
- When a user agent includes Content-Location in a request, it refers to where the user agent obtained the content of the original representation.
- An origin server receiving Content-Location in a request should treat it as transitory request context, not metadata for the representation, and must not use it to alter request semantics (e.g., to update only one of the negotiated representations).

##### Representation Data

- HTTP message representation data can be found in the message payload or referred to by the message's semantics and effective request URI.

- The format and encoding of representation data are specified by the representation metadata header fields.

- The data type of representation data is established using the Content-Type and Content-Encoding header fields.

- These header fields create a two-layer, ordered encoding model as follows: 

  `representation-data := Content-Encoding( Content-Type( bits ) )`

### Request Header Fields

![image-20231206231649830](./images/image-20231206231649830.png)

##### Expect

- 