# Specification for Beatmap Visualization Server Protocol

<!-- TOC depthFrom:2 orderedList:false -->

- [Overview](#overview)
- [Base Protocol](#base-protocol)
    - [Header Part](#header-part)
    - [Content Part](#content-part)
- [Base JSON Structures](#base-json-structures)
    - [`Message`](#message)
    - [`RequestMessage`](#requestmessage)
    - [`ResponseMessage`](#responsemessage)
    - [Notification Message](#notification-message)
    - [Specific Requests and Notifications](#specific-requests-and-notifications)
- [Methods](#methods)
    - [`simInitialize`](#siminitialize)
    - [`simInitialized`](#siminitialized)
    - [`simExit`](#simexit)
    - [`simExited`](#simexited)
    - [`edExit`](#edexit)

<!-- /TOC -->

## Overview

TBD

Unlike LSP, relations of editors and simulators in BVSP are more closed to peer-to-peer.

Like debugging software, simulators can run in standalone mode, or be launched by editors. In the first case, there is no need to connect to an editor. In the second case, the editor always starts a server, the simulator connects to it, then starts its own server: the editor-simulator communication works in a duplex way.

TBD: life cycle graph of an editor and a simulator here.

## Base Protocol

Similar to LSP, BVSP is based on [JSON-RPC 2.0 protocol](http://www.jsonrpc.org/specification).

### Header Part

Header part is encoded in ASCII.

| Header Field Name | Value Type | Description |
|---|---|---|
| Content-Length | number | The length of the content part in bytes. This header is required. |
| Content-Type | string | The MIME type of the content part. Defaults to `application/jsonrpc-bvsp; charset=utf-8` |

### Content Part

The encoding of content part is specified in `Content-Type` header. Currently UTF-8 is the only supported encoding. As LSP suggests, you should use `utf-8` instead of `utf8`.

## Base JSON Structures

### `Message`

General message is defined in JSON-RPC as below:

```ts
interface Message {
    jsonrpc: string;
}
```

### `RequestMessage`

Message for requests:

```ts
interface RequestMessage extends Message {
    id: number | string | null;
    method: string;
    params?: any[] | object;
}
```

Examples:

```json
{
    "jsonrpc": "2.0",
    "method": "method1",
    "params": [{}],
    "id": null
}

[{
    "jsonrpc": "2.0",
    "method": "method2",
    "params": null,
    "id": "id2"
}]
```

### `ResponseMessage`

Message for responses:

```ts
interface ResponseMessage extends Message {
    id: number | string | null;
    result?: any;
    error?: ResponseError<any>;
}
```

Particularly, the actual type of the response corresponds to the request. The response will be an array if the request is an array, and element orders are maintained.

The `result` field may be omitted if an error ocurred. In this case, the `error` field will have a value to describe the error.

`ResponseError` is defined as:

```ts
interface ResponseError<TData> {
    code: number;
    message: string;
    data?: TData;
}
```

Standard error codes:

```ts
export namespace ErrorCodes {
    // Defined by JSON RPC
    export const ParseError: number = -32700;
    export const InvalidRequest: number = -32600;
    export const MethodNotFound: number = -32601;
    export const InvalidParams: number = -32602;
    export const InternalError: number = -32603;
    export const serverErrorStart: number = -32099;
    export const serverErrorEnd: number = -32000;
    export const ServerNotInitialized: number = -32002;
    export const UnknownErrorCode: number = -32001;
}
```

Examples:

```json
{
   "jsonrpc": "2.0",
   "result": {
       "value1": 0
   },
   "id": null
}

[{
   "jsonrpc": "2.0",
   "result": [{
       "value2a": 1
   }, {
       "value2b": 2
   }],
   "id": "id2"
}]

[{
   "jsonrpc": "2.0",
   "error": {
       "code": -32001,
       "message": "A New Error"
   },
   "id": "id2"
}]
```

### Notification Message

```ts
interface NotificationMessage extends Message {
    method: string;
    params?: any[] | object;
}
```

### Specific Requests and Notifications

Requests and notifications whose methods start with `$/` are messages which are protocol implementation dependent and might not be implementable in all clients or servers. For example, asynchronous actions (LSP's `$/cancelRequest`), game-specific methods (`$/game/cgss/preview/gotoMeasure`), etc.

## Methods

"S" stands for simulators; "E" stands for editors.

| Name | Type | Category | Direction | Description |
|---|---|---|---|---|
| `general/simInitialize` | Request | General | S→E→S | TBD |
| `general/simInitialized` | Notification | General | S→E | TBD |
| `general/simExit` | Request | General | S→E→S | TBD |
| `general/simExited` | Notification | General | S→E | TBD |
| `general/edExit` | Request | General | E→S→E | TBD |
| `preview/play` | Request | Previewing | E→S→E | TBD |
| `preview/playing` | Notification | Previewing | S→E | TBD |
| `preview/tick` | Notification | Previewing | S→E | TBD |
| `preview/pause` | Request | Previewing | E→S→E | TBD |
| `preview/paused` | Notification | Previewing | S→E | TBD |
| `preview/stop` | Request | Previewing | E→S→E | TBD |
| `preview/stopped` | Notification | Previewing | S→E | TBD |
| `preview/getPlaybackState` | Request | Previewing | E→S→E | TBD |
| `preview/gotoTime` | Request | Previewing | E→S→E | TBD |
| TBD | TBD | TBD | TBD | TBD |

### `simInitialize`

TBD

### `simInitialized`

TBD

### `simExit`

TBD

### `simExited`

TBD

### `edExit`

TBD
