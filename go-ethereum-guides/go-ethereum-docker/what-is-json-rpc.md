## What is JSON-RPC

JSON-RPC is a stateless remote procedure call protocol (RPC) that allows you to remotely execute code on a server by sending the server a JSON string that looks like this:

```javascript
{
  "jsonrpc": "2.0",
  "method": "methodName",
  "params": { "something": "likeAnObject", [ "or", "an", "array"], 42},
  "id": 1
}
```

The JSON-RPC endpoint should return a response that looks like this:

```javascript
{
  "jsonrpc": "2.0",
  "result": {"thisResult": "anyType"},
  "id": 1
}
```

Or return an error:

```javascript
{
  "jsonrpc": "2.0"
  "error": {
    "code": -32700,
    "message": "This is the error code reserved for a 'Parse error'"
  },
  "id": 1
}
```

The [JSON-RPC specification](https://www.jsonrpc.org/specification) is currently at version 2.0.

## Why JSON-RPC?

- It is transport-agnostic. The JSON-RPC standard describes only the format of the request and response of an RPC call, allowing you to implement it as your RPC protocol-of-choice in any setup.

(can we send JSON-RPC calls to the geth IPC? — yes. ethereum/go-ethereum/readme.md states that "\[geth\] can be used by other processes as a gateway into the Ethereum network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports.")

## Ethereum JSON-RPC service

Official implementations of the Ethereum node (geth for Go, Aleth for C++, Pyeth for Python) allow you to start a JSON-RPC service that lets your application interact with the Ethereum node through a REST endpoint.

## Reserved Error Codes

|code|message|meaning|
|--- |--- |--- |
|-32700|Parse error|Invalid JSON was received by the server.An error occurred on the server while parsing the JSON text.|
|-32600|Invalid Request|The JSON sent is not a valid Request object.|
|-32601|Method not found|The method does not exist / is not available.|
|-32602|Invalid params|Invalid method parameter(s).|
|-32603|Internal error|Internal JSON-RPC error.|
|-32000 to -32099|Server error|Reserved for implementation-defined server-errors.|
