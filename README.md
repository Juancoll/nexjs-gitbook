# Home

## websockets

We use websocket as an Inter program or module communication protocol. I develop a custom protocol over Websocket that implement this 2 Patterns

* Request/Response Pattern
* Publish/Subscribe Pattern

We can response to almost any problems in a effective way with a combination of this 2 patterns:

* Synchronized calls
* Notifications by Events

I start developing the protocol to communicate local Webs or \(mobile apps\) to a c\# Engine as the User Interface for Shine Product \([see more](https://public.juancoll.me/programming/shine)\).

* **nexjs - websocket** is used in frontend/Backend apps
* **nexcs - websocket** is used in local c\# application controlled by mobile, desktop or web applications.

## Nexjs - Websocket <a id="nexjs-websocket"></a>

### the idea <a id="the-idea"></a>

Separate in server the Functionality \(Contract\) from the call protocol and transport \(Rest API, Websocket TCP IP, HTTP, ...\) and create or update typed clients automaticaly. the main goals are

* Alway typed \(Client and Server\)
* Never implement a client
* Any update generate errors in code
* Separte functions \(contracts\) from transports and protocols. You van bind multiples protocols and transports on one server withoutcode duplication.
* Realtime Applications: the combination of Publish/Subscribe and Rest is required for an easy implementation.
* Realy simplified server code.
* Authentication separated from funcionality

![](.gitbook/assets/ecosystem.jpg)

### Code overview 

* Only Implements the server
* automatic client generation \(ts or c\#\)

```typescript
// ----------------------------------------------------------------------------
// [ Functionalities ]
class AContract {

  // [ decorators ]
  @Hub({ service: 'aContract', ...}) // Publish/Subscribe Protocol over websocket
  onUpdate = new HubEvent();         // Emitter custom class - only to unificate event system

  // [ decorators ]
  @Rest({ service: 'aContract',...}) // Request/Response Protocol over websocket
  methodA(): Promise<void>{
    // anything                      // Can return anything
  }  
}

// ----------------------------------------------------------------------------
// main.ts

const ioServer = ... // socket.io initialization 

// [ the binding ]
const wss = new WSServer<User, Token>();  // create Websocket protocols
wss.register(new AContract ());           // register class 
wss.init(new SocketIOServer(ioServer));   // initialize with a socket.io server

// ----------------------------------------------------------------------------
// in command line (create api client)
npm install @nexjs/cli -g
nexjs ts ws up-client -s ./[server-folder] -o ./[client-folder]/[api-folder]

// ----------------------------------------------------------------------------
// in client (after automatic generation)
const wsapi = new WSApi<User, Token>(new SocketIOClient()); // create client

// [Publish/Subscribe] protocol
// 1. register event Handler
wsapi.aContract.onUpdate.on(() =>
  console.log(`onUpdate`) // 
);
// 2. Subscribe to event
await wsapi.baseContract.onUpdate.sub();

// [Request/Response]  protocol 
await wsapi.baseContract.methodA();

//That's all !!! 
```

### Main repositories

| Repository name | Links | Description |
| :--- | :--- | :--- |
| @nexjs/wsserver | [github](https://github.com/Juancoll/nexjs-wsserver), [npm](https://www.npmjs.com/package/@nexjs/wsserver) | Server Decorators and Bindings Class |
| @nexjs/wsclient | [github](https://github.com/Juancoll/nexjs-wsclient), [npm](https://www.npmjs.com/package/@nexjs/wsclient) | Client Base Class \(required by all clients\) |
| @nexjs/cli | [github](https://github.com/Juancoll/nexjs-cli), [npm](https://www.npmjs.com/package/@nexjs/cli) | Command line to generate/update Clients |

### Development repositories

| Repository name | Link | Description |
| :--- | :--- | :--- |
| nexjs-ws.dev-project.server | [github](https://github.com/Juancoll/nexjs-ws.dev-project.server) | Project with @nexjs/wsserver in source code \(debuggable\) |
| nexjs-ws.dev-project.client | [github](https://github.com/Juancoll/nexjs-ws.dev-project.client) | Project with @nexjs/wsclient in source code \(debuggable\) |

### Demo repositories

| Repository name | Link | Description |
| :--- | :--- | :--- |
| nexjs-ws.demo-project.server-base | [github](https://github.com/Juancoll/nexjs-ws.demo-project.server-base) | simpliest server  |
| nexjs-ws.demo-project.server-nestjs |  | nestjs, mongoDb |
| nexjs-ws.demo-project.client-vue | [github](https://github.com/Juancoll/nexjs-ws.demo-project.client-vue) |  |
| nexjs-ws.demo-project.client-vuetify-dynamic-router |  |  |
| nexjs-ws.demo-project.client-nuxtjs |  |  |



