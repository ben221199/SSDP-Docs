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

Defined in `00` and `01`. Likely deprecated in favour of [`SEARCH` (`M-SEARCH`)](#search-m-search).

#### Request

 - **Method:** OPTIONS
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
 1. `Location`
 2. `Alt-Locations`
 3. UDP Sender

### `ANNOUNCE`

Defined in `00` and `01`. Likely deprecated.

### `SUBSCRIBE`

Defined in `02`. Likely deprecated.

### `SSDPC`

Defined in `02`. Likely deprecated.

### `SEARCH` (`M-SEARCH`)

Defined in `02` and `03`.

### `NOTIFY`

Defined in `02` and `03`.