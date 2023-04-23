# SSDP

A full documentation of SSDP to be used in a definitive SSDP RFC specification.

## Versions

| Name | Date |
| - | - |
| `draft-cai-ssdp-v1-00` | 1999-02-26 |
| `draft-cai-ssdp-v1-01` | 1999-04-08 |
| `draft-cai-ssdp-v1-02` | 1999-06-21 |
| `draft-cai-ssdp-v1-03` | 1999-10-28 |

## Messages

### `OPTIONS`

Defined in `00` and `01`, originally specified in [RFC 2068 (HTTP/1.1)](https://tools.ietf.org/html/rfc2068). Likely deprecated in favour of [`SEARCH` (`M-SEARCH`)](#search-m-search).

#### Request

 - **Method:** `OPTIONS`
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1

| Header | Requirement | Description |
| - | - | - |
| `Host` | MUST | See HTTP. |
| `Request-ID` | MUST | An ID to link request and response to eachother. |
| `Content-Length` | MAY | See HTTP. Useful when having request body. |
| `Content-Type` | MAY | See HTTP. Useful when having request body. |
| `Location` | \* | See HTTP. Send response to this location if present. |
| `Alt-Locations` | \* | Send response to one of these locations if present. |

A request body is optional.

\* Not listed in request section, but seems OPTIONAL.

#### Response

 - **Version:** HTTP/1.1
 - **Status:** 200 (OK) or 3xx (Retry on `Location` and/or `Alt-Locations` values).

| Header | Requirement | Description |
| - | - | - |
| `Request-ID` | MUST | The same values as in the request. |
| `Content-Length` | MAY | See HTTP. Useful when having response body. |
| `Content-Type` | MAY | See HTTP. Useful when having response body. |
| `Location` | SHOULD | See HTTP. Send alternative location with a 3xx status for retry. |
| `Alt-Locations` | SHOULD | Send alternative locations with a 3xx status for retry. |

A response body is optional.

When sending a response, send it with the following fallback is:
 1. `Location` from the request
 2. `Alt-Locations` from the request
 3. UDP Sender of the request

### `ANNOUNCE`

Defined in `00` and `01`, originally specified in [RFC 2326 (RTSP/1.1)](https://tools.ietf.org/html/rfc2326). Likely deprecated in favour of [`NOTIFY`](#notify).

#### Request

 - **Method:** `ANNOUNCE`
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1

| Header | Requirement | Description |
| - | - | - |
| `Host` | UNKNOWN\* | See HTTP. |
| `Content-Length` | MAY | See HTTP. Useful when having request body. |
| `Content-Type` | MAY | See HTTP. Useful when having request body. |
| `Location` | SHOULD | See HTTP. This URI will be announced as protocol endpoint. |
| `Alt-Locations` | SHOULD\*\* | This URIs will be announced as protocol endpoint. |

A request body is optional.

\* The `Host` header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

\*\* Only `SHOULD` if the service provider has multiple URIs.

#### Response

No response.

### `SUBSCRIBE`

Defined in `02`, originally specified in [draft-cohen-gena-client-00](https://tools.ietf.org/html/draft-cohen-gena-client-00). Likely deprecated.

#### Request

 - **Method:** `SUBSCRIBE`
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1

| Header | Requirement | Description |
| - | - | - |
| `Host` | UNKNOWN\* | See HTTP. |
| `NT` | "is to be" | Some <ins>**N**</ins>otification <ins>**T**</ins>ype. |
| `Callback` | UNKNOWN\* | Some callback. |
| `Scope` | UNKNOWN\* | Some scope. |
| `Timeout` | UNKNOWN\* | Some timeout. |

\* This header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

#### Response

 - **Version:** HTTP/1.1
 - **Status:** 200 (OK)

| Header | Requirement | Description |
| - | - | - |
| `Subscription-ID` | UNKNOWN\* | The id of the subscription. |
| `Timeout` | UNKNOWN\* | Some timeout. |

\* This header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

### `SSDPC`

Defined in `02`. Likely deprecated.

#### Request

 - **Method:** `SSDPC`
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1

| Header | Requirement | Description |
| - | - | - |
| `Host` | UNKNOWN\* | See HTTP. |
| `PN` | MUST | Some <ins>**P**</ins>roxy <ins>**N**</ins>umber. |
| `USN` | MUST |Some <ins>**U**</ins>nique <ins>**S**</ins>ervice <ins>**N**</ins>ame. |

\* The `Host` header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

#### Response

No response.

### `SEARCH` (`M-SEARCH`)

Defined in `02` and `03`, originally specified in [RFC 5323 (WebDAV SEARCH)](https://tools.ietf.org/html/rfc5323), formerly DASL (DAV Searching and Locating).

#### Request

 - **Method:** `M-SEARCH` (mandatory `SEARCH`)
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1
 
 | Header | Requirement | Description |
| - | - | - |
| `Host` | UNKNOWN\* | See HTTP. |
| `Man` | ? | Mandatory extension. |
| `ST` | MUST | Some <ins>**S**</ins>ervice <ins>**T**</ins>ype. |
| `MM` | ? | <ins>**M**</ins>ini<ins>**m**</ins>um number of seconds a service must wait before it sends a response |
| `MX` | ? | <ins>**M**</ins>a<ins>**x**</ins>imum number of seconds a service must wait before it sends a response |
| `S` | ? | Unknown, but seems to be some id. |

A request body is optional.

\* This header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

#### Response

 - **Version:** HTTP/1.1
 - **Status:** 200 (OK) or 207 (Multi-status)
 
| Header | Requirement | Description |
| - | - | - |
| `ST` | MUST | Some <ins>**S**</ins>ervice <ins>**T**</ins>ype. |
| `USN` | MUST | Some <ins>**U**</ins>nique <ins>**S**</ins>ervice <ins>**N**</ins>ame. |
| `Location` | SHOULD | Some location. |
| `AL` | SHOULD | Some <ins>**A**</ins>lternative <ins>**L**</ins>ocations. |
| `Content-Length` | ? | See HTTP. Useful when having response body. |
| `Content-Type` | ? | See HTTP. Useful when having response body. |
| `S` | ? | Unknown, but seems to be some id. |
| `Ext` | ? | Unknown. |
| `Cache-Control` | SHOULD | If caching data. |
| `Expires` | SHOULD | If caching data. |

A response body is optional.

### `NOTIFY`

Defined in `02` and `03`.

#### Request

 - **Method:** `NOTIFY`
 - **Request URI:** `...TODO...`
 - **Version:** HTTP/1.1
 
 | Header | Requirement | Description |
| - | - | - |
| `Host` | UNKNOWN\* | See HTTP. |
| `NT` | MUST | Some <ins>**N**</ins>otification <ins>**T**</ins>ype. |
| `NTS` | ? | Some <ins>**N**</ins>otification <ins>**T**</ins>ype <ins>**S**</ins>ub. |
| `USN` | MUST | Some <ins>**U**</ins>nique <ins>**S**</ins>ervice <ins>**N**</ins>ame. |
| `AL` | SHOULD | Some <ins>**A**</ins>lternative <ins>**L**</ins>ocations. |
| `Location` | SHOULD | Some location. |
| `Cache-Control` | SHOULD | If caching data. |
| `PN` | MUST (if proxy) | Some <ins>**P**</ins>roxy <ins>**N**</ins>umber. |

A request body is optional.

\* This header is only visible in the example, but nowhere in the text. It is unknown if it is required or optional.

#### Response

No response, when over HTTPU.