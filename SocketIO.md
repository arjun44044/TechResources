# Socket.IO- Introduction

****Socket.IO is a**** popular  library that is used for building real-time web applications. It allowsyour website and server to talk to each other instantly, making things like live chat and instant updates possible.

* Socket.IO makes it easy to manage WebSocket connections and events.
* It works even with older browsers that don’t fully support WebSockets.

## ****Why Use Socket.IO?****

Many web applications use a client-server setup, where clients (like web
browsers) request things from servers.  Traditional web communication
often uses HTTP, which can be slow for real-time updates and only allows
 one message at a time. Socket.IO solves this problem.

* Socket.IO enables instant, two-way communication between clients and servers.
* It’s more efficient than traditional HTTP for real-time applications.

## Features of Socket.IO

Socket.IO, built on top of Engine.IO, offers several important features that make
it a robust choice for real-time web applications:

* ****Reliability:**** Maintains connections even when faced with network obstacles like proxies, load balancers, firewalls, and antivirus software.
* ****Automatic Reconnection:**** The client automatically attempts to reconnect to the server if the connection is lost.
* ****Disconnection Detection:**** Provides mechanisms for both the client and server to detect when the other party has disconnected.
* ****Multiplexing:**** Allows multiple communication channels to operate over a single connection, improving efficiency.
* ****Binary Streaming:**** Supports the transmission of binary data, such as ArrayBuffers and Blobs.

## How Socket.IO Works?

Socket.IO consists of two main components

1. ****Server-side (Node.js):**** Listens for incoming connections and handles events.
2. ****Client-side (Browser/Frontend):**** Connects to the server and sends/receives messages.

##### How Communication Happens

1. A client connects to the  ****Socket.IO server**** .
2. The server acknowledges the connection.
3. Both parties can send and receive messages using  ****events**** .
4. If the connection is lost, Socket.IO automatically tries to reconnect.

## Real-time Applications

A real-time application (RTA) is an application that functions within a period that the user senses as immediate or current.

Some examples of real-time applications are −

* **Instant messengers** − Chat apps like Whatsapp, Facebook Messenger, etc. You need not refresh your app/website to receive new messages.
* **Push Notifications** − When someone tags you in a picture on Facebook, you receive a notification instantly.
* **Collaboration Applications** − Apps like google docs, which
  allow multiple people to update same documents simultaneously and apply
  changes to all people's instances.
* **Online Gaming** − Games like Counter Strike, Call of Duty, etc., are also some examples of real-time applications.

## ❓ Why Socket.IO?

-**Writing a real-time application with popular web applications stacks like LAMP (PHP) has traditionally been very hard. It involves pollingthe server for changes, keeping track of timestamps, and it is a lotslower than it should be.**

Sockets have traditionally been the solution around which most real-time systems are architected, providing a bi-directional
communication channel between a client and a server. This means that the server can push messages to clients. Whenever an event occurs, the idea is that the server will get it and push it to the concerned connectedclients.

> Socket.IO is quite popular, it is used by  **Microsoft Office, Yammer, Zendesk, Trello,** . and numerous other organizations to build robust real-time systems. It one of the most powerful **JavaScript frameworks** on  **GitHub** , and most depended-upon NPM (Node Package Manager) module. Socket.IOalso has a huge community, which means finding help is quite easy.

# ⚙️ How Socket.IO Works Internally

##### 1. **Initial Connection — HTTP Long Polling**

When a client connects:

* It  **first initiates a handshake over HTTP** .
* If WebSockets are supported by the browser and server, it **upgrades** the connection to WebSocket.
* If WebSockets are  **not supported** , it **falls back to long-polling** (keeping an HTTP connection open and reusing it).

> **HTTP Long Polling** is a **technique to emulate real-time communication** over the traditional HTTP protocol,  **before WebSockets became widely supported** .It’s a **fallback mechanism** used by libraries like **Socket.IO** when WebSockets are unavailable.
>
> 🔁 How It Works:
>
> 1. **Client sends an HTTP request** to the server (e.g., `GET /updates`).
> 2. The  **server does not respond immediately** . It **waits** until there’s **new data** to send (or a timeout occurs).
> 3. Once there’s data (e.g., a new message), the  **server sends a response** .
> 4. The **client immediately re-initiates** another HTTP request.
> 5. This  **cycle repeats** , keeping the client nearly always connected.
>
> So while it’s still using  **standard HTTP** , it feels *almost* real-time.
>
> 🧠 Visual Representation:
>
> ```
> Client: GET /updates
>        (waiting...)
> Server: [new message] → sends response
> Client: GET /updates (again)
> ```
>
> ⚠️ Key Points:
>
> | Feature                                                                                                                                                                                                                                                                                                                                          | Description                                          |
> | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
> | Protocol                                                                                                                                                                                                                                                                                                                                         | HTTP (not persistent)                                |
> | Real-time simulation                                                                                                                                                                                                                                                                                                                             | Yes, via frequent long-lived requests                |
> | Bandwidth<br />--**Bandwidth** refers to the **maximum amount of data** that can be<br /> transmitted over a network connection  **in a given amount of time** .<br />--**Bandwidth ≠ Speed** — Bandwidth is about  **capacity** ,<br /> not how fast individual data bits travel (that’s  **latency** ). | Less efficient than WebSocket (headers each time)    |
> | Latency<br />**---Latency** refers to the **time delay** between when an action is initiated<br /> and when the effect of that action is observed.<br />---In thsi context, **Latency** is the time it takes for a  **message to travel <br />from client to server and back** .                                        | Higher than WebSocket, but good enough for many uses |
> | Fallback use                                                                                                                                                                                                                                                                                                                                     | Used by Socket.IO when WebSocket isn't available     |
>
> ✅ Why Socket.IO Uses It
>
> Socket.IO uses **HTTP long polling as a fallback** to ensure:
>
> * Compatibility with **older browsers** or **strict proxies/firewalls**
> * A  **reliable initial connection** , which can later upgrade to WebSocket
>
> **HANDSHAKES**-----
>
> The **initial handshake** is critical for  **establishing identity** , negotiating  **transport methods** , and **setting up** the persistent connection.
>
> It sends some data like:
>
> * Supported transport methods (e.g., WebSocket, polling)
> * Session info
> * Authentication tokens (optional)

##### **2. Server Responds**

The server sends back a response with:

* A **unique session ID**
* Info about which transport will be used (`polling`, `websocket`, etc.)
* Any other config needed to proceed

##### **3. Upgrading to WebSocket (if possible)**

* If both the client and server support  **WebSocket** , the connection is **upgraded** from HTTP to  **WebSocket** .
* This upgrade is a special HTTP request called the `Upgrade` request.

##### **4.Persistent Connection**

After the upgrade:

* The **WebSocket** connection stays open
* It allows **bidirectional, real-time communication** without further HTTP overhead

##### 🧠 Why this matters:

* The **initial handshake** is critical for  **establishing identity** , negotiating  **transport methods** , and **setting up** the persistent connection.
* Even though WebSocket is the goal, **HTTP is used first** because it’s compatible with most firewalls and browsers.

###### 🚀 In Summary:

> “Handshake over HTTP” means the client first talks to the server via **HTTP** to agree on settings, then (if supported)  **switches to a faster, real-time WebSocket connection** .

##### **5.Event-Based Communication**

* Both client and server can **emit** and **listen** for custom events.
* Internally, Socket.IO uses a custom protocol to **serialize** and **parse** messages using JSON or binary formats.

Example:

```javascript
// Client
socket.emit("message", "Hello Server");

// Server
socket.on("message", (data) => {
  console.log(data); // "Hello Server"
});

```

##### **6.Namespaces and Rooms**

Socket.IO introduces two powerful concepts:

* **Namespaces** : Logical channels for separation (`/chat`, `/news`)
* **Rooms** : Groups of sockets you can broadcast to (e.g., only send to users in "room123")

```javascript
socket.join("room123");
io.to("room123").emit("newMessage", "Hi room!");

```

##### **7.Reconnection & Heartbeats**

* Built-in **reconnection logic** — if the connection drops, it auto-reconnects.
* Uses **heartbeats (ping/pong)** to detect dead connections.

HENCE PERSISTENCY ACHIEVED--

✅ 1. **Persistent**

🔄 What it means:

Once a WebSocket connection is established, it **stays open** until  **either the client or server closes it** .

🔧 How:

* The client sends an HTTP **Upgrade** request to switch to the WebSocket protocol.
* If the server agrees, they  **upgrade the connection** .
* From then on, the TCP socket stays  **open** , and no new requests need to be made for every message (unlike HTTP).

```
GET /socket HTTP/1.1
Upgrade: websocket
Connection: Upgrade

```

# WebSocket vs Socket.IO

## 🔗 WebSocket (the protocol)

### ✅ What it is:

* A **native browser protocol** (like HTTP).
* Enables **full-duplex** (two-way), real-time communication between client and server over a single TCP connection.

### 🛠️ How it works:

1. Starts as an  **HTTP handshake** .
2. Upgrades to a  **persistent WebSocket connection** .
3. Sends/receives data as  **binary or text frames** .

### ⚙️ Features:

* Lightweight and fast.
* Works directly via browser APIs (`new WebSocket()`).
* **No fallback mechanisms** — relies on the environment to support WebSocket.
* Pure protocol: **No reconnection, fallback, heartbeats, etc.** out of the box.

## 🚀 Socket.IO (the library)

### ✅ What it is:

* A **JavaScript library** built on top of WebSockets.
* Provides a **higher-level abstraction** with many features WebSocket lacks.

### 🛠️ How it works:

1. Tries to use WebSocket if available.
2. Falls back to **polling or other transports** if WebSocket fails.
3. Adds logic for:
   * Automatic reconnections
   * Broadcasting to rooms
   * Event-based communication
   * Acknowledgements
   * Heartbeats

### ⚙️ Features:

* Built-in  **reconnection support** .
* **Event-based syntax** (e.g., `socket.on("message")`).
* Works in  **Node.js & browser** .
* Supports namespaces, middleware, and rooms (like chat rooms).
* Requires both a **client and server library** (`socket.io-client` and `socket.io`).

# Installation and Basic Codes

To use  **Socket.IO** , you need to install both the **server-side** and optionally the **client-side** packages.

##### Server side (Node Js)--- `npm install socket.io`

Now in index.js-

```javascript
const { Server } = require("socket.io");
const http = require("http");

const server = http.createServer();
const io = new Server(server);

io.on("connection", (socket) => {
  console.log("A user connected");
});

server.listen(3000);

```

✅ What’s happening here?

1. **`const io = new Server(server);`**

* This creates a new **Socket.IO server instance** and binds it to your existing HTTP server (`server`).
* This means Socket.IO will now **listen for WebSocket connections** (and fallbacks like long-polling) on the same server.

2. **`io.on("connection", callback)`**

* This sets up a **listener** for the `"connection"` event.
* Every time a  **new client connects** , this callback runs.

3. **`(socket) => { ... }`**

* `socket` is a unique object that represents the connection  **between the server and a specific client** .
* You can use it to:
  * Receive messages from that client (`socket.on(...)`)
  * Send messages to that client (`socket.emit(...)`)
  * Listen for disconnects (`socket.on('disconnect', ...)`)

##### Client Side-

Option 1: via CDN--- If you’re using  **plain HTML/JS** :

```html
<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
<script>
  const socket = io("http://localhost:3000");
</script>

```

Option 2: via NPM (e.g., in React, Vue, etc.)----  `npm install socket.io-client`

```javascript
import { io } from "socket.io-client";
const socket = io("http://localhost:3000");

```

##### Socket Object (Server) in `io.on("connection", (socket) => `

```javascript
io.on("connection", (socket) => {
  console.log("User connected with ID:", socket.id);

  socket.on("message", (msg) => {
    console.log("Received message:", msg);
    // Echo the message back to the same client
    socket.emit("message", "Server received: " + msg);
  });

  socket.on("disconnect", () => {
    console.log("User disconnected:", socket.id);
  });
});

```

| Part                     | What it does                         |
| ------------------------ | ------------------------------------ |
| `socket`               | Represents one connected client      |
| `socket.on("event")`   | Listen for custom events from client |
| `socket.emit("event")` | Send messages back to the client     |

The `socket.id` in Socket.IO is a **unique identifier** automatically assigned to each client connection. It plays a critical role in distinguishing each connected client.

🔍 What is `socket.id`?

* It's a **string ID** (e.g., `"a2xs3a41WbZpRKUqAAAD"`).
* It is  **unique per connection** .
* Generated by the server  **when a client connects** .
* Changes every time the client reconnects (i.e., refreshes or loses connection).

🧠 Why is it useful?

You can use `socket.id` to:

1. **Track users individually**

   Store it in a database or memory map to associate the connection with a user/session.
2. **Send messages to a specific user**

   `io.to(socket.id).emit("private", "Hello only you!");`

⚠️ Important

* If a user refreshes the browser, the old `socket.id` becomes invalid and a **new one** is created.
* Don’t treat it like a stable user ID. If you need persistence, associate `socket.id` with a user ID from a database.

##### NOW FROM FRONTEND TO BACKEND COURSE OF EVENTS--

🔄 What happens step-by-step?

1. On the  **client side** , this line is run: ` const socket = io("http://localhost:3000");` It  initiates a connection to the Socket.IO server.
2. On the  **server side** , the Socket.IO server is listening:

```javascript
io.on("connection", (socket) => {
  console.log("A user connected");
});


```

As soon as the client connects, the `connection` event fires, and the server receives the **`socket` object** for that specific client.

3. This `socket` allows the server to:

* Send messages to that client only (`socket.emit(...)`)
* Listen for events from that client (`socket.on(...)`)
* Access metadata like `socket.id`

# How Socket.IO Communication Works, Sending and Listening To events

Socket.IO allows **real-time, bidirectional communication** between client and server using  **event-based architecture** . You send and listen for custom-named events, just like you’d use `addEventListener`.

## ✅ A. **Client Sends Event → Server Receives**

### 📦 Client Code (Frontend)

```javascript
// Connect to server
const socket = io("http://localhost:3000");

// Send event to server
socket.emit("send-message", {
  user: "Alice",
  message: "Hello Server!"
});

```

### 🖥️ Server Code (Backend)

```javascript
io.on("connection", (socket) => {
  console.log("Client connected:", socket.id);

  // Listen to 'send-message' event from client
  socket.on("send-message", (data) => {
    console.log("Message from client:", data);
    // data = { user: "Alice", message: "Hello Server!" }
  });
});

```

## ✅ B. **Server Sends Event → Client Receives**

### 🖥️ Server Code

```javascript
io.on("connection", (socket) => {
  // Send a welcome message to client
  socket.emit("welcome", "Welcome to the server!");
});

```

### 📦 Client Code

```javascript
const socket = io("http://localhost:3000");

// Listen for 'welcome' event
socket.on("welcome", (msg) => {
  console.log("Received from server:", msg);
});

```

## ✅ C. **Server Broadcast to All Clients**

```javascript
socket.on("send-message", (data) => {
  // Send to all other clients except sender
  socket.broadcast.emit("receive-message", data);

  // OR: Send to all including sender
  io.emit("receive-message", data);
});

```

### Client

```javascript
socket.on("receive-message", (data) => {
  console.log("New message from server:", data.message);
});

```

## 🧪 Putting it all together (Simple Chat Example)

### Client (Browser / Frontend):

```javascript
const socket = io("http://localhost:3000");

// Send message to server
document.querySelector("form").addEventListener("submit", (e) => {
  e.preventDefault();
  const msg = document.querySelector("input").value;
  socket.emit("send-message", { user: "Bob", message: msg });
});

// Receive message from server
socket.on("receive-message", (data) => {
  console.log("Received:", data.user + ": " + data.message);
});

```

### Server (Node.js Backend):

```javascript
const io = require("socket.io")(3000, {
  cors: { origin: "*" }
});

io.on("connection", (socket) => {
  console.log("New client:", socket.id);

  socket.on("send-message", (data) => {
    console.log("From client:", data);
    // Broadcast to all clients
    io.emit("receive-message", data);
  });
});

```

# Socket.IO Rooms

--By default every single user in socket.io has their own room, with themselves, that just their socket.id

> Hence to send 1-on-1 private message you can use the the other person's id as a room

In Socket.IO, a **room** is a **channel** where sockets (clients) can join and emit/listen to events  **only within that group** .

> ✅ Rooms are **logical groupings** — they don’t require the client to know about others in the room.
>
> ✅ Used for  **private chats** ,  **group chats** ,  **games** ,  **notifications** , etc.

### 🛠️ Key Methods

* `socket.join(roomName)` → join a room
* `socket.leave(roomName)` → leave a room
* `io.to(roomName).emit(...)` → emit to all in a room // It assumes --> `io.broadcast.to(roomName).emit(...)` since it doesn't send it to themselves
* `socket.to(roomName).emit(...)` → emit to all in room **except sender**

### 📦 1. Server Code (Backend)

```javascript
// index.js (Node.js backend)
const { Server } = require("socket.io");
const io = new Server(3000, {
  cors: { origin: "*" },
});

io.on("connection", (socket) => {
  console.log("Client connected:", socket.id);

  // Join a room
  socket.on("join-room", (roomName) => {
    socket.join(roomName);
    console.log(`Socket ${socket.id} joined room ${roomName}`);
  
    // Notify others in room
    socket.to(roomName).emit("user-joined", `${socket.id} joined the room`);
  });

  // Receive and forward messages in a room
  socket.on("send-room-message", ({ room, message }) => {
    io.to(room).emit("receive-room-message", {
      sender: socket.id,
      message,
    });
  });

  // Optional: Leave room
  socket.on("leave-room", (room) => {
    socket.leave(room);
    socket.to(room).emit("user-left", `${socket.id} left the room`);
  });
});

```

### 🌐 2. Client Code (Frontend)

```javascript
<input id="roomInput" placeholder="Enter room name" />
<input id="messageInput" placeholder="Message" />
<button onclick="joinRoom()">Join Room</button>
<button onclick="sendMessage()">Send Message</button>

<ul id="messages"></ul>

<script src="https://cdn.socket.io/4.6.1/socket.io.min.js"></script>
<script>
  const socket = io("http://localhost:3000");

  let currentRoom = "";

  function joinRoom() {
    const room = document.getElementById("roomInput").value;
    if (!room) return;
    currentRoom = room;
    socket.emit("join-room", room);
  }

  function sendMessage() {
    const msg = document.getElementById("messageInput").value;
    if (!currentRoom || !msg) return;
    socket.emit("send-room-message", {
      room: currentRoom,
      message: msg,
    });
  }

  socket.on("receive-room-message", (data) => {
    const li = document.createElement("li");
    li.textContent = `${data.sender}: ${data.message}`;
    document.getElementById("messages").appendChild(li);
  });

  socket.on("user-joined", (msg) => {
    const li = document.createElement("li");
    li.textContent = msg;
    document.getElementById("messages").appendChild(li);
  });

  socket.on("user-left", (msg) => {
    const li = document.createElement("li");
    li.textContent = msg;
    document.getElementById("messages").appendChild(li);
  });
</script>

```

### 🧪 Sample Flow

1. User enters `room123` and clicks **Join Room**
2. The server puts them in `room123`
3. They send `"Hello"`
4. Only users in `room123` receive `"Hello"`

### 🧩 Optional: List Users in a Room (Advanced)

```javascript
const usersInRoom = await io.in("room123").fetchSockets();
const userIds = usersInRoom.map(socket => socket.id);

```

### 🧼 Bonus: Auto Leave on Disconnect

```javascript
socket.on("disconnect", () => {
  console.log(`Socket ${socket.id} disconnected`);
  // Rooms are left automatically
});

```

# Emit Callback

When the server (or client) emits an event, it can pass a **callback function** as the last argument. The receiver can call this function to **send a response** back.

### 🔁 Here's what happens in an  *emit callback* :

**1. Server emits an event with a callback**

```javascript
socket.emit("get-user-data", (dataFromClient) => {
  console.log("Client sent back:", dataFromClient);
});

```

**2. Client receives the event and uses the callback:**

```javascript
socket.on("get-user-data", (ackCallback) => {
  const user = { name: "Alice", age: 24 };
  ackCallback(user); // Sends the data back to the server
});

```

✅ OUTPUT on server:

`Client sent back: { name: 'Alice', age: 24 }`

### Why is this useful?

* It mimics **request-response** behavior (like HTTP) in real-time communication.
* You don’t have to manually create a separate `on()` listener for every response.
* It's  **built-in acknowledgment** : the emitter knows when the receiver got the message and responded.

## 💬 Another Use Case (Client → Server with Callback)

You can also go the other way — client emits with a callback:

```javascript
socket.emit("saveMessage", { text: "Hello" }, (ack) => {
  console.log("Server confirmed:", ack);
});

```

On the server:

```javascript
socket.on("saveMessage", (data, callback) => {
  console.log(data);
  callback("Message saved successfully!");
});

```

# Admin dashboards in Socket.io

In  **Socket.IO** , an **admin dashboard** typically refers to a **visual interface** where you can monitor and manage the state of your real-time server. **Socket.IO admin dashboard** (especially the official one at [admin.socket.io]()) can  **monitor clients in real-time** .

#### ✅ What You Can Monitor About Clients:

| What                        | Description                                                                    |
| --------------------------- | ------------------------------------------------------------------------------ |
| **Socket ID**         | Unique identifier for each connected client.                                   |
| **Connection Time**   | When the client connected.                                                     |
| **Namespace**         | Which namespace the client is connected to.                                    |
| **Rooms Joined**      | List of rooms the client has joined.                                           |
| **IP Address**        | The IP address of the connected client.                                        |
| **Custom Data**       | If you're sending metadata (e.g. username, user ID), you can monitor that too. |
| **Ping/Pong Latency** | You can see how long it takes to ping the client (latency info).               |
| **Event Log**         | View real-time event activity from that client.                                |

#### 🔧 Features You Can Include in a Socket.IO Admin Dashboard:

| Feature                      | Description                                                                |
| ---------------------------- | -------------------------------------------------------------------------- |
| **Connected Clients**  | View the list of connected sockets (clients), their IDs, rooms, IPs.       |
| **Rooms Monitoring**   | See which rooms exist and which clients are in them.                       |
| **Emit Events**        | Send custom events from the dashboard to a specific socket or all clients. |
| **Log Messages**       | View logs of real-time events, joins/leaves, messages, etc.                |
| **Disconnect Clients** | Forcefully disconnect a socket connection.                                 |
| **Stats**              | Track number of users, rooms, pings, packet rates, etc.                    |
| **Namespaces**         | Monitor traffic/activity per namespace if you're using multiple.           |

#### 👀 Example (Using `@socket.io/admin-ui`)

When you open the dashboard:

* You’ll see all connected **clients** listed with their  **Socket IDs** .
* You can click on each to see:
  * Their current **rooms**
  * The events they’ve sent/received
  * Live status updates when they disconnect or change rooms

#### ✅ Option 1: Use [socket.io-admin-ui](https://github.com/socketio/socket.io-admin-ui) (Official)

Socket.IO provides an  **official admin UI** :

**Installation**

On the  **Socket.IO server** : `npm install @socket.io/admin-ui`

**Usage (in your server code):**

```javascript
const { instrument } = require("@socket.io/admin-ui");

const io = require("socket.io")(3000, {
  cors: {
    origin: ["https://admin.socket.io"],
    credentials: true
  }
});

instrument(io, {
  auth: {      // use Auth only if you need the protection elese leave it
    type: "basic",
    username: "admin",
    password: "your-password" // use bcrypt hash in production
  },
});

```

Now visit:

🔗 [https://admin.socket.io]()

Log in using your credentials, and it will show a **real-time dashboard** of your server.

#### ✅ Option 2: Build Your Own Custom Admin Dashboard

If you want more control, you can create your own frontend using React or any framework, and use:

* `io.sockets.sockets` to list all connected clients.
* `io.of("/").adapter.rooms` to inspect rooms.
* `socket.to(room).emit()` or `io.emit()` to send messages.
* REST API + Socket.IO hybrid setup to control sockets via dashboard buttons.

# Namespaces

#### 📛 What is a **Namespace** in Socket.IO?

A **Namespace** in Socket.IO is a way to create **separate communication channels** on the same Socket.IO server. Think of them as **sub-applications** that share the same underlying connection but can have  **isolated logic, events, and rooms** .

By default, Socket.IO uses the  **root namespace (`"/"`)** . But you can define **custom namespaces** to split different functionalities like:

* `/chat`
* `/admin`
* `/notifications`

#### 💡 Why Use Namespaces?

* Organize different parts of your app (e.g., admin vs user).
* Apply **authentication** or **middleware** differently.
* Limit or scope events to certain users or features.
* Reduce event clutter on the server and client.

**🧠 Server-Side: Creating Namespaces**

```javascript
const io = require("socket.io")(server);

// Root namespace (default)
io.on("connection", (socket) => {
  console.log("User connected to /");
});

// Custom namespace - /chat
const chatNamespace = io.of("/chat");

chatNamespace.on("connection", (socket) => {
  console.log("User connected to /chat namespace");

  socket.on("message", (data) => {
    console.log("Chat message:", data);
    chatNamespace.emit("message", data);
  });
});

// Another namespace - /admin
const adminNamespace = io.of("/admin");

adminNamespace.on("connection", (socket) => {
  console.log("Admin connected");

  socket.emit("admin-welcome", "Welcome to the admin panel!");
});

```

**🖥 Client-Side: Connecting to Namespaces**

```javascript
// Connect to root namespace
const rootSocket = io("/");

// Connect to /chat namespace
const chatSocket = io("/chat");

chatSocket.on("connect", () => {
  console.log("Connected to /chat");
  chatSocket.emit("message", "Hello from chat!");
});

// Connect to /admin namespace
const adminSocket = io("/admin");

adminSocket.on("admin-welcome", (msg) => {
  console.log(msg);
});

```

#### 🧪 Key Behavior

* Each namespace **has its own set of events** (`connection`, `disconnect`, etc.).
* You can **broadcast** within a namespace without affecting others.
* **Authentication** can be applied per namespace.

#### 🧼 When NOT to Use Namespaces

* If you're simply grouping users (e.g., chat rooms), **rooms** are a better choice.
* Avoid too many namespaces; they don't scale horizontally easily (require coordination in multi-server setups).

🔐 Middleware Per Namespace

```javascript
chatNamespace.use((socket, next) => {
  const token = socket.handshake.auth.token;
  if (isValid(token)) {
    next();
  } else {
    next(new Error("Authentication failed"));
  }
});

```

# Middlewares

In  **Socket.IO** , **middleware functions** allow you to **intercept and process socket connections or events** before they reach the final handler.

They are **very useful for:**

* Authenticating users
* Logging
* Blocking/rejecting connections
* Modifying data before it reaches the event handlers

#### 🧭 Types of Middleware in Socket.IO

There are two main types:

1. **Connection Middleware**

Runs **once** when a client attempts to connect to the server.

2. **Event Middleware**

Runs **every time** a specific event is received.

#### 🔌 1. Connection Middleware (on namespace level)

This is the most common type. It intercepts socket connections and decides whether to allow or reject them.

##### ✅ Example: Authenticating a user on connect

**Server (Node.js)**

```javascript
const io = require("socket.io")(3000);

// Middleware for all connections (on root namespace)
io.use((socket, next) => {
  const token = socket.handshake.auth.token;
  
  if (token === "my-secret-token") {
    return next(); // Allow connection
  }
  
  return next(new Error("Unauthorized"));
});

io.on("connection", (socket) => {
  console.log("User connected");
});

```

> 🔍 `socket.handshake.auth.token` — What it means
>
> When a client tries to connect to a Socket.IO server, it sends a **handshake** request. This request can include  **authentication data** , such as a token. On the server, you can access this data via: `socket.handshake.auth.token`
>
> This is commonly used to:
>
> * Authenticate users using a JWT or session token
> * Prevent unauthorized access to your app
> * Attach user info to the socket for later use

**Client**

```javascript
const socket = io("http://localhost:3000", {
  auth: {
    token: "my-secret-token"
  }
});

```

#### 🧪 2. Middleware for Custom Namespaces

```javascript
const adminNamespace = io.of("/admin");

adminNamespace.use((socket, next) => {
  const isAdmin = socket.handshake.auth.role === "admin";

  if (isAdmin) {
    next(); // Allow
  } else {
    next(new Error("Access denied"));
  }
});

adminNamespace.on("connection", (socket) => {
  console.log("Admin connected");
});

```

#### ⚙️ 3. Event Middleware (per socket)

> Available only in **Socket.IO v4.0+**

SYNTAX--

```javascript
socket.use(([event, ...args], next) => {
  // ...
});


```

You  **do need to destructure `[event, ...args]` exactly like that** , because that's the structure Socket.IO internally uses when it passes the event data to the middleware.

🔍 Here's why:

When a client emits an event like:  `socket.emit("message", "Hello", 123);`

The Socket.IO server internally represents this as: `["message", "Hello", 123]`

This full array is passed as the **first argument** to your middleware. So when you do: `([event, ...args], next)`

Example--

```javascript
io.on("connection", (socket) => {
  // Runs on each event sent from the client
  socket.use(([event, ...args], next) => {
    console.log(`Received event "${event}" with data`, args);
  
    // Example: block "delete-user" event
    if (event === "delete-user") {
      return next(new Error("This action is blocked."));
    }
  
    next(); // Allow other events
  });

  socket.on("message", (data) => {
    console.log("Message received:", data);
  });
});

```

#### 🔴 Handling Errors from Middleware (Client Side)

If middleware throws an error, the client will get it via the `.on("connect_error")` event:

```javascript
const socket = io();

socket.on("connect_error", (err) => {
  console.error("Connection error:", err.message); // e.g. "Unauthorized"
});

```

#### 📝 Summary

| Type             | Runs On                                   | Usage                         |
| ---------------- | ----------------------------------------- | ----------------------------- |
| `io.use()`     | On every connection (global or namespace) | Authentication, logging       |
| `socket.use()` | On every event received                   | Logging, validation, blocking |
| `next(error)`  | Used to reject connection or event        | Sends error to client         |

# Handling Errors

### 🔹 1. `connect_error`

> Triggered on the **client** side when the initial connection attempt fails.

##### 📦 Example:

```javascript
socket.on("connect_error", (err) => {
  console.error("Connection failed:", err.message);
});

```

This can happen due to:

* Server unreachable
* Auth error in middleware (e.g. token rejection)
* Invalid namespace

### 🔹 2. Custom error events in event-level middleware

If you reject something inside a middleware:

**Server**:

```javascript
io.use((socket, next) => {
  const token = socket.handshake.auth.token;
  if (token !== "valid-token") {
    return next(new Error("Authentication error"));
  }
  next();
});

```

**Client:**

```javascript
socket.on("connect_error", (err) => {
  console.log("Middleware blocked connection:", err.message);
});

```

### 🔹 3. Acknowledgment errors (Callback-based)

You can handle errors returned from server via callbacks.

**Client:**

```javascript
socket.emit("joinRoom", "room1", (response) => {
  if (response.error) {
    console.error("Failed to join:", response.error);
  }
});

```

**Server:**

```javascript
socket.on("joinRoom", (room, callback) => {
  if (!validRoom(room)) {
    return callback({ error: "Invalid room" });
  }
  socket.join(room);
  callback({ success: true });
});

```

### 🔹 4. `error` event (custom use)

You can also emit your own `error` event:

**Server:   `socket.emit("error", "Something bad happened");`**

**Client**:

```javascript
socket.on("error", (msg) => {
  console.log("Server sent error:", msg);
});

```

> But beware: using `"error"` can conflict with internal usage, so it's better to use a custom name like `"operation_error"`.

### 🔹 5. Transport or Engine.IO-level errors (rare)

These are deeper, lower-level issues like:

* `transport error`
* `ping timeout`
* `server error`

Most of the time, you’ll catch these indirectly via `connect_error`, `disconnect`, or `reconnect_error`.

# `socket.connect()` and `socket.disconnect()`

#### 🔌 `socket.connect()` — Reconnects a manually disconnected socket

✅ Purpose:

Used to manually **(re)connect** a socket if:

* You had disconnected it earlier using `socket.disconnect()`
* Or you initialized the socket with `{ autoConnect: false }`

SYNTAX -- `socket.connect();`

Example--

```javascript
// Client
const socket = io("http://localhost:3000", {
  autoConnect: false // Don't connect immediately
});

document.getElementById("connectBtn").onclick = () => {
  socket.connect(); // Connect when button clicked
};

socket.on("connect", () => {
  console.log("Connected with ID:", socket.id);
});

```

#### 🔌 `socket.disconnect()` — Closes the connection

✅ Purpose:

Used to **manually close** the connection from the client side.

SYNTAX-- ` socket.disconnect();`

Example--

```javascript
document.getElementById("disconnectBtn").onclick = () => {
  socket.disconnect(); // Disconnect when button clicked
};

socket.on("disconnect", (reason) => {
  console.log("Disconnected:", reason);
});

```

🛠 Server Example:

```javascript
const io = require("socket.io")(3000);

io.on("connection", (socket) => {
  console.log("New client connected:", socket.id);

  socket.on("disconnect", (reason) => {
    console.log(`Client ${socket.id} disconnected: ${reason}`);
  });
});

```

#### Reason

🔥 You can pass a custom reason from the client by calling `socket.disconnect()` with an optional reason using a query parameter or custom event. However, Socket.IO’s `disconnect()` method doesn’t directly accept a reason string.

To pass a reason explicitly, you can emit a custom event before disconnecting:

**Client:**

```javascript
socket.emit("custom-disconnect", "User clicked logout");
socket.disconnect();

```

**Server:**

```javascript
socket.on("custom-disconnect", (reason) => {
  console.log(`Client ${socket.id} sent reason: ${reason}`);
});

socket.on("disconnect", (reason) => {
  console.log(`Client ${socket.id} disconnected: ${reason}`);
});

```


# `socket.volatile.emit()` and `socket.emit()` vs `socket.volatile.emit()`

#### 🔥 `socket.volatile.emit` in **Socket.IO**

`socket.volatile.emit` is a method used to emit events  **without guaranteeing delivery** . It’s best for  **non-critical, high-frequency data** , such as live typing indicators, cursor positions, or real-time sensor data.

✅ Key Characteristics:

| Feature                       | Behavior                                                                   |
| ----------------------------- | -------------------------------------------------------------------------- |
| **Delivery Guarantee**  | ❌ No — if the packet can’t be sent immediately, it's**dropped** . |
| **Use Case**            | ⚡ Real-time, fast updates where it's okay to miss a few.                  |
| **Bandwidth Efficient** | ✅ Skips sending if socket is temporarily congested.                       |

#### 🧠 Analogy:

It’s like yelling a message to someone during a loud concert. If they hear it, great! If not, you don’t repeat it.

📦 Example:

```javascript
io.on("connection", (socket) => {
  // Sending non-critical updates like cursor movement
  setInterval(() => {
    const position = { x: Math.random() * 100, y: Math.random() * 100 };
    socket.volatile.emit("cursor-position", position);
  }, 50); // Emits every 50ms
});

```

If the network is slow or the client is lagging, these fast updates will **not queue up** — they will be **dropped** to prevent congestion.


```javascript
socket.on("cursor-position", (pos) => {
  // Update UI with received position
  moveCursorTo(pos.x, pos.y);
});

```

#### 🛑 When **NOT** to use `volatile`:

* Sending chat messages
* Notifications
* File uploads
* Any data that must arrive

#### 🆚 `socket.emit` vs `socket.volatile.emit`

|          Method          | Delivery Guarantee | Use Case                     |
| :----------------------: | ------------------ | ---------------------------- |
|     `socket.emit`     | ✅ Guaranteed      | Important messages           |
| `socket.volatile.emit` | ❌ Not guaranteed  | Frequent, disposable updates |


# SIMPLE CHAT BOX EXAMPLE 

### CLIENT---

```javascript
import React, { useEffect, useMemo, useState } from "react";
import { io } from "socket.io-client";
import {
  Box,
  Button,
  Container,
  Stack,
  TextField,
  Typography,
} from "@mui/material";
const App = () => {
  const socket = useMemo(
    () =>
      io("http://localhost:3000", {
        withCredentials: true,
      }),
    []
  );       // useMemo used so that so when each time new message is written by user, the socket won't connect that much time. useMemo is used to ensure that the socket instance is only created once, when the component first mounts, and not re-created on every render.

  const [messages, setMessages] = useState([]);
  const [message, setMessage] = useState("");
  const [room, setRoom] = useState("");
  const [socketID, setSocketId] = useState("");
  const [roomName, setRoomName] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    socket.emit("message", { message, room });
    setMessage("");
  };

  const joinRoomHandler = (e) => {
    e.preventDefault();
    socket.emit("join-room", roomName);
    setRoomName("");
  };

  useEffect(() => {
    socket.on("connect", () => {
      setSocketId(socket.id);
      console.log("connected", socket.id);
    });

    socket.on("receive-message", (data) => {
      console.log(data);
      setMessages((messages) => [...messages, data]);
    });

    socket.on("welcome", (s) => {
      console.log(s);
    });

    return () => {
      socket.disconnect();
    };
  }, []);

  return (
    <Container maxWidth="sm">
      <Box sx={{ height: 500 }} />
      <Typography variant="h6" component="div" gutterBottom>
        {socketID}
      </Typography>

      <form onSubmit={joinRoomHandler}>
        <h5>Join Room</h5>
        <TextField
          value={roomName}
          onChange={(e) => setRoomName(e.target.value)}
          id="outlined-basic"
          label="Room Name"
          variant="outlined"
        />
        <Button type="submit" variant="contained" color="primary">
          Join
        </Button>
      </form>

      <form onSubmit={handleSubmit}>
        <TextField
          value={message}
          onChange={(e) => setMessage(e.target.value)}
          id="outlined-basic"
          label="Message"
          variant="outlined"
        />
        <TextField
          value={room}
          onChange={(e) => setRoom(e.target.value)}
          id="outlined-basic"
          label="Room"
          variant="outlined"
        />
        <Button type="submit" variant="contained" color="primary">
          Send
        </Button>
      </form>

      <Stack>
        {messages.map((m, i) => (
          <Typography key={i} variant="h6" component="div" gutterBottom>
            {m}
          </Typography>
        ))}
      </Stack>
    </Container>
  );
};

export default App;
```

> * Here, useMemo is used so that so when each time new message is written by user, the socket won't connect that much time.
> * useMemo is used to ensure that the socket instance is only created once, when the component first mounts, and not re-created on every render.
> * Every time your component re-renders (e.g., when you type in the message input), a **new socket connection** would be created. This leads to **multiple unnecessary socket connections** being opened. It can cause  **memory leaks** ,  **duplicate event listeners** , and **unexpected behavior** like receiving messages multiple times.
>
> `useMemo` prevents this by:
>
> * Running the `io(...)` only  **once** , on the first render.
> * Reusing the same socket instance on every subsequent render.

### SERVER:

```javascript
import express from "express";
import { Server } from "socket.io";
import { createServer } from "http";
import cors from "cors";
import jwt from "jsonwebtoken";
import cookieParser from "cookie-parser";

const secretKeyJWT = "asdasdsadasdasdasdsa";
const port = 3000;

const app = express();
const server = createServer(app);
const io = new Server(server, {
  cors: {
    origin: "http://localhost:5173",
    methods: ["GET", "POST"],
    credentials: true,
  },
});

app.use(
  cors({
    origin: "http://localhost:5173",
    methods: ["GET", "POST"],
    credentials: true,
  })
);

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.get("/login", (req, res) => {
  const token = jwt.sign({ _id: "asdasjdhkasdasdas" }, secretKeyJWT);

  res
    .cookie("token", token, { httpOnly: true, secure: true, sameSite: "none" })
    .json({
      message: "Login Success",
    });
});

io.use((socket, next) => {
  cookieParser()(socket.request, socket.request.res, (err) => {
    if (err) return next(err);

    const token = socket.request.cookies.token;
    if (!token) return next(new Error("Authentication Error"));

    const decoded = jwt.verify(token, secretKeyJWT);
    next();
  });
});

io.on("connection", (socket) => {
  console.log("User Connected", socket.id);

  socket.on("message", ({ room, message }) => {
    console.log({ room, message });
    socket.to(room).emit("receive-message", message);
  });

  socket.on("join-room", (room) => {
    socket.join(room);
    console.log(`User joined room ${room}`);
  });

  socket.on("disconnect", () => {
    console.log("User Disconnected", socket.id);
  });
});

server.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```


# Broadcast to a Room---- `io.to(room).emit(...)`

The expression `io.to(room).emit(...)` is used in **Socket.IO (server-side)** to  **broadcast an event to all sockets (clients) that have joined a specific room** .

SYNTAX-- `io.to("room-name").emit("event-name", data);`

* `io`: The main Socket.IO server instance.
* `.to("room-name")`: Targets **all clients** in the room `"room-name"`.
* `.emit("event-name", data)`: Sends the specified **event** with optional  **data** .

✅ Example – Simple Chat Room:

```javascript
io.on("connection", (socket) => {
  socket.on("joinRoom", (room) => {
    socket.join(room);
    console.log(`${socket.id} joined room ${room}`);
  });

  socket.on("message", ({ room, text }) => {
    // Sends message to everyone in the room, including sender
    io.to(room).emit("message", { user: socket.id, text });
  });
});

```

**CLIENT:**

```javascript
socket.emit("joinRoom", "developers");

socket.on("message", (data) => {
  console.log(`${data.user}: ${data.text}`);
});

socket.emit("message", { room: "developers", text: "Hello devs!" });
```

⚠️ Note

If you want to  **broadcast to everyone *except* the sender** , you should use:  `socket.to("room-name").emit("event", data);`

**Comparison----**

| Usage                 | Sends To                                       |
| --------------------- | ---------------------------------------------- |
| `io.to("room")`     | All clients in room,**including sender** |
| `socket.to("room")` | All clients in room,**excluding sender** |



# All Methods and Properties of `socket`

##### 🔹 `socket` Properties (Server-side)

| Property         | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| `id`           | A unique identifier for the connected socket (`socket.id`). |
| `handshake`    | Contains details like headers, query params, IP, and auth.    |
| `rooms`        | A `Set`of rooms this socket is currently joined to.         |
| `data`         | A custom object to store any arbitrary data per socket.       |
| `connected`    | Boolean flag indicating if socket is connected.               |
| `disconnected` | Boolean flag indicating if socket is disconnected.            |
| `nsp`          | The `Namespace`instance this socket belongs to.             |

##### 🔹 Client-Side `socket` Properties

| Property         | Description                                                                |
| ---------------- | -------------------------------------------------------------------------- |
| `id`           | Unique identifier assigned by the server (same as `socket.id`on server). |
| `connected`    | `true`if the client is connected to the server.                          |
| `disconnected` | `true`if the client is disconnected.                                     |
| `active`       | `true`if the connection is active (experimental).                        |
| `io`           | Reference to the underlying `Manager`instance (controls the connection). |
| `nsp`          | Namespace the client is connected to (e.g.,`'/'`,`'/admin'`).          |
| `auth`         | An object to send auth data during handshake.                              |

##### 🔹 `socket` Methods (Server-side)

🔸 Emitting / Receiving Events

| Method                                     | Description                                                 |
| ------------------------------------------ | ----------------------------------------------------------- |
| `socket.emit(event, data)`               | Emit an event to the connected client.                      |
| `socket.on(event, callback)`             | Listen for an event from the client.                        |
| `socket.once(event, callback)`           | Like `on`, but only once.                                 |
| `socket.removeListener(event, callback)` | Remove a specific listener.                                 |
| `socket.off(event, callback)`            | Remove a specific listener. (Modern way, best practise)    |
| `socket.off(event) `                     | Remove all listeners for event. (Modern way, best practise) |

##### 🔸 Disconnection

| Method                              | Description                                                        |
| ----------------------------------- | ------------------------------------------------------------------ |
| `socket.disconnect(close = true)` | Disconnect the socket. Optionally close the underlying connection. |

##### 🔸 Rooms

| Method                                | Description                                        |
| ------------------------------------- | -------------------------------------------------- |
| `socket.join(room)`                 | Join the specified room.                           |
| `socket.leave(room)`                | Leave the specified room.                          |
| `socket.to(room).emit(event, data)` | Emit to clients in a room (excluding this socket). |
| `socket.in(room).emit(event, data)` | Alias of `to(room)`                              |

##### 🔸 Broadcasting

| Method                                  | Description                            |
| :-------------------------------------- | -------------------------------------- |
| `socket.broadcast.emit(event, data)`  | Send to everyone except this socket.   |
| `socket.broadcast.to(room).emit(...)` | Send to a room, excluding this socket. |

##### 🔸 Acknowledgements (Callbacks)

| Method                                       | Description                                        |
| -------------------------------------------- | -------------------------------------------------- |
| `socket.emit(event, data, callback)`       | Emit with a callback to handle client’s response. |
| `socket.on(event, (data, callback) => {})` | Listen with support for client callback.           |

##### 🔸 Advanced

| Method                           | Description                                       |
| -------------------------------- | ------------------------------------------------- |
| `socket.compress(boolean)`     | Enables/disables compression for the next packet. |
| `socket.binary(boolean)`       | Send data as binary (for the next packet).        |
| `socket.timeout(ms).emit(...)` | Wait for ack within a time limit.                 |


## `socket` Property- handshake

The `socket.handshake` object on the **server side** in Socket.IO gives you access to various **client connection details** during the handshake process — i.e.,  **when a client connects** .

```javascript
io.on("connection", (socket) => {
  const { headers, auth, query, address, time } = socket.handshake;

  console.log("New connection from:", address);
  console.log("Auth token:", auth.token);
  console.log("Query params:", query);
  console.log("User-Agent:", headers['user-agent']);
});

```

## `socket` Property- rooms

In  **Socket.IO** , each `socket` (client connection) has a `.rooms` property on the  **server side** , which represents all the **rooms** the socket has joined.

✅ By default:

When a socket connects, it automatically joins a room with its own  **socket ID** .

Example:

```javascript
io.on("connection", (socket) => {
  console.log(socket.id);            // e.g., '1b2c3d'
  console.log(socket.rooms);         // Set { '1b2c3d' }
});

```

**Rooms** allow you to **group sockets** together so you can  **emit events to all sockets in a room** .

You can:

* Join a room using `socket.join(roomName)`
* Leave a room using `socket.leave(roomName)`
* Emit to a room using `io.to(roomName).emit(...)`

## `socket` Property- data

In  **Socket.IO** , the `socket.data` property is a place where you can **store arbitrary data** associated with a specific client connection (socket). It's like attaching custom properties to a user's socket session.

🔹 What Is `socket.data`?

* A per-socket storage object.
* Exists only for the lifetime of the socket connection.
* Not shared between sockets or persisted between sessions.

✅ Use Cases

* Storing user information after authentication
* Tracking state related to the client
* Attaching roles, preferences, or session info

🧪 Example

**Server (Node.js with Socket.IO)**

```javascript
io.on("connection", (socket) => {
  // Assign user data to the socket
  socket.data.username = "Alice";
  socket.data.role = "admin";

  // Use it later in an event
  socket.on("sayHello", () => {
    console.log(`Hello from ${socket.data.username}`);
  });
});

```

## `socket` Property- connected

The `socket.connected` property in **Socket.IO** is a boolean that tells you **whether a socket is currently connected** to the server or not.

`true`: The socket is currently **connected** and active.  `false`: The socket is **disconnected** (manually or due to connection loss).


## `socket` Property- disconnected

The `socket.disconnected` property in **Socket.IO** is a boolean that tells you **whether a socket is currently disconnected** to the server or not.

`true`: The socket is currently dis**connected** (manually or due to connection loss).  `false`: The socket is **connected**


## `socket` Property- `nsp`

The `socket.nsp` property in **Socket.IO** refers to the **namespace** the socket is connected to.

🔹 What is `socket.nsp`?

* It’s a reference to the **namespace object** the socket is a part of.
* This is useful if you want to broadcast to other sockets within the  **same namespace** .

🧠 Background: What is a namespace?

In Socket.IO, a namespace is a **communication channel** allowing you to separate logic and connections.

By default, all sockets connect to the `"/"` namespace. You can also define custom ones (e.g. `/admin`, `/chat`).

✅ Example

```javascript
io.of("/admin").on("connection", (socket) => {
  console.log(socket.nsp.name); // '/admin'
});

```

In this case, `socket.nsp` refers to the namespace object for `/admin`.

🔄 Broadcasting using `socket.nsp`

You can use `socket.nsp.emit()` to **broadcast to all clients** in the same namespace:   `socket.nsp.emit("event", "data");`

This is equivalent to:    `io.of("/admin").emit("event", "data");`

🧪 Example with Default Namespace

```javascript
io.on("connection", (socket) => {
  console.log(socket.nsp.name); // '/'
});

```

Even in the default namespace, `socket.nsp` exists.

🔧 Use Cases

* When you write generic socket handlers and want to emit to the **same namespace** dynamically.
* When you're in a custom namespace handler (`io.of('/namespace')`) and don’t want to hardcode namespace names.

> **--- `socket.nsp.emit()`**
>
> `socket.nsp.emit()` is a method used to **broadcast an event to all connected sockets** in the **same namespace** as the current `socket`.
>
> 🧠 **Use Case**
>
> You want to **broadcast** something to every socket connected to the same namespace from within a single socket's handler — e.g., to notify all clients of a new user, a global event, or server status.
>
> Example--
>
> 🔄 **Compare with:**
>
> | Method                      | Broadcasts to                    | Includes sender? |
> | --------------------------- | -------------------------------- | ---------------- |
> | `socket.emit()`           | Just that socket                 | ✅ Yes           |
> | `socket.broadcast.emit()` | All except sender                | ❌ No            |
> | `io.emit()`               | All sockets in default namespace | ✅ Yes           |
> | `io.to(room).emit()`      | Only sockets in a specific room  | Depends          |
> | `socket.nsp.emit()`       | All sockets in current namespace | ✅ Yes           |

```javascript
io.of("/chat").on("connection", (socket) => {
  console.log("User connected to /chat");

  // Broadcast to all in the namespace
  socket.nsp.emit("announcement", "A new user has joined the chat!");
});
                     	
```

## `socket` Method- `socket.once(event, callback)`

The `socket.once(event, callback)` method in **Socket.IO** is used to **listen for an event only once** — after the first time the event is received, the listener is automatically removed.

✅ Syntax--

```javascript
socket.once("event-name", (data) => {
  // This will run only once when the event is received
});

```

The second call from the client will be ignored

🧠 Use Cases

* One-time authentication handshake
* Listening for a one-time confirmation or response
* Avoiding memory leaks when expecting only one event

## `socket` Method- `socket.once(event, callback)`

The `socket.removeListener(event, callback)` method is used to **unregister** (remove) a previously registered event listener.

✅ Syntax---  `socket.removeListener("event-name", callbackFunction);`

It's an **alias** of `socket.off()` — they behave the same way.

📌 Why Use It?

If you've attached a listener using `.on()` or `.once()` and want to remove it (to prevent it from being called later), use `.removeListener()`.

EXAMPLE--

```javascript
function handleMessage(msg) {
  console.log("Received:", msg);
}

io.on("connection", (socket) => {
  socket.on("message", handleMessage);

  // Let's remove it after 5 seconds
  setTimeout(() => {
    socket.removeListener("message", handleMessage);
    console.log("Listener removed");
  }, 5000);
});

```

🔍 Use Cases

* Prevent memory leaks in long-lived connections
* Dynamically control which events are being listened to
* Temporarily attach listeners during a specific task

## `socket` Method- `socket.in(room).emit(event, data)` and  `socket.in(room).emit(event, data)` vs `socket.at(room).emit(event, data)`

`socket.in(room).emit(event, data)` is used to  **send a message to all sockets in a specific room** , **except the current socket** that is sending the message.

-- Both `socket.in(room).emit(...)` and `socket.at(room).emit(...)` in **Socket.IO v4+** are used to  **emit events to specific rooms** , but they have a subtle and important difference regarding  **whether the sender receives the event or not** .

🔁 Comparison: `socket.in(...)` vs `socket.at(...)`

| Method                        | Sends to                | Includes sender?            | Version            |
| ----------------------------- | ----------------------- | --------------------------- | ------------------ |
| `socket.in(room).emit(...)` | All sockets in the room | ❌**Excludes sender** | All versions       |
| `socket.at(room).emit(...)` | All sockets in the room | ✅**Includes sender** | **v4+ only** |


## `socket` Method- `socket.broadcast.to(room).emit(event, data)` and `socket.broadcast.to(room).emit(event, data) `  vs `socket.in(room).emit(event, data)`

`socket.broadcast.to(room).emit(event, data)` is a **more explicit** form of the same thing: `socket.in(room).emit(event, data)` which btw is just not much explicit


## `socket` Method-  `socket.compress(boolean) `

🔧 `socket.compress(boolean)` in **Socket.IO**

The `socket.compress()` method is used to  **control whether the data sent to the client is compressed or not** .

SYNTAX-- `socket.compress(true).emit("event", data);`

✅ Purpose

Socket.IO (under the hood using WebSocket or polling) **compresses messages** using **per-message deflate** to reduce bandwidth.

However,  **compression takes CPU time** , so for **small messages** or  **low-priority events** , you might want to **disable compression** to improve performance.

⚙️ Parameters

| Parameter | Type    | Description                                 |
| --------- | ------- | ------------------------------------------- |
| `true`  | Boolean | (default) Enables compression               |
| `false` | Boolean | Disables compression for that specific emit |

🔁 Sending a chat message (compression ON by default). ⚡ Sending a frequent update (e.g., mouse position) —make compression OFF


## `socket` Method- `socket.binary(boolean)`

The `socket.binary()` method is used to specify  **whether the emitted event should be treated as binary or not** .

✅ Purpose

By default, Socket.IO can **automatically detect binary data** (like `Buffer`, `Blob`, `ArrayBuffer`) and handle it accordingly.

However, if you want to **force the binary option** (either on or off), you can use `socket.binary(true or false)`.

SYNTAX--`  socket.binary(true).emit("event", data);`

🧠 Use Cases

✅ Example 1: Sending a file buffer (binary)

```javascript
const fs = require("fs");

const imageBuffer = fs.readFileSync("image.png");

socket.binary(true).emit("image", imageBuffer);

```

This tells Socket.IO to send the data as binary explicitly.

❌ Example 2: Prevent sending as binary

`socket.binary(false).emit("imageData", { buffer: imageBuffer, type: "png" });`

You may want this if you’re wrapping binary in an object and want it encoded as JSON, not binary.

🔍 When to Use

* Usually not needed —  **Socket.IO auto-detects binary** .
* Only useful when you want to  **override the automatic behavior** , especially when you're:
  * Optimizing message formats
  * Avoiding binary decoding on the client
  * Sending mixed-format payloads


## `socket` Method- `socket.timeout(ms).emit(...)`

The `socket.timeout(ms).emit(...)` method in **Socket.IO (v4+)** allows you to  **emit an event with a timeout for the acknowledgment (ack) callback** .

If the server doesn’t respond within the given time, it triggers a timeout error.

✅ Why Use It?

When you emit an event and expect a response (ack), but want to **avoid waiting forever** — you set a timeout!

📌 Syntax

```javascript
socket.timeout(5000).emit("eventName", data, (err, response) => {
  if (err) {
    console.error("Timeout or error", err);
  } else {
    console.log("Server responded:", response);
  }
});

```

🧠 How It Works

* `timeout(ms)` sets a **deadline** in milliseconds.
* If the server **acknowledges** the event before the timeout, `err` is `null`.
* If not, `err` will be an object:  `{ message: 'timeout' }`

✅ Example

**Client Side**

```javascript
// emit with 3-second timeout
socket.timeout(3000).emit("getData", { id: 42 }, (err, data) => {
  if (err) {
    console.log("⏰ Request timed out:", err.message);
  } else {
    console.log("✅ Server response:", data);
  }
});

```

**Server Side**

```javascript
io.on("connection", (socket) => {
  socket.on("getData", (data, callback) => {
    // Simulate async processing
    setTimeout(() => {
      callback({ name: "Item 42", price: 99 }); // ack response
    }, 2000); // Responds in 2s (within timeout)
  });
});

```

🟢 Result: Acknowledgment received → client sees the response

❌ Now With Delay Exceeding Timeout

If server takes too long:

```javascript
// Server
setTimeout(() => {
  callback({ name: "Item 42" });
}, 4000); // Exceeds 3s timeout

```

Client output: ⏰ Request timed out: timeout    


## `socket` Method- `socket.off(event)` and `socket.off(event, callback)`

* **Modern & preferred** method (introduced in later Node versions and Socket.IO).
* Removes a specific listener if callback is provided.
* If no callback is given, **removes all listeners** for that event.

✅ Usage:

```javascript
// Remove specific listener
socket.off("message", messageHandler);

// Remove all listeners for "message"
socket.off("message");

// Remove all listeners for all events (rarely used)
socket.off();

```

⚠️ Gotcha

Removing a listener **only works if the exact function reference** is passed. This won’t work:

```javascript
socket.on("event", () => console.log("hi"));
socket.off("event", () => console.log("hi")); // ❌ won't remove

```

**You need:**

```javascript
const handler = () => console.log("hi");
socket.on("event", handler);
socket.off("event", handler); // ✅ works

```

The `socket.off()` method is used to **remove event listeners** from a socket. It's the modern and preferred way to **unsubscribe** or **detach** functions that were previously attached with `socket.on()` or `socket.once()`.

🧠 Why Use `off()`?

* Prevent **duplicate listeners** during reconnection or re-renders (especially in React).
* Avoid  **memory leaks** .
* Dynamically **switch behavior** of sockets.
* Properly clean up listeners in single-page apps or components.


## Client-side and Server-side lifecycle events

**🔹 Events Common to Both Client & Server:**

| Event Name     | Description                                 |
| -------------- | ------------------------------------------- |
| `connect`    | Triggered when a connection is established. |
| `disconnect` | Triggered when a socket disconnects.        |
| `error`      | Triggered on any socket-level error.        |

**🔹 Client-Side ONLY Events:**

| Event Name            | Description                                             |
| --------------------- | ------------------------------------------------------- |
| `connect_error`     | When initial connection fails.                          |
| `connect_timeout`   | When connection times out.                              |
| `reconnect`         | Fired when successfully reconnected.                    |
| `reconnect_attempt` | Fired on every reconnection attempt.                    |
| `reconnect_error`   | Fired when a reconnection attempt fails.                |
| `reconnect_failed`  | Fired after all reconnection attempts fail.             |
| `ping`,`pong`     | Low-level events used by Socket.IO’s heartbeat system. |
