# HTTP Status Codes

### **I. Core Definition and Communication Derivation Logic**

* **Core definition**: An HTTP status code is the **3-digit decimal numeric code** returned by the server on the first line of the response message after the client sends an HTTP request, used to report the request's execution status and follow-up handling instructions in real time.
* **Encoding derivation logic**:
  `Client sends request` $\rightarrow$ `Server parses and routes` $\rightarrow$ `Returns status code (1xx-5xx) + response headers/body` $\rightarrow$ `Client takes the next action based on the first-digit class (continue sending / render data / jump to a new address / fix the request / retry and alert)`.
* **Key classification takeaways**:
  * **1xx (Informational)**: The request has been received and processing continues (temporary response).
  * **2xx (Success)**: The request has been successfully received, understood, and accepted.
  * **3xx (Redirection)**: Further client action (redirect / read cache) is required to complete the request.
  * **4xx (Client Error)**: The request contains a syntax error, missing authentication, or an illegal client call; the responsibility lies with the client.
  * **5xx (Server Error)**: The request is valid, but the server's internal execution, gateway forwarding, or load handling hit an anomaly; the responsibility lies with the server.

---

### **II. 1xx Informational Responses — Transport Negotiation Phase**

Core logic: The TCP connection is established and the request headers have been received; the server instructs the client to continue sending the request body or to switch the communication protocol.

* **100 Continue**: Request headers validated $\rightarrow$ the client may continue sending a large request body (paired with the `Expect: 100-continue` request header to save bandwidth).
* **101 Switching Protocols**: The server agrees to the client's `Upgrade` request header $\rightarrow$ regular HTTP is dropped and the connection is upgraded in place to a more advanced or long-lived protocol (e.g., upgrading from HTTP/1.1 to WebSocket).
* **102 Processing**: WebDAV extension; the server is working through a time-consuming subtask, preventing the client from judging a timeout and disconnecting after a long silence.
* **103 Early Hints**: Before the final response is ready, a provisional response containing `Link` headers is returned early $\rightarrow$ the browser preloads CSS/JS static assets, optimizing first-screen rendering performance.

---

### **III. 2xx Success Responses — Normal Request Completion**

Core logic: The client's request has been legitimately received and parsed by the server, and the business logic has completed.

* **200 OK**: Standard success response; a GET request returns the target resource entity, and a POST request returns a description of the processing result.
* **201 Created**: Synchronous write operation succeeded; commonly used for POST/PUT requests — the server has generated a new resource and returns its URI in the `Location` response header.
* **202 Accepted**: Asynchronous processing mode; the request has entered a background task queue but has not finished executing (no immediate synchronous result).
* **203 Non-Authoritative Information**: The request succeeded, but the response metadata comes from a proxy or cached copy rather than the origin server.
* **204 No Content**: Processing succeeded but the response body is empty; suited to operations that only need to confirm success without refreshing the page view (e.g., a successful DELETE, or a save-status heartbeat).
* **205 Reset Content**: Processing succeeded with no response body; the client is instructed to reset the current document view (e.g., clearing form input fields).
* **206 Partial Content**: Range request succeeded; returns the requested byte-range fragment in combination with the client's `Range` request header, supporting chunked downloads of large files, video seeking, and resumable transfers.
* **207 Multi-Status**: WebDAV extension; the response body of a batch operation is XML wrapping the different status codes of multiple independent sub-requests.
* **208 Already Reported**: WebDAV extension; members already bound to a collection were enumerated in a preceding response, avoiding duplicated output that would cause an infinite loop.
* **226 IM Used**: The server returns only the incremental delta against the current instance (Delta Encoding), saving the bandwidth of a full transfer.

---

### **IV. 3xx Redirection — Path Relocation and Cache Hits**

Core logic: The target resource has moved or the local cache is still valid, guiding the client to send the request to a new URI or to read the local copy directly.

| Status Code | Name | Core Mechanism and Key Distinctions (Method Preservation & Caching Strategy) |
| :--- | :--- | :--- |
| **300** | Multiple Choices | Multiple candidate resources exist (e.g., multiple languages/formats); a list is returned for the client or user to choose from. |
| **301** | Moved Permanently | **Permanent redirect**; the old URI is retired and search engine weight transfers to the new address in `Location`; legacy implementations often convert POST to GET. |
| **302** | Found | **Temporary redirect**; the resource has moved temporarily and the original URI remains valid; legacy browsers often downgrade POST to GET. |
| **303** | See Other | Explicitly requires the client to switch to the **GET** method when accessing the new URI (commonly used to jump to a result page after a POST form submission, preventing duplicate submissions). |
| **304** | Not Modified | **Negotiation caching hit**; paired with `If-Modified-Since` / `If-None-Match`, the resource is unmodified, no response body is returned, and the client reads its local cache directly. |
| **307** | Temporary Redirect | **Strict temporary redirect**; explicitly forbids changing the original HTTP request method and request body (a POST stays POST after the redirect). |
| **308** | Permanent Redirect | **Strict permanent redirect**; permanent relocation with the original HTTP request method and request body strictly preserved (the strict method-preserving version of 301). |

---

### **V. 4xx Client Errors — Request-Side Validation Rejections**

Core logic: The client constructed parameters incorrectly, its identity or permissions do not match, or it exceeded request policy limits; the server refuses to execute.

#### **1. Syntax, Addressing, and Authentication (High-Frequency Exam Material)**
* **400 Bad Request**: The request message has a syntax error, invalid JSON format, or failed required-field validation, so the server cannot parse it.
* **401 Unauthorized**: Missing valid identity credentials (Token/Cookie absent or expired); the response must include the `WWW-Authenticate` header, and the request can be retried once authentication is supplied.
* **402 Payment Required**: A protocol-reserved status code, used for digital payments, API paywalls, or anti-abuse rate-limit blocks.
* **403 Forbidden**: **Insufficient permissions**; the server understood the request and knows who the user is, but that role has no right to access the target resource (logging in again will not help).
* **404 Not Found**: The target URI resource does not exist on the server, or the server deliberately hides the existence of an unauthorized resource; it does not distinguish temporary from permanent disappearance.
* **410 Gone**: The resource has been **permanently removed** with no forwarding address; it explicitly tells clients and search engines to clean up the dead link (more absolute in meaning than 404).

#### **2. Protocol Rules, Methods, and Header Constraints**
* **405 Method Not Allowed**: The target resource does not support the current HTTP method (e.g., sending DELETE to a read-only endpoint); the response must include an `Allow` header listing the valid methods.
* **406 Not Acceptable**: Server-side content negotiation failed; the server cannot generate a resource format satisfying the client's `Accept-*` request headers.
* **407 Proxy Authentication Required**: Same logic as 401, but the authenticating party is an intermediate proxy server rather than the origin.
* **408 Request Timeout**: The client connection sat idle or sent the message too slowly; the server timed out waiting and closed the connection.
* **409 Conflict**: The request conflicts with the current state of the target resource on the server (e.g., version conflicts from multiple users concurrently editing the same document, or duplicate unique keys).
* **411 Length Required**: The server refuses to accept messages that do not carry an explicit `Content-Length` request header.
* **412 Precondition Failed**: A precondition set in the request headers (e.g., `If-Match` version checking) evaluated to false on the server, blocking an overwriting update.
* **413 Content Too Large**: The uploaded data payload exceeds the server's per-request receiving limit (may be accompanied by `Retry-After`).
* **414 URI Too Long**: The URL string exceeds the server's parsing buffer limit (commonly caused by improperly stuffing large numbers of query parameters into a GET request).
* **415 Unsupported Media Type**: The request body format (`Content-Type`) is not supported by the target endpoint (e.g., sending `text/plain` when `application/json` is required).
* **416 Range Not Satisfiable**: The `Range` byte interval in a partial request exceeds the actual total size of the target file.
* **417 Expectation Failed**: The server cannot fulfill the expected behavior specified in the request's `Expect` header.

#### **3. Semantic Validation, Flow Control, and Compliance Extensions**
* **418 I'm a teapot**: An Easter egg from RFC 2324, the Hyper Text Coffee Pot Control Protocol (refusing to brew coffee with a teapot); now kept as a protocol-standard placeholder or a playful block code.
* **421 Misdirected Request**: Under HTTP/2 connection reuse, the request was routed to a server unable to produce a response for that domain.
* **422 Unprocessable Content**: The request syntax and format are perfectly valid (not a 400), but internal business-semantic validation failed (e.g., a negative value in an age field).
* **423 Locked** / **424 Failed Dependency**: WebDAV extensions; the target resource is held by a concurrent lock, or a preceding dependent operation failed, causing the current operation to be canceled along with it.
* **425 Too Early**: The server refuses to process TLS early data (0-RTT Early Data) that could be vulnerable to replay attacks.
* **426 Upgrade Required**: The server rejects the current protocol version and requires the client to upgrade the protocol per the `Upgrade` header before communicating.
* **428 Precondition Required**: Forces the request to carry conditional headers (e.g., `If-Match`) to prevent the "Lost Update" concurrency conflict.
* **429 Too Many Requests**: **API rate limiting** triggered; the client's request frequency per unit time exceeded the limit, with `Retry-After` indicating the cooldown period.
* **431 Request Header Fields Too Large**: A single request header or the total size of all request headers exceeded the limit (commonly caused by endlessly growing Cookies).
* **451 Unavailable For Legal Reasons**: A tribute to the novel *Fahrenheit 451*; access to the resource is blocked due to government regulation, copyright complaints, or legal-compliance bans.

---

### **VI. 5xx Server Errors — Backend and Architecture Failures**

Core logic: The client's request is entirely valid, but the response fails due to server code exceptions, broken upstream/downstream gateway communication, or hardware overload.

* **500 Internal Server Error**: The generic catch-all error; server code threw an uncaught exception (e.g., a null pointer, a crashed database connection) and no more specific status code matches.
* **501 Not Implemented**: The server does not support the functionality or HTTP method required by the request (a global capability gap, as opposed to the resource-specific 405).
* **502 Bad Gateway**: **Upstream/downstream communication failure**; a server acting as a reverse proxy or gateway (e.g., Nginx) received an invalid response from the upstream application server or had its connection reset.
* **503 Service Unavailable**: The server is **temporarily overloaded** or **down for maintenance**; a temporary condition — it may return `Retry-After` to guide the client to retry later.
* **504 Gateway Timeout**: A server acting as a gateway or proxy failed to receive a response from the upstream server (or database/microservice) within the allotted time.
* **505 HTTP Version Not Supported**: The server refuses to support the major HTTP protocol version used in the request message.
* **506 Variant Also Negotiates**: Misconfigured transparent content negotiation on the server causes an internal dead-loop of resource references.
* **507 Insufficient Storage**: WebDAV extension; the server's disk or quota is exhausted and it cannot persist the state required by the request.
* **508 Loop Detected**: WebDAV extension; an infinitely recursive reference chain was discovered while processing the request.
* **510 Not Extended**: The request is missing the extension policy declaration that the server mandates for accessing the resource.
* **511 Network Authentication Required**: The client is on a controlled network (e.g., a captive portal on public Wi-Fi) and must complete network-level login authentication before accessing the outside internet.

---

### **VII. Quick Comparison of Commonly Confused Status Codes (Core Review Takeaways)**

* **301 vs 302 vs 307 vs 308 (redirect selection)**:
  * Permanent or temporary move: choose **301 / 308** for permanent (updates bookmarks and SEO weight); **302 / 307** for temporary.
  * Whether the original request method is locked: choose **301 / 302** if POST may become GET; **307 / 308** if the request method and payload must never change; **303** to force conversion to GET.
* **400 vs 422 (parameter validation layers)**:
  * **400**: Syntax-level parsing failure (missing JSON brackets, mismatched field types).
  * **422**: Syntax parsing succeeded but business-rule validation failed (invalid email format, insufficient balance).
* **401 vs 403 (the two stages of auth)**:
  * **401 (Authentication)**: "Who are you?" — not logged in or credentials expired; solvable by logging in.
  * **403 (Authorization)**: "Are you allowed?" — identity recognized but permission level insufficient; unsolvable until switching to a higher-privilege account.
* **404 vs 410 (nature of the missing resource)**:
  * **404**: Not found right now; status unknown (could be a typo or a temporary takedown).
  * **410**: Confirmed permanently and thoroughly deleted; the client is expected to erase historical links outright.
* **502 vs 503 vs 504 (gateway and load troubleshooting)**:
  * **502**: The upstream process crashed or returned garbage (cannot connect / wrong response).
  * **503**: The service is actively rate-limiting for protection or down for maintenance (overwhelmed / maintenance sign hung up).
  * **504**: The upstream executed too slowly and missed the response threshold (waited too long, timed out).
