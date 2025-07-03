# WebRTC- Introduction

## Applications

Alllows to establish **real-time communication** for:

* 📹 Live video streaming
* 🎙 Audio calls
* 📁 Peer-to-peer file sharing
* 🎮 Real-time gaming
* 💬 Chat apps

> ✅ It works directly in the browser (Chrome, Firefox, Safari, etc.) without any plugins.

## 🧠 How WebRTC Works – The High-Level Architecture (Refer Note First)

WebRTC allows **two clients** (peers) to  **communicate directly** , but before that happens, they must **exchange connection details** using a  **signaling mechanism** .

### 🛠 WebRTC Needs Three Main Components:

|                   Component                   | Purpose                                                               |
| :-------------------------------------------: | --------------------------------------------------------------------- |
|          **Signaling Server**          | Used to exchange metadata (like IP, ports, and session descriptions). |
|          **STUN/TURN Server**          | Helps discover and traverse NAT/firewalls.                            |
| **Peer Connection (RTCPeerConnection)** | Establishes the actual P2P connection and media/data transmission.    |

#### 🌐 What is **NAT** (Network Address Translation)?

🧠 **Definition:**

**NAT** is a technique used by routers to **translate private (local) IP addresses into a public IP address** so devices can communicate over the internet.

**🏠 Real-World Analogy:**

Think of NAT like a receptionist at a company:

* **Inside phones** (private IPs) don’t have direct external lines.
* The **receptionist** (router) handles all incoming/outgoing calls (public IP).
* She keeps track of **who made which request** and routes replies correctly.

**🔍 Why NAT exists:**

* IPv4 addresses are limited (only about 4.3 billion).
* Most home/office networks use **private IPs** like `192.168.x.x`, `10.x.x.x`, or `172.16.x.x`.
* These are not directly accessible from the public internet.

So the router does the following:

1. **Outgoing:** Translates internal IPs to a **public IP** and sends requests.
2. **Incoming:** When replies come back, it remembers who asked and routes them properly.

🔁 **What NAT really does:**

> **NAT (Network Address Translation)** converts **private IP addresses** (used inside your home or office) to a **public IP address** so devices can communicate with the internet.

**✅ So the flow is:**

* Your device (e.g. `192.168.1.10`) sends a request to the internet.
* The **router** (with a public IP like `203.0.113.5`) **translates** your internal IP and port to something like:
  ```
  192.168.1.10:1234 ➝ 203.0.113.5:54231

  ```

-- The website on the internet responds to `203.0.113.5:54231`, and the router **remembers** which internal IP/port to send the response back to.

**🔐 Why?**

Because:

* **Private IPs** (like `192.168.x.x`, `10.x.x.x`) **cannot be routed on the public internet.**
* Only **public IPs** (like `203.x.x.x`) are recognized globally.

📦 Example:

| Internal Device | Private IP  | NAT Translation | Public IP (used on Internet) |
| --------------- | ----------- | --------------- | ---------------------------- |
| Your laptop     | 192.168.0.5 | Port 52000      | 122.123.14.22:52000          |
| Your phone      | 192.168.0.6 | Port 52001      | 122.123.14.22:52001          |

**They both appear to the internet as coming from the same IP (`122.123.14.22`) but different  **ports** .

**❗The Problem: NAT Breaks Peer-to-Peer**

If you’re on a NATed network,  **another peer can’t just connect to your internal IP** , because it’s not publicly visible.

Hence e use STUN/TURN server to get **ICE Candidates**

#### 🔹 What is STUN?

**STUN** stands for  **Session Traversal Utilities for NAT** . It's a protocol that helps a device behind a **NAT (Network Address Translation)** discover its **public IP address** and **port** as seen by the outside world.

STUN is essential in **WebRTC** for peer-to-peer communication, especially when both clients are behind NATs and need to know their public-facing IPs to establish a connection.

**🔹 Why "Discover" a Public IP If It's Already Public?**

When you're behind a **NAT** (Network Address Translation),  **you don't actually know what your public IP and port are** , even though they **do exist** from the perspective of the outside world.

Let’s break this down with an analogy and technical explanation.

**🔸 How NAT Works (Quick Recap)**

Most home/office networks have **private IP addresses** (e.g., `192.168.x.x`, `10.x.x.x`) which **aren’t routable** on the public internet. The NAT device (usually your router):

* Keeps a mapping between internal (private) IP:port and public IP:port.
* Rewrites IP/port headers when packets go out.
* Multiple internal devices share one public IP using port translation.

**🔸 The Problem**

Your computer **knows** its private IP (`192.168.0.5`) but **not** the public IP and port the NAT assigns when sending traffic to the internet. And:

* NAT behavior is **not standardized** — it might use predictable or random port mapping.
* You cannot rely on local inspection to know what public IP:port the NAT assigned.

**🔸 Enter STUN: "Tell Me How You See Me"**

A STUN server is a simple server on the public internet. Your device sends it a request like:

> "Hey STUN server, what IP and port do **you** see me coming from?"

The STUN server replies with:

> "You're reaching me from `203.0.113.45:58324`"

Now your device knows its **public-facing address** — not what it thinks it is locally.

**🔸 Why This Matters for WebRTC**

WebRTC needs both peers to **know and share their public IP:port pairs** to try establishing a  **direct peer-to-peer connection** . Without STUN:

* You would share your private IP (`192.168.x.x`) with a peer — which is **useless** over the internet.

With STUN, both parties learn and share their **real public-facing endpoints** (called  **ICE candidates** ) so that a direct connection might be possible.

**✅ TL;DR**

Even though a public IP exists  **externally** , your device doesn’t know what it is because:

* It's behind a NAT.
* The NAT may change your IP/port mapping.
* Your system only sees its  **local/private IP** .

So **STUN helps "discover"** the real public IP and port  **from the outside’s point of view** , which is **critical** for peer-to-peer communication like WebRTC.

**Also, No, STUN doesn't replace the job of NAT — it *relies* on it.**

Let me explain why:

**🔹 NAT vs STUN — What Each One Does**

| Aspect                  | NAT (Network Address Translation)                     | STUN (Session Traversal Utilities for NAT)              |
| ----------------------- | ----------------------------------------------------- | ------------------------------------------------------- |
| **Purpose**       | Translates private IP/port to public IP/port          | Helps discover what public IP/port the NAT assigned     |
| **Where it runs** | On your router or firewall                            | On a public server on the internet                      |
| **Who uses it**   | Every device behind a router                          | WebRTC clients, VoIP apps, etc.                         |
| **Function**      | Enables multiple private devices to share 1 public IP | Reports back your public-facing IP/port as seen outside |

**🔸 Why STUN *Needs* NAT**

* NAT is the **one doing the translation** of your local address to something routable on the internet.
* STUN **cannot work without NAT** because there's nothing to “discover” unless your address is being rewritten.

> Think of it like this:
>
> **NAT is the mask** that changes your identity.
>
> **STUN is the mirror** that shows you what you look like with the mask on.

**🔹 So What Is Actually Happening?**

1. Your device sends a message to a STUN server.
2. NAT rewrites your private IP:port into public IP:port.
3. The STUN server sees the public IP:port and tells you, “Here's how you appear to the outside world.”
4. Your app now knows what to tell another peer for direct connection attempts.

**✅ Final Thought**

So no,  **STUN doesn’t replace NAT** , even temporarily. It just **uses NAT's translation** and helps your device  **discover what that translation was** .

-- Without NAT, STUN has no job. Without STUN, your app can't know how to work with NAT.

#### **❄️ What are  ICE Candidates ?**

**📘 Definition:**

**ICE (Interactive Connectivity Establishment)** candidates are a list of potential network connection paths your browser collects to try and reach another peer.

There can be multiple ICE candidate---- This is because your system  **can have multiple IP addresses** , and **ICE candidates** in WebRTC take advantage of that. Here’s how and why:

✅ **Types of IP addresses your system may have:**

| Type                          | Example           | Description                                                  |
| ----------------------------- | ----------------- | ------------------------------------------------------------ |
| **Local IP**            | `192.168.1.5`   | Assigned by your router (used on LAN).                       |
| **Public IP (via NAT)** | `122.56.78.91`  | Seen by the internet. Usually your router's IP.              |
| **IPv6 address**        | `fe80::1abc...` | Modern IP version; might be assigned by the ISP.             |
| **Loopback address**    | `127.0.0.1`     | Refers to your own system (localhost).                       |
| **VPN address**         | Depends on VPN    | Virtual IP if you’re connected to a VPN.                    |
| **Wi-Fi + Ethernet**    | Separate IPs      | If you’re connected to both, each interface has its own IP. |

**🌐 Why multiple IPs matter in WebRTC?**

WebRTC needs to **figure out the best path** to send data peer-to-peer. It collects:

* **Local candidates** (e.g. `192.168.x.x`)
* **Public NAT candidates** via  **STUN** (will see STUN later)
* **Relay candidates** via **TURN servers**

**🧠 Example ICE Candidate Set from Your System:**

```
1. 192.168.1.5:54729 (Local IP via Wi-Fi)
2. 10.0.0.2:55000 (Local IP via VPN)
3. 203.0.113.55:62344 (Public IP via STUN)
4. 35.192.22.13:45000 (TURN relay server)

```

> WebRTC then **negotiates** with the other peer to find the best match that works for both.

**🧩 Each ICE candidate includes:**

* **IP address** (local or public)
* **Port number**
* **Transport protocol** (usually UDP)

The browser generates these candidates and exchanges them with the other peer during the WebRTC setup.

**🏗 Types of ICE Candidates:**

| Type                       | Description                                                               | Example                             |
| -------------------------- | ------------------------------------------------------------------------- | ----------------------------------- |
| **Host**             | Your local IP address.                                                    | `192.168.1.2`                     |
| **Server Reflexive** | Your**public IP**discovered via a **STUN server** .           | `103.56.91.13`                    |
| **Relay**            | A public IP/port provided by a**TURN server**(used if others fail). | `relay.tokyo.turnserver.com:3478` |

**🔄 How ICE Candidates Work in WebRTC**

Here’s how the process typically goes:

1. **Each peer gathers ICE candidates** using the `RTCPeerConnection` API.
2. As candidates are found, they're sent to the other peer through the **signaling server** (Socket.IO, WebSocket, etc.).
3. Both peers try all possible connection combinations from their own candidates + the ones received.
4. WebRTC automatically **chooses the most efficient working path** (host > srflx > relay).

**📊 Example Timeline**

Let’s say Alice and Bob want to connect via WebRTC.

1. Alice gathers these candidates:
   * Host: `192.168.1.4:5000`
   * Server Reflexive: `103.88.21.18:64034`
2. Bob gathers:
   * Host: `10.0.0.2:5023`
   * Server Reflexive: `104.54.29.10:51920`
3. They exchange candidates through the signaling server.
4. They try connecting:
   * Alice host → Bob host ❌ (not reachable)
   * Alice reflexive → Bob reflexive ✅ (success!)

#### 🔹 Peer Connection (RTCPeerConnection)

* RTCPeerConnection-- Core API to manage connection, ICE, SDP, media, data.
* **Each peer gathers ICE candidates** using the `RTCPeerConnection` API.
* **This uses** STUN servers internally.

`RTCPeerConnection` is a **JavaScript API** provided by the browser to:

* Establish peer-to-peer audio, video, or data channels.
* Handle ICE candidate gathering and connection.
* Use STUN and TURN under the hood via the  **ICE framework** .

You use `RTCPeerConnection` like this:

```javascript
const pc = new RTCPeerConnection({
  iceServers: [
    { urls: "stun:stun.l.google.com:19302" }  // this is STUN!
  ]
});

```

Here, the `iceServers` config includes a  **STUN server** , which will be used to gather  **ICE candidates** .

**✅ Summary**

* `RTCPeerConnection` is **not** STUN.
* It is a **browser API** that **uses STUN** (and optionally TURN) as part of ICE (Interactive Connectivity Establishment) to allow P2P connections.
* The STUN server helps the browser discover what its **public IP and port** are.

#### 🛑 Firewall

A **firewall** is a security system—either  **hardware** ,  **software** , or both—that **monitors and controls incoming and outgoing network traffic** based on predefined rules.

**🔒 Purpose of a Firewall**

Its main goal is to:

* **Protect** your computer or network from unauthorized access.
* **Block harmful traffic** like viruses, malware, or hacking attempts.
* **Allow only trusted traffic** to enter or leave your system.

#### TURN

**TURN** stands for **Traversal Using Relays around NAT** — it's a protocol used in **WebRTC** to help devices  **communicate reliably when direct peer-to-peer connections fail** , especially due to restrictive NATs or firewalls.

**📌 Why TURN is Needed in WebRTC**

WebRTC is designed for **peer-to-peer** communication (e.g., video calls, file sharing), so two users can connect directly to reduce latency and server load.

However, in the real world:

* Many users are behind **NATs** (Network Address Translators) or  **firewalls** .
* Some NAT types (like  **Symmetric NAT** ) make **direct peer-to-peer (P2P)** communication impossible.
* **STUN** servers help most of the time by finding public IP addresses, but  **sometimes that’s not enough** .

That's where **TURN** comes in.

**🔄 What TURN Does**

* TURN **relays the media traffic** (audio/video/data) **through a server** when peers  **can’t connect directly** .
* This ensures  **100% connection reliability** , even in  **hostile network conditions** .

WebRTC is designed for  **peer-to-peer communication** , meaning your device should connect **directly** to the other person’s device for things like video, audio, or data.

-- But sometimes, because of **firewalls** or  **complex NATs** , that direct connection  **fails** .

**🔁 So What Does TURN Do?**

When a direct connection fails, TURN steps in and **acts as a middleman** — a  **relay server** .

Let’s say you are Alice, and you're trying to video call Bob:

**
    ✅ Ideal (P2P) situation:**

    Alice  <---->  Bob

    The media (video/audio) flows**directly** between both peers.

    ❌ But with a restrictive NAT or firewall:

    Alice  xxxxx>  Bob  ❌ (No direct route)

    ✅ With TURN:

    Alice  --->  TURN Server  --->  Bob

The TURN server **receives** media from Alice and **forwards** it to Bob, and vice versa.

* It's no longer  **peer-to-peer** , but  **peer-to-TURN-to-peer** .
* It guarantees the call  **will work** , even in the worst network conditions.

**🔥 Important Notes**

* **TURN is a fallback** , not the first option. TURN is **costly** (bandwidth + latency).
* It ensures connection in  **enterprise networks** ,  **carrier-grade NATs** , or **double NAT** environments.
* Running a TURN server requires:
  * Good bandwidth
  * Proper security
  * Authentication setup (usually via **coturn** or  **twilio** )

**🧑‍💻 Example: WebRTC Config with STUN & TURN**

```javascript
const iceServers = {
  iceServers: [
    { urls: "stun:stun.l.google.com:19302" }, // STUN
    {
      urls: "turn:turn.myserver.com:3478",    // TURN
      username: "user",
      credential: "pass"
    }
  ]
};

const peer = new RTCPeerConnection(iceServers);

```

### 🔄 Step-by-Step: WebRTC Connection Process

Let’s break it down into  **5 major steps** :

#### **1. Signaling (Exchange Metadata via Server)**

Before a peer connection can be established, the two peers must  **exchange connection information** , including:

* **Session Descriptions** (SDP - offer/answer)
* **ICE Candidates** (IP addresses + ports for connection)

> Signaling is not defined by WebRTC. You can use  **Socket.IO, WebSockets, HTTP** , or any custom messaging system.

Example-- `socket.emit('offer', { sdp, iceCandidates }); // example signaling`

#### **2. Media Capture (Optional)**

If you’re doing audio/video:

`const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });`

This stream can then be added to the peer connection:

`peerConnection.addTrack(stream.getTracks()[0], stream);`

#### **3. Creating and Exchanging Offer/Answer**

One peer (caller) creates an offer:

```javascript
const offer = await peerConnection.createOffer();
await peerConnection.setLocalDescription(offer);

```

This offer is sent to the other peer through the signaling server.

The second peer (callee) receives the offer, creates an answer:

```javascript
await peerConnection.setRemoteDescription(offer);
const answer = await peerConnection.createAnswer();
await peerConnection.setLocalDescription(answer);

```

And sends it back.

#### **4. ICE Candidate Exchange (Finding the Best Network Path)**

Each peer collects ICE candidates — possible ways to reach the other peer (local, public, relay IPs):

```javascript
peerConnection.onicecandidate = (event) => {
  if (event.candidate) {
    socket.emit('ice-candidate', event.candidate); // send via signaling server
  }
};

```

Peers exchange these ICE candidates and try to connect using the best available route.

#### **5. Media or Data Transmission Begins**

Once ICE negotiation succeeds and connection is established,  **media or data flows directly peer-to-peer** :

* `ontrack`: used for media
* `ondatachannel`: used for file/data/chat

```javascript
peerConnection.ontrack = (event) => {
  remoteVideo.srcObject = event.streams[0];
};

const dataChannel = peerConnection.createDataChannel("chat");
dataChannel.onmessage = (e) => console.log("Message from peer:", e.data);

```

## 💬 Example Use Case – Chat App with Video

1. User A opens the app and grants camera/mic access.
2. User A creates a WebRTC offer.
3. User A sends the offer to User B via Socket.IO.
4. User B receives the offer, creates an answer, sends it back.
5. Both exchange ICE candidates.
6. Once connection is established, they start chatting or sharing video.

# EXAMPLE VIDEO CHAT PROGRAM WITH DETAILED EXPLANATION

### SocketProvider.jsx----

```javascript
import React, { createContext, useMemo, useContext } from "react";
import { io } from "socket.io-client";

const SocketContext = createContext(null);

export const useSocket = () => {
  const socket = useContext(SocketContext);
  return socket;
};

export const SocketProvider = (props) => {
  const socket = useMemo(() => io("localhost:8000"), []);

  return (
    <SocketContext.Provider value={socket}>
      {props.children}
    </SocketContext.Provider>
  );
};
```

### index.js

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { SocketProvider } from "./context/SocketProvider";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <SocketProvider>
        <App />
      </SocketProvider>
    </BrowserRouter>
  </React.StrictMode>
);

```

### App.js

```javascript
import { Routes, Route } from "react-router-dom";
import "./App.css";
import LobbyScreen from "./screens/Lobby";
import RoomPage from "./screens/Room";

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path="/" element={<LobbyScreen />} />
        <Route path="/room/:roomId" element={<RoomPage />} />
      </Routes>
    </div>
  );
}

export default App;
```

### Lobby.jsx

```javascript
import React, { useState, useCallback, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { useSocket } from "../context/SocketProvider";

const LobbyScreen = () => {
  const [email, setEmail] = useState("");
  const [room, setRoom] = useState("");

  const socket = useSocket();
  const navigate = useNavigate();

  const handleSubmitForm = useCallback(
    (e) => {
      e.preventDefault();
      socket.emit("room:join", { email, room });
    },
    [email, room, socket]
  );

  const handleJoinRoom = useCallback(
    (data) => {
      const { email, room } = data;
      navigate(`/room/${room}`);
    },
    [navigate]
  );

  useEffect(() => {
    socket.on("room:join", handleJoinRoom);
    return () => {
      socket.off("room:join", handleJoinRoom);
    };
  }, [socket, handleJoinRoom]);

  return (
    <div>
      <h1>Lobby</h1>
      <form onSubmit={handleSubmitForm}>
        <label htmlFor="email">Email ID</label>
        <input
          type="email"
          id="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <br />
        <label htmlFor="room">Room Number</label>
        <input
          type="text"
          id="room"
          value={room}
          onChange={(e) => setRoom(e.target.value)}
        />
        <br />
        <button>Join</button>
      </form>
    </div>
  );
};

export default LobbyScreen
```

Here ----

`useCallback(fn, deps)` is a React hook that returns a  **memoized version of the callback function** , which means:

* The function is *only recreated* if one of the dependencies changes.
* It helps prevent unnecessary re-renders or re-subscriptions in child components or hooks that depend on function identity (like `useEffect`, `useMemo`, etc.).

You pass `handleJoinRoom` to the `socket.on()` inside a `useEffect`. Without `useCallback`, `handleJoinRoom` would be  **recreated on every render** , which would:

* Cause the `useEffect` cleanup to run and re-subscribe every time.
* Result in  **unnecessary `socket.off` and `socket.on` calls** , potentially causing bugs like multiple listeners being attached.

So here, `useCallback` is **very important** to keep the reference stable between renders and make the effect work correctly.

However for handleSubmitform()

* `handleSubmitForm` is only used as an `onSubmit` handler for the form.
* You **don’t need it to be memoized** unless:
  * You were passing it to a child component that depended on reference equality for optimization (`React.memo`, `useEffect`, etc.).

So, in this case, `useCallback` is  **not strictly necessary** , but it doesn't hurt performance either. It might be used here for consistency or future-proofing if you later refactor or optimize.

### peer.js

```javascript
class PeerService {
  constructor() {
    if (!this.peer) {
      this.peer = new RTCPeerConnection({
        iceServers: [
          {
            urls: [
              "stun:stun.l.google.com:19302",
              "stun:global.stun.twilio.com:3478",
            ],
          },
        ],
      });
    }
  }
  
  async getOffer() {
    if (this.peer) {
      const offer = await this.peer.createOffer();
      await this.peer.setLocalDescription(offer);
      return offer;
    }
  }

  async getAnswer(offer) {
    if (this.peer) {
      await this.peer.setRemoteDescription(offer);
      const ans = await this.peer.createAnswer();
      await this.peer.setLocalDescription(new RTCSessionDescription(ans)); // or await this.peer.setRemoteDescription(ans)
      return ans;
    }
  }

  async setRemoteAnswer(ans) {
    if (this.peer) {
      await this.peer.setRemoteDescription(new RTCSessionDescription(ans)); // or await this.peer.setRemoteDescription(ans)
    }
  }

}

export default new PeerService(
```

This class acts as a **helper utility** to manage the `RTCPeerConnection` lifecycle in a simpler, reusable, and centralized way.

Instead of repeating the verbose WebRTC code (`createOffer`, `setLocalDescription`, etc.) in every React component, you isolate it into one service file: `peer.js`.

**What’s Happening Here:**

* A constructor is being called when you instantiate the class.
* Inside it, you're creating a new `RTCPeerConnection` and saving it to `this.peer`.

`iceServers`

* This config tells WebRTC which **STUN (Session Traversal Utilities for NAT)** servers to use.
* STUN servers help your browser discover its **public IP and port** when behind a NAT or firewall — so peers can try to connect directly.
* Why it's called `iceServers` instead of `stunServers`?

  * ICE is the overall **framework** that WebRTC uses to establish a peer-to-peer connection  **through or around NATs and firewalls** .Under this framework, ICE can use **two kinds of servers** to help with connectivity:

| Server Type                                         | Purpose                                                                                | Example URL                      |
| --------------------------------------------------- | -------------------------------------------------------------------------------------- | -------------------------------- |
| **STUN**(Session Traversal Utilities for NAT) | Helps a peer discover its**public IP and port**                                  | `stun:stun.l.google.com:19302` |
| **TURN**(Traversal Using Relays around NAT)   | **Relays traffic**when direct peer-to-peer connection fails (used as a fallback) | `turn:turn.example.com:3478`   |

So, even if you’re using  **only STUN** , the property is still called `iceServers` because WebRTC is prepared to work with both **STUN** and  **TURN** , depending on the situation.

🔹 In other words:

* `iceServers` is a **list of all possible servers ICE can use** to gather candidates.
* These candidates are used to determine the best network path between peers.
* That list may include:
  * Only STUN
  * Only TURN
  * Or both

Example used here:  `stun:stun.l.google.com:19302`

> This is a free STUN server provided by Google.

-- `getOffer()`

This function handles:

1. `createOffer()` — Asks WebRTC to create an **SDP offer** describing local media capabilities (video, audio, etc.).
2. `setLocalDescription(offer)` — Sets this offer as your **own description** (so ICE candidates can be gathered and signaling can proceed).

   * ICE candidates can (and often do) start gathering even before an answer is received.
   * **Once `setLocalDescription()` is called** , ICE candidate gathering begins **immediately** in the background.
   * When you call `setLocalDescription(offer)`, you're making a commitment with the SDP obbject u received and telling the WebRTC engine:

     > "Okay, I'm officially using this configuration. Begin ICE gathering."
     >
3. Then it **returns the offer** (to be sent via signaling to the remote peer).

> This is typically called by the person **initiating the call** (caller).

-- `getAnswer(offer)`

This is used by the **receiver** of the call (callee):

1. `setRemoteDescription(offer)` — Sets the caller’s offer as the remote description.
2. `createAnswer()` — Generates an SDP answer compatible with the offer.
3. `setLocalDescription(new RTCSessionDescription(ans))` — Sets this answer as your own local description.

   * `RTCSessionDescription` is a built-in WebRTC constructor used to create a **session description object** that can be passed to:
     `peerConnection.setLocalDescription()` and `peerConnection.setRemoteDescription()`
     * It wraps the **SDP (Session Description Protocol)** content that defines the configuration of a peer connection — like codecs, media types, encryption, etc.
   * In  **older WebRTC APIs** , `setRemoteDescription()` and `setLocalDescription()` required the input to be an instance of `RTCSessionDescription`. However...Modern WebRTC allows you to skip this. Hence this is fine too -- `await peerConnection.setRemoteDescription(offer); `
4. Then it returns the answer, to send back to the caller.

-- `export default new PeerService();`

You export a **single instance** of this class (a singleton). This ensures that both peers operate on the **same `RTCPeerConnection` object** throughout the app.

**⚡ Benefits of using a class:**

| ✅ Advantage             | 💬 What it means                                                                               |
| ------------------------ | ---------------------------------------------------------------------------------------------- |
| **Organized code** | All WebRTC logic lives in one place.                                                           |
| **Reusable**       | You can call `peer.getOffer()`,`peer.getAnswer()`from any file that imports it.            |
| **Stateful**       | You can maintain internal state, like `this.peer`for the RTCPeerConnection object.           |
| **Encapsulation**  | Internal variables like `peer`, ICE candidates, or custom flags stay protected in the class. |

In contrast, if you just wrote functions like:

```javascript
let peer = new RTCPeerConnection();

function getOffer() { ... }
function setAnswer() { ... }

```

— you're managing global variables. That gets messy quickly.

**✅  Why export a singleton? (`new PeerService()`)**

`export default new PeerService();` creates **one single shared instance** of `PeerService` across the entire app.

Why we need a singleton for WebRTC:

WebRTC peer connections involve a single `RTCPeerConnection` object per user per call.

* 🔁 If you created a **new `PeerService` object every time** (e.g., `new PeerService()` in multiple components), then:
  * You’d have **multiple** `RTCPeerConnection` objects,
  * Tracks, ICE candidates, or SDP (offer/answer) would not be synchronized properly,
  * Your call would break or behave inconsistently.

    🧠  Singleton = shared brain for WebRTC

By exporting `new PeerService()` once:

* All React components use the  **same WebRTC connection** .
* That means:
  * Only one offer/answer exchange happens,
  * ICE candidates are managed centrally,
  * The peer connection lifecycle is controlled cleanly.

🔁 Analogy---

Think of `PeerService` as the **walkie-talkie** device.

* 🧑‍🚀 If Alice and Bob are talking in a call,
* Both sides should interact through **one walkie-talkie** instance,
* Not create a new one each time they say something.

If you used a new walkie-talkie every time you said “Hello,” it wouldn't work.

### Room.jsx

```javascript
import React, { useEffect, useCallback, useState } from "react";
import ReactPlayer from "react-player";
import peer from "../service/peer";
import { useSocket } from "../context/SocketProvider";

const RoomPage = () => {
  const socket = useSocket();
  const [remoteSocketId, setRemoteSocketId] = useState(null);
  const [myStream, setMyStream] = useState();
  const [remoteStream, setRemoteStream] = useState();

  const handleUserJoined = useCallback(({ email, id }) => {
    console.log(`Email ${email} joined room`);
    setRemoteSocketId(id);
  }, []);

  const handleCallUser = useCallback(async () => {
    const stream = await navigator.mediaDevices.getUserMedia({
      audio: true,
      video: true,
    });
    const offer = await peer.getOffer();
    socket.emit("user:call", { to: remoteSocketId, offer });
    setMyStream(stream);
  }, [remoteSocketId, socket]);

  const handleIncommingCall = useCallback(
    async ({ from, offer }) => {
      setRemoteSocketId(from);
      const stream = await navigator.mediaDevices.getUserMedia({
        audio: true,
        video: true,
      });
      setMyStream(stream);
      console.log(`Incoming Call`, from, offer);
      const ans = await peer.getAnswer(offer);
      socket.emit("call:accepted", { to: from, ans });
    },
    [socket]
  );

  const sendStreams = useCallback(() => {
    for (const track of myStream.getTracks()) {
      peer.peer.addTrack(track, myStream);
    }
  }, [myStream]);

  const handleCallAccepted = useCallback(
    ({ from, ans }) => {
      peer.setRemoteAnswer(ans);
      console.log("Call Accepted!");
      sendStreams();
    },
    [sendStreams]
  );

  const handleNegoNeeded = useCallback(async () => {
    const offer = await peer.getOffer();
    socket.emit("peer:nego:needed", { offer, to: remoteSocketId });
  }, [remoteSocketId, socket]);

  useEffect(() => {
    peer.peer.addEventListener("negotiationneeded", handleNegoNeeded);
    return () => {
      peer.peer.removeEventListener("negotiationneeded", handleNegoNeeded);
    };
  }, [handleNegoNeeded]);

  const handleNegoNeedIncomming = useCallback(
    async ({ from, offer }) => {
      const ans = await peer.getAnswer(offer);
      socket.emit("peer:nego:done", { to: from, ans });
    },
    [socket]
  );

  const handleNegoNeedFinal = useCallback(async ({ ans }) => {
    await peer.setRemoteAnswer(ans);
  }, []);

  useEffect(() => {
    peer.peer.addEventListener("track", async (ev) => {
      const remoteStream = ev.streams;
      console.log("GOT TRACKS!!");
      setRemoteStream(remoteStream[0]);
    });
  }, []);

  useEffect(() => {
    socket.on("user:joined", handleUserJoined);
    socket.on("incomming:call", handleIncommingCall);
    socket.on("call:accepted", handleCallAccepted);
    socket.on("peer:nego:needed", handleNegoNeedIncomming);
    socket.on("peer:nego:final", handleNegoNeedFinal);

    return () => {
      socket.off("user:joined", handleUserJoined);
      socket.off("incomming:call", handleIncommingCall);
      socket.off("call:accepted", handleCallAccepted);
      socket.off("peer:nego:needed", handleNegoNeedIncomming);
      socket.off("peer:nego:final", handleNegoNeedFinal);
    };
  }, [
    socket,
    handleUserJoined,
    handleIncommingCall,
    handleCallAccepted,
    handleNegoNeedIncomming,
    handleNegoNeedFinal,
  ]);

  return (
    <div>
      <h1>Room Page</h1>
      <h4>{remoteSocketId ? "Connected" : "No one in room"}</h4>
      {myStream && <button onClick={sendStreams}>Send Stream</button>}
      {remoteSocketId && <button onClick={handleCallUser}>CALL</button>}
      {myStream && (
        <>
          <h1>My Stream</h1>
          <ReactPlayer
            playing
            muted
            height="100px"
            width="200px"
            url={myStream}
          />
        </>
      )}
      {remoteStream && (
        <>
          <h1>Remote Stream</h1>
          <ReactPlayer
            playing
            muted
            height="100px"
            width="200px"
            url={remoteStream}
          />
        </>
      )}
    </div>
  );
};

export default RoomPage;
```

> For good details on navigator.mediaDevices.getUserMedia(), See Javascript.md

**🔄 Quick Flow Overview of Your WebRTC App**

Here’s what’s happening, step-by-step:

1. **User A joins a room** .
2. **User B joins later** , triggering a `"user:joined"` event → sets `remoteSocketId`.
3. **User A clicks "CALL"** → gets media stream → creates offer → emits `user:call` via socket to User B.
4. **User B receives `incomming:call`** → gets media stream → generates answer → emits `call:accepted`.
5. **User A receives `call:accepted`** → sets remote answer → sends local tracks.
6. WebRTC starts P2P streaming → `track` event receives and plays remote stream.
7. Renegotiation (e.g., adding more tracks later) is handled via `"negotiationneeded"` → exchange of new offer/answer via `"peer:nego:*"` events.

--`handleCallAccepted()`

🔹 Purpose:

When  **User B accepts the call** , they send an SDP **answer** via socket. User A uses this handler to:

* Set the received `answer` as the **remote description** in the RTCPeerConnection.
* Call `sendStreams()` to send local audio/video tracks to User B.

This finalizes the peer connection setup from User A's side.

--`peer.peer.addEventListener("negotiationneeded", handleNegoNeeded) + handleNegoNeede()`

🔹 Purpose:

WebRTC **automatically emits** a `negotiationneeded` event when something changes in the peer connection that requires re-negotiation (e.g., adding tracks).

This listener:

* Creates a new offer (`peer.getOffer()` calls `createOffer` and sets local description).
* Emits the offer to the other peer via `"peer:nego:needed"` socket event.

The other peer will then generate an answer and complete the renegotiation.

-- `handleNegoNeedFinal()`

🔹 Purpose:

Used during  **renegotiation** . After an updated offer is sent and accepted (e.g., due to new tracks being added or codecs being renegotiated), this handler:

* Applies the **final answer** to the peer connection using `setRemoteAnswer`.

This completes the new round of negotiation.

-- `peer.peer.addEventListener("track", async (ev) => { ... })`

🔹 Purpose:

This listener is triggered when **remote media tracks** (audio/video) are received from the other peer.

* `ev.streams` contains MediaStreams associated with the received tracks.
* The first one (`ev.streams[0]`) is usually the full incoming remote stream.
* This is stored in state (`setRemoteStream`), which ReactPlayer uses to display the remote video.

Instead of ReactPlayer, you could use `<video>`  --

    Best for WebRTC:`<video>` element

> Beacuse:
>
> * ✔️ Lightweight
> * ✔️ Made for live streams (`MediaStream`)
> * ✔️ Fully controlled
> * ✔️ No extra dependency
>
> ❌ Why **not** use `ReactPlayer` for WebRTC?
>
> `ReactPlayer` is a **third-party React wrapper** designed for playing: YouTube videos, Vimeo, Twitch, Local/remote MP4s,  Audio files, HLS/DASH streams. It is **not meant for attaching a live `MediaStream`** (which is what WebRTC gives you).
>
> ### Issues with `ReactPlayer` for WebRTC:
>
> * ❌ Doesn’t natively support `MediaStream` objects
> * ❌ Adds unnecessary overhead and abstraction
> * ❌ Not designed for two-way live video calling

```html
{myStream && (
        <>
          <h1>My Stream</h1>
          <video
            autoplay
            muted
	    playsInline
      	    style={{ width: "200px", height: "100px" }}
            src={myStream}
          />
        </>
      )}
      {remoteStream && (
        <>
          <h1>Remote Stream</h1>
          <video
            autoplay
            muted
	    playsInline
      	    style={{ width: "200px", height: "100px" }}
            src={remoteStream}
          />
        </>
      )}
```

### server / index.js

```javascript
const { Server } = require("socket.io");

const io = new Server(8000, {
  cors: true,
});

const emailToSocketIdMap = new Map();
const socketidToEmailMap = new Map();

io.on("connection", (socket) => {
  console.log(`Socket Connected`, socket.id);
  socket.on("room:join", (data) => {
    const { email, room } = data;
    emailToSocketIdMap.set(email, socket.id);
    socketidToEmailMap.set(socket.id, email);
    io.to(room).emit("user:joined", { email, id: socket.id });
    socket.join(room);
    io.to(socket.id).emit("room:join", data);
  });

  socket.on("user:call", ({ to, offer }) => {
    io.to(to).emit("incomming:call", { from: socket.id, offer });
  });

  socket.on("call:accepted", ({ to, ans }) => {
    io.to(to).emit("call:accepted", { from: socket.id, ans });
  });

  socket.on("peer:nego:needed", ({ to, offer }) => {
    console.log("peer:nego:needed", offer);
    io.to(to).emit("peer:nego:needed", { from: socket.id, offer });
  });

  socket.on("peer:nego:done", ({ to, ans }) => {
    console.log("peer:nego:done", ans);
    io.to(to).emit("peer:nego:final", { from: socket.id, ans });
  });
});
```

Here emailToSocketIdMap and socketidToEmailMap are used so that

* When a user joins, we **remember their socket ID** for their email.
* And we **remember their email** if we only have their socket ID.

🤔 So why aren’t they used anywhere?

But here’s the key insight:

✅ These maps are  **likely meant for use in future logic** , like:

🔸 1. Sending a message to a user by email:

Instead of hardcoding a socket ID:

```javascript
const socketId = emailToSocketIdMap.get(email);
io.to(socketId).emit("some:event", data);

```

Removing users on disconnect:

🔸 2. Removing users on disconnect:

```javascript
socket.on("disconnect", () => {
  const email = socketidToEmailMap.get(socket.id);
  emailToSocketIdMap.delete(email);
  socketidToEmailMap.delete(socket.id);
});

```

This helps you **clean up** and manage connected users properly.

🔸 3. Displaying online users (example feature): `const onlineUsers = Array.from(emailToSocketIdMap.keys());`

You could use this to show who's online in a chat or video app.

> ***If you're only doing simple video calls now, they may seem unused, but they're foundational for scaling later.***

##### 🔧 High-Level Overview

We are using:

* **WebRTC** : for real-time peer-to-peer video/audio streaming.
* **Socket.IO** : for signaling — the process of setting up, managing, and tearing down WebRTC connections (exchange of SDP offers/answers and ICE candidates).

##### 🔁 Full Call Flow Walkthrough

**1. Room Join**

* User A joins room: emits `room:join`.
* Server maps email ↔ socket.id and emits `user:joined`.

**2. User B Joins**

* A gets notified via `user:joined`.
* A now knows B's socket ID and can initiate a call.

**3. A Calls B**

* A gets local stream (`getUserMedia`) and emits `user:call` with an  **offer** .
* Server forwards this to B as `incomming:call`.

**4. B Receives Call**

* B gets local stream and calls `peer.getAnswer(offer)`.
* B emits `call:accepted` with  **answer** .
* Server forwards this to A.

**5. A Receives Answer**

* A sets `setRemoteAnswer(ans)`.
* A sends its stream using `sendStreams()`.

**6. Both Streams Flow**

* Each peer gets the remote stream via the `"track"` event and shows video.

**7. Renegotiation (optional)**

* If one peer adds a track (e.g., screen share), `negotiationneeded` triggers.
* Offer → `peer:nego:needed` → Answer → `peer:nego:done` → `peer:nego:final`

# All Important and Foundational Methods used in the above Program

### `RTCPeerConnection()` - Peer Connection

The `RTCPeerConnection` constructor in WebRTC is used to create a new peer-to-peer connection. It takes  **one optional parameter** : a  **configuration object** . So technically, it has **one parameter** — but that object can contain  **several fields** .

✅ Syntax:   `const pc = new RTCPeerConnection([configuration]);`

There are many fields in the object. The most commly used are below---

| Field                    | Type                                   | Description                                                                                             |
| ------------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `iceServers`           | `Array`                              | List of STUN/TURN servers used to establish the connection.                                             |
| `bundlePolicy`         | `string`                             | `"balanced"`,`"max-compat"`, or `"max-bundle"`— controls how tracks are bundled over transports. |
| `iceCandidatePoolSize` | `number`                             | Size of the ICE candidate pool (default is 0).                                                          |
| `sdpSemantics`         | `string`                             | `"unified-plan"`(default) or `"plan-b"`— describes SDP formatting.                                 |
| `portRange`            | `object`(non-standard, Firefox-only) | `{min: number, max: number}`— range of local UDP ports.                                              |

EXAMPLE--

```javascript
const pc = new RTCPeerConnection({
  iceServers: [
    { urls: 'stun:stun.l.google.com:19302' },
    {
      urls: 'turn:turn.example.com',
      username: 'user',
      credential: 'pass'
    }
  ],
  iceCandidatePoolSize: 10,
  bundlePolicy: 'balanced',
  sdpSemantics: 'unified-plan'
});

```

### peer.createOffer()

The `createOffer()` method of the `RTCPeerConnection` interface generates an SDP (Session Description Protocol) offer to initiate a WebRTC connection.

✅ Syntax: `peerConnection.createOffer(options)`

🔹 Parameters:

It takes  **one optional parameter** : an object called `options` (of type `RTCOfferOptions`), which can contain the following fields:

| Parameter                  | Type        | Default   | Description                                             |
| -------------------------- | ----------- | --------- | ------------------------------------------------------- |
| `iceRestart`             | `boolean` | `false` | Forces ICE restart (renegotiates connection).           |
| `voiceActivityDetection` | `boolean` | `true`  | Controls whether to use voice activity detection (VAD). |

### peer.createAnswer()

The `createAnswer()` method of the `RTCPeerConnection` interface is used to create an SDP **answer** in response to an SDP **offer** from a remote peer.

🔹 Parameters:

`createAnswer()` takes **one optional parameter** — an object of type `RTCAnswerOptions`.

| Parameter                  | Type        | Default  | Description                                                                |
| -------------------------- | ----------- | -------- | -------------------------------------------------------------------------- |
| `voiceActivityDetection` | `boolean` | `true` | Whether to include voice activity detection (VAD) when generating the SDP. |

### peer.setLocalDescription(offer)

The method `setLocalDescription()` of the `RTCPeerConnection` interface is used to set the local description (offer or answer) for a WebRTC peer connection.

✅ **Syntax:   `peerConnection.setLocalDescription(description)`**

⚠️ Notes:

* `setLocalDescription()` starts ICE candidate gathering  **if not already started** .
* It returns a `Promise<void>` — it doesn't return anything on success, but it **throws** if the description is invalid or incompatible with the current signaling state.

### peer.peer.addTrack(track, streams)

🔷 Why `peer.peer.addTrack(...)` instead of `peer.addTrack(...)` IN THE ABOVE EXAMPLE PROGRAM?

You’re importing an **instance** of a class called `PeerService`:   `import peer from "../service/peer"; // <- instance of PeerService`

This instance (`peer`) contains a **property** called `peer`, which holds the actual `RTCPeerConnection` object:

```javascript
class PeerService {
  constructor() {
    if (!this.peer) {
      this.peer = new RTCPeerConnection(...); // <-- here
    }
  }
}

```

Similarly:

* `peer.addTrack(...)` would **only work** if you had defined `addTrack` directly in your `PeerService` class.
* But `addTrack` is a method of `RTCPeerConnection`, not of your service class — and your `PeerService` instance holds the `RTCPeerConnection` inside `this.peer`.

So you must access:    `peer.peer.addTrack(track, myStream);`

The `.addTrack()` method of the `RTCPeerConnection` interface is used to add a media track (like audio or video) to a WebRTC connection.

✅ **Syntax:**    `RTCRtpSender = RTCPeerConnection.addTrack(track, ...streams);`

🔹 **Parameters**

| Parameter      | Type                                                                         | Required                          | Description                                                                                                                                 |
| -------------- | ---------------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `track`      | `MediaStreamTrack`                                                         | ✅ Yes                            | The media track you want to send (e.g., audio or video).                                                                                    |
| `...streams` | [`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream) | ⛔ Optional, but usually included | One or more media streams that the track will be<br /> part of. This helps the browser understand which<br /> streams the track belongs to. |

* You can pass **one or more** MediaStream objects.
* Internally, the track will be added to the peer connection and associated with the stream(s).

🔁 **Return Value**

Returns an [`RTCRtpSender`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpSender) object that can be used to modify or stop the track later.

✅ **Example**

```javascript
const pc = new RTCPeerConnection();
const stream = await navigator.mediaDevices.getUserMedia({ video: true });
const videoTrack = stream.getVideoTracks()[0];

const sender = pc.addTrack(videoTrack, stream);

```

### `peer.peer.addEventListener("negotiationneeded", ...)`

✅ What it does:

This event is fired when **WebRTC determines** that a renegotiation is needed. It typically happens  **after tracks are added** , and the connection needs to renegotiate the session.

💡 Use case:

For example, when you call:   `peer.peer.addTrack(track, stream);`

WebRTC automatically triggers the `"negotiationneeded"` event because adding a track changes the media stream, which requires renegotiation.

📦 Example:

```javascript
peer.peer.addEventListener("negotiationneeded", async () => {
  const offer = await peer.getOffer();
  // send offer to remote peer over socket
});

```

🧠 Why it's useful:

You don’t manually have to detect when to call `createOffer()` — this event tells you, “Hey! The connection has changed. Time to create a new offer.”

### `peer.peer.addEventListener("track", ...)`

✅ What it does:

This event fires **when a remote track is received** from the other peer.

💡 Use case:

When the other peer adds a track and sends it across the WebRTC connection, you’ll receive it here and can attach it to a media element like `<video>`.

📦 Example:

```javascript
peer.peer.addEventListener("track", async (event) => {
  const remoteStream = new MediaStream();
  remoteStream.addTrack(event.track);
  remoteVideoRef.current.srcObject = remoteStream;
});

```

If the remote peer sends multiple tracks (like audio + video), this can fire multiple times, or you can add `event.streams[0]` directly to a video tag.

### 👀 Tip for `PeerService` Integration

You can attach both listeners in your class constructor like this:

```javascript
this.peer.addEventListener("negotiationneeded", async () => {
  const offer = await this.getOffer();
  // Send offer to remote user via socket
});

this.peer.addEventListener("track", (event) => {
  const remoteStream = new MediaStream();
  remoteStream.addTrack(event.track);
  // Use this remoteStream in your React video element
});

```

# Events fired from `RTCPeerConnection` object

### 🔹 **1. `icecandidate`**

* **Fires when:** A new ICE candidate is found.
* **Usage:** Send the candidate to the remote peer over your signaling server.

```javascript
peerConnection.addEventListener("icecandidate", (event) => {
  if (event.candidate) {
    // Send to remote peer
  }
});

```

### 🔹 **2. `icecandidateerror`**

* **Fires when:** An error occurs while gathering ICE candidates.
* **Usage:** Useful for debugging network issues.

```javascript
peerConnection.addEventListener("icecandidateerror", (event) => {
  console.error("ICE Candidate Error:", event.errorText);
});

```

### 🔹 **3. `connectionstatechange`**

* **Fires when:** The overall connection state changes (`connected`, `disconnected`, etc.)
* **Usage:** Monitor for disconnections or connection failures.

```javascript
peerConnection.addEventListener("connectionstatechange", () => {
  console.log(peerConnection.connectionState);
});

```

### 🔹 **4. `signalingstatechange`**

* **Fires when:** The signaling state (`stable`, `have-local-offer`, etc.) changes.
* **Usage:** Helps debug the signaling process.

```javascript
peerConnection.addEventListener("signalingstatechange", () => {
  console.log(peerConnection.signalingState);
});

```

### 🔹 **5. `iceconnectionstatechange`**

* **Fires when:** The ICE connection state changes (`checking`, `connected`, `failed`, etc.)
* **Usage:** More granular than `connectionstatechange`, specific to ICE negotiation.

```javascript
peerConnection.addEventListener("iceconnectionstatechange", () => {
  console.log(peerConnection.iceConnectionState);
});

```

### 🔹 **6. `icegatheringstatechange`**

* **Fires when:** The ICE gathering state changes (`new`, `gathering`, `complete`).
* **Usage:** To know when candidate gathering is complete.

```javascript
peerConnection.addEventListener("icegatheringstatechange", () => {
  console.log(peerConnection.iceGatheringState);
});

```

### 🔹 **7. `negotiationneeded`**

See  "All Important and Foundational Methods used in the above Program" Section

### 🔹 **8. `track`**

See  "All Important and Foundational Methods used in the above Program" Section

### 🔹 **9. `datachannel`**

* **Fires when:** The remote peer creates a `RTCDataChannel`.
* **Usage:** Accept and bind to the incoming data channel.

```javascript
peerConnection.addEventListener("datachannel", (event) => {
  const channel = event.channel;
  channel.onmessage = (e) => console.log(e.data);
});

```

**✅ Summary Table**

| Event Name                   | Purpose                                                                  |
| ---------------------------- | ------------------------------------------------------------------------ |
| `icecandidate`             | ICE candidate discovered                                                 |
| `icecandidateerror`        | ICE candidate error occurred                                             |
| `connectionstatechange`    | Overall connection status changes (`connected`,`disconnected`, etc.) |
| `signalingstatechange`     | Changes in signaling state (`stable`,`have-local-offer`, etc.)       |
| `iceconnectionstatechange` | ICE-specific connection changes (`checking`,`failed`, etc.)          |
| `icegatheringstatechange`  | ICE candidate gathering phase updates                                    |
| `negotiationneeded`        | Browser triggers a need for SDP renegotiation                            |
| `track`                    | Remote track received (audio/video)                                      |
| `datachannel`              | Remote peer created a data channel                                       |

# All Interfaces, Methods and Properties of webRTC

WebRTC provides several  **core APIs** , primarily through the `RTCPeerConnection`, `MediaStream`, `MediaStreamTrack`, and `RTCDataChannel` interfaces. Here's a comprehensive and clearly categorized list of **methods** and **properties** associated with WebRTC.

### 🚀 Core Interfaces in WebRTC

| Interface                 | Purpose                                  |
| ------------------------- | ---------------------------------------- |
| `RTCPeerConnection`     | Manages WebRTC connection                |
| `MediaStream`           | Represents a stream of media tracks      |
| `MediaStreamTrack`      | Represents individual audio/video tracks |
| `RTCDataChannel`        | Bidirectional data transmission          |
| `RTCIceCandidate`       | Contains ICE candidate info              |
| `RTCSessionDescription` | Contains SDP offer/answer                |

### 🧠 **1. `RTCPeerConnection`**

##### ✅ Properties:

| Property                         | Description                                                                      |
| -------------------------------- | -------------------------------------------------------------------------------- |
| `connectionState`              | State of the connection (`new`,`connected`,`disconnected`, etc.)           |
| `iceConnectionState`           | ICE transport state (`new`,`checking`,`connected`, etc.)                   |
| `iceGatheringState`            | State of ICE candidate gathering (`new`,`gathering`,`complete`, etc.)     |
| `signalingState`               | Signaling state (`stable`,`have-local-offer`, etc.)                          |
| `localDescription`             | The current local description (`RTCSessionDescription`, `null`)              |
| `remoteDescription`            | The current remote description (`RTCSessionDescription`, `null`)             |
| `pendingLocalDescription`      | Pending local description (for rollback) (`RTCSessionDescription`, `null`)  |
| `pendingRemoteDescription`     | Pending remote description (for rollback) (`RTCSessionDescription`, `null`) |
| `currentLocalDescription`      | Current local description (active) (`RTCSessionDescription`, `null`)        |
| `currentRemoteDescription`     | Current remote description (active) (`RTCSessionDescription`, `null`)       |
| `sctp`  (WILL SEE THIS LATER) | SCTP transport used by data channels                                             |
| `canTrickleIceCandidates`      | Whether the peer supports trickling ICE candidates (`true \|\| false`)           |

> **📎`iceGatheringState`**
>
> The `iceGatheringState` is a property of the `RTCPeerConnection` object that tells you the current state of the ICE candidate gathering process. It can have one of  **three values** :
>
> 🔹 `new`
>
> * **What it means** : ICE gathering has  **not yet started** .
> * **When you see this** : Before `setLocalDescription()` is called.
>
> 🔹 `gathering`
>
> * **What it means** : The browser is **currently collecting** ICE candidates.
> * **When you see this** : Immediately after calling `setLocalDescription()`.
>
> 🔹 `complete`
>
> * **What it means** : ICE candidate gathering is **finished** — no more candidates will be found.
> * **When you see this** : After all candidates have been emitted (including the `null` candidate in the `onicecandidate` event).
>
> **📎`signalingState`**
>
> The `signalingState` is a property of the `RTCPeerConnection` object in WebRTC. It tells you the **current signaling status** of the connection — basically,  **where the connection is in terms of offer/answer negotiation** .
>
> 📌 Why it's important:
>
> It helps you understand what step your connection is at in the signaling process (e.g., have you sent an offer? received an answer? is negotiation done? etc.)
>
> values-- 'stable' (No ongoing offer/answer exchange. Everything is in sync), 'have-local-offer', 'have-remote-offer', 'closed', etc(others are rarely used)
>
> **`📎pendingLocalDescription ` `pendingRemoteDescription` `currentLocalDescription` `currentRemoteDescription` **
>
> `pendingLocalDescription` --- A local description (offer or answer) that  **has been set with `setLocalDescription()`** , but **is not yet stable** (i.e. negotiation is not complete).  Values -- `RTCSessionDescription` or `null`
>
> `currentLocalDescription` --- The local description that is currently in effect (negotiation complete). Values -- `RTCSessionDescription`   or `null`
>
> `📎canTrickleIceCandidates`
>
> If the**remote peer supports "trickle ICE"** — a process where ICE candidates can be sent and received  *incrementally* , as they are found, then its true else false

##### ✅ Methods:

| Method                                                        | Description                                         |
| ------------------------------------------------------------- | --------------------------------------------------- |
| `createOffer()`                                             | Creates an SDP offer                                |
| `createAnswer()`                                            | Creates an SDP answer                               |
| `setLocalDescription(desc)`                                 | Sets the local SDP                                  |
| `setRemoteDescription(desc)`                                | Sets the remote SDP                                 |
| `addIceCandidate(candidate)`                                | Adds an ICE candidate received from the remote peer |
| `addTrack(track, stream)`                                   | Adds a media track to the connection                |
| `removeTrack(sender)`                                       | Removes a track                                     |
| `getSenders()`                                              | Returns an array of `RTCRtpSender`                |
| `getReceivers()`                                            | Returns an array of `RTCRtpReceiver`              |
| `getTransceivers()`                                         | Returns an array of `RTCRtpTransceiver`           |
| `addTransceiver(trackOrKind, options)`                      | Adds a transceiver (for audio/video/media control)  |
| `close()`                                                   | Closes the connection                               |
| `createDataChannel(label, options?) ` (WILL SEE THIS LATER) | Creates a new data channel                          |

> -- `removeTrack(sender)`
>
> 📌 Purpose:  Removes a track (audio/video) that was previously added to the connection using `addTrack()`.
>
> 🧠 Why it's useful:  To stop sending a media stream during a call (e.g., turn off camera).
>
> ✅ Syntax: `peerConnection.removeTrack(sender);`
>
> Example--
>
> ```javascript
> const stream = await navigator.mediaDevices.getUserMedia({ video: true });
> const track = stream.getVideoTracks()[0];
> const sender = peerConnection.addTrack(track, stream);
>
> // Later, to stop sending video:
> peerConnection.removeTrack(sender);
> ```
>
> --  `getSenders()`
>
> 📌 Purpose:  Returns an array of `RTCRtpSender` objects — each representing a  **media track being sent** .
>
> ✅ Syntax: `const senders = peerConnection.getSenders();`
>
> 🔸 Use Cases:  To remove a track → you first call `getSenders()` to find the sender:
>
> ```javascript
> const sender = peerConnection.getSenders().find(s => s.track.kind === "video");
> peerConnection.removeTrack(sender);
> ```
>
> --  `getReceivers()`
>
> 📌 Purpose: Returns an array of `RTCRtpReceiver` objects — each representing a **track received** from the remote peer.
>
> ✅ Syntax: `const receivers = peerConnection.getReceivers();`
>
> 🔸 Use Case: To inspect received tracks
>
> Example-
>
> ```javascript
> peerConnection.getReceivers().forEach(receiver => {
>   console.log("Received track:", receiver.track.kind);
> });
> ```
>
> -- `sender.replaceTrack(newTrack)`
>
> This is a method that **replaces the existing media track (audio or video)** being sent to the remote peer **without renegotiating** the entire connection.
>
>> It's part of the [`RTCRtpSender`](https://developer.mozilla.org/en-US/docs/Web/API/RTCRtpSender) object, which you get from `peerConnection.getSenders()` or the return value of `addTrack()`.
>>
>
> ✅ Why Use It?
>
> Imagine you're on a video call, and you:
>
> * Switch from front to back camera 📷🔁
> * Want to mute/unmute your audio 🎤
> * Start screen sharing 🖥️
>
> Instead of:
>
> * Calling `removeTrack()` + `addTrack()` (which  **requires renegotiation** ),
> * You can just do `sender.replaceTrack(newTrack)` — ✅ seamless, efficient, no renegotiation needed!
>
> ✅ Syntax: `sender.replaceTrack(newTrack);` (If `newTrack` is `null`, it stops sending media (e.g., mute).)
>
> Example-- 🔄 Switching from front to rear camera:
>
> ```javascript
> // Initial: using front camera
> const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
> const videoTrack = stream.getVideoTracks()[0];
> const sender = peerConnection.addTrack(videoTrack, stream);
>
> // Later: switch to rear camera
> const newStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } });
> const newVideoTrack = newStream.getVideoTracks()[0];
>
> // Replace the track being sent
> await sender.replaceTrack(newVideoTrack);
>
> // if await sender.replaceTrack(null);---> This will stop sending the video to the remote peer without renegotiation.
> ```
>
> --  `close()`
>
> 📌 Purpose:  Closes the peer connection — permanently ends all media streams and frees resources.
>
> ✅ Syntax: `peerConnection.close();`
>
> 🔸 What it does:
>
> * Stops all tracks
> * Frees ICE and DTLS transports
> * Prevents further signaling or media exchange
>
> 🔸 Use Case:
>
> * End a call when the user clicks "Leave" or "Hang Up"

##### **✅ Events:**

| Event Name                   | Purpose                                                                  |
| ---------------------------- | ------------------------------------------------------------------------ |
| `icecandidate`             | ICE candidate discovered                                                 |
| `icecandidateerror`        | ICE candidate error occurred                                             |
| `connectionstatechange`    | Overall connection status changes (`connected`,`disconnected`, etc.) |
| `signalingstatechange`     | Changes in signaling state (`stable`,`have-local-offer`, etc.)       |
| `iceconnectionstatechange` | ICE-specific connection changes (`checking`,`failed`, etc.)          |
| `icegatheringstatechange`  | ICE candidate gathering phase updates                                    |
| `negotiationneeded`        | Browser triggers a need for SDP renegotiation                            |
| `track`                    | Remote track received (audio/video)                                      |
| `datachannel`              | Remote peer created a data channel                                       |

### 🎥 **2. `MediaStream` (Same exists as Javasript built-in Object)**

##### ✅ Properties:

| Property                         | Description                  |
| -------------------------------- | ---------------------------- |
| `id`                           | Unique ID of the stream      |
| `active`                       | Whether the stream is active |
| `onaddtrack`,`onremovetrack` | Track event handlers         |

##### ✅ Methods:

| Method                 | Description                     |
| ---------------------- | ------------------------------- |
| `getTracks()`        | Returns all tracks              |
| `getAudioTracks()`   | Returns only audio tracks       |
| `getVideoTracks()`   | Returns only video tracks       |
| `addTrack(track)`    | Adds a track to the stream      |
| `removeTrack(track)` | Removes a track from the stream |
| `clone()`            | Clones the stream               |

> `clone()` --
>
> Creates a **duplicate (shallow copy)** of the `MediaStream` object.
>
> Syntax--  `const clonedStream = originalStream.clone();`
>
> ✅ What It Does
>
> * It creates a new `MediaStream` object.
> * All tracks (both audio and video) in the original stream are cloned.
> * Each **MediaStreamTrack** inside the stream is cloned individually using `.clone()`.
> * The **cloned tracks are independent** of the original tracks.
>
> ✅ Why Use `.clone()`?
>
> Here are common use cases:
>
> | Use Case                          | Why Clone?                                                                   |
> | --------------------------------- | ---------------------------------------------------------------------------- |
> | **Record & Preview**        | Record one stream, preview a clone                                           |
> | **Send to Multiple Peers**  | Send the same media independently to multiple WebRTC peers                   |
> | **Apply Different Effects** | Apply filters or transformations on the clone without affecting the original |
> | **Avoid Side Effects**      | Safely stop, mute, or modify one stream without touching the original        |
>
> ✅ Example
>
> ```javascript
> navigator.mediaDevices.getUserMedia({ video: true, audio: true })
>   .then(originalStream => {
>     const clonedStream = originalStream.clone();
>
>     // Play original stream in one video element
>     document.getElementById("video1").srcObject = originalStream;
>
>     // Play cloned stream in another video element
>     document.getElementById("video2").srcObject = clonedStream;
>
>     // Stop tracks in the cloned stream after 5 seconds
>     setTimeout(() => {
>       clonedStream.getTracks().forEach(track => track.stop());
>       console.log("Cloned stream stopped.");
>     }, 5000);
>   });
>
> ```
>
> 🧠 Notes
>
> * Cloning does **not** duplicate media data, it clones the track logic.
> * Cloned tracks are **new track instances** with the same media source.
> * You can independently call `.stop()`, `.mute`, or apply transformations to cloned tracks.

### 🎙️ **3. `MediaStreamTrack`**

##### ✅ Properties:

| Property                | Description                            |
| ----------------------- | -------------------------------------- |
| `id`                  | Unique track ID                        |
| `kind`                | `"audio"`or `"video"`              |
| `label`               | Label of the track (e.g., device name) |
| `enabled`             | Whether the track is enabled           |
| `muted`               | Whether the track is muted             |
| `readyState`          | `live`or `ended`                   |
| `onmute`,`onunmute` | Track event handlers                   |

##### ✅ Methods:

| Method      | Description      |
| ----------- | ---------------- |
| `stop()`  | Stops the track  |
| `clone()` | Clones the track |

### `🔄 4. RTCDataChannel`

##### ✅ Properties:

| Property              | Description                                    |
| --------------------- | ---------------------------------------------- |
| `id`                | The ID of the data channel                     |
| `label`             | Label for the data channel                     |
| `ordered`           | Whether messages are ordered                   |
| `maxPacketLifeTime` | Max time in ms before dropping message         |
| `maxRetransmits`    | Max number of retransmissions                  |
| `protocol`          | Subprotocol                                    |
| `readyState`        | `connecting`,`open`,`closing`,`closed` |
| `bufferedAmount`    | Amount of data queued                          |
| `negotiated`        | Whether it was manually negotiated or not      |
| `binaryType`        | `'blob'`or `'arraybuffer'`                 |

##### ✅ Methods:

| Method         | Description                 |
| -------------- | --------------------------- |
| `send(data)` | Sends data over the channel |
| `close()`    | Closes the channel          |

##### **✅ Events:**

| Event                                      | Triggered when...                                                      |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| `open`                                   | The data channel is ready to send/receive messages                     |
| `close`                                  | The data channel is closed                                             |
| `message`                                | A message is received from the remote peer                             |
| `error`                                  | An error occurs on the data channel (rare)                             |
| *(in older specs)* `bufferedamountlow` | Triggered when `bufferedAmount`falls below a threshold (rarely used) |

### ❄️ **5. `RTCIceCandidate`**

##### ✅ Properties:

| Property             | Description             |
| -------------------- | ----------------------- |
| `candidate`        | Candidate SDP string    |
| `sdpMid`           | Media stream ID         |
| `sdpMLineIndex`    | Media line index in SDP |
| `usernameFragment` | ICE username fragment   |

### 📞 **6. `RTCSessionDescription`**

##### ✅ Properties:

| Property | Description                             |
| -------- | --------------------------------------- |
| `type` | `offer`,`answer`,`rollback`, etc. |
| `sdp`  | Session description string              |

# RTCPeerConnection  Example Program

**WebRTC Peer-to-Peer Connection with Debug Logs for Each Property**

```javascript
<!DOCTYPE html>
<html>
<head>
  <title>WebRTC Property Demo</title>
</head>
<body>
  <h2>Check Console for Logs</h2>
  <button onclick="startConnection()">Start Connection</button>

  <script>
    let peer1, peer2;
    let localStream;

    async function startConnection() {
      // 1. Get user media
      localStream = await navigator.mediaDevices.getUserMedia({ video: false, audio: true });

      // 2. Create two RTCPeerConnection instances
      peer1 = createPeer('Peer1');
      peer2 = createPeer('Peer2');

      // 3. Add tracks from local stream to peer1
      localStream.getTracks().forEach(track => peer1.addTrack(track, localStream));

      // 4. Handle ICE candidates
      peer1.onicecandidate = e => {
        if (e.candidate) peer2.addIceCandidate(e.candidate);
      };
      peer2.onicecandidate = e => {
        if (e.candidate) peer1.addIceCandidate(e.candidate);
      };

      // 5. Track negotiation state changes
      logPeerConnectionStates(peer1, 'Peer1');
      logPeerConnectionStates(peer2, 'Peer2');

      // 6. Track receiving
      peer2.ontrack = (event) => {
        console.log("[Peer2] Received track:", event.streams[0]);
      };

      // 7. Offer/Answer exchange
      const offer = await peer1.createOffer();
      await peer1.setLocalDescription(offer);
      await peer2.setRemoteDescription(peer1.localDescription);

      const answer = await peer2.createAnswer();
      await peer2.setLocalDescription(answer);
      await peer1.setRemoteDescription(peer2.localDescription);
    }

    function createPeer(label) {
      const pc = new RTCPeerConnection({
        iceServers: [{
          urls: ['stun:stun.l.google.com:19302']
        }]
      });

      console.log(`[${label}] Created peer:`, pc);
      return pc;
    }

    function logPeerConnectionStates(peer, label) {
      // Observe changes in connection state
      peer.addEventListener("connectionstatechange", () => {
        console.log(`[${label}] connectionState:`, peer.connectionState);
      });

      peer.addEventListener("iceconnectionstatechange", () => {
        console.log(`[${label}] iceConnectionState:`, peer.iceConnectionState);
      });

      peer.addEventListener("icegatheringstatechange", () => {
        console.log(`[${label}] iceGatheringState:`, peer.iceGatheringState);
      });

      peer.addEventListener("signalingstatechange", () => {
        console.log(`[${label}] signalingState:`, peer.signalingState);
      });

      setInterval(() => {
        console.log(`[${label}] localDescription:`, peer.localDescription?.type);
        console.log(`[${label}] remoteDescription:`, peer.remoteDescription?.type);
        console.log(`[${label}] pendingLocalDescription:`, peer.pendingLocalDescription);
        console.log(`[${label}] pendingRemoteDescription:`, peer.pendingRemoteDescription);
        console.log(`[${label}] currentLocalDescription:`, peer.currentLocalDescription);
        console.log(`[${label}] currentRemoteDescription:`, peer.currentRemoteDescription);
        console.log(`[${label}] canTrickleIceCandidates:`, peer.canTrickleIceCandidates);
        console.log(`[${label}] SCTP available?`, !!peer.sctp);
      }, 3000); // Poll every 3 seconds
    }
  </script>
</body>
</html>

```

### ✨ Some Explanations

⭐ `onicecandidate` events are triggered **after** you call --

For Peer1:

```javascript
const offer = await peer1.createOffer();               // Step 1
await peer1.setLocalDescription(offer);               // Step 2 → triggers ICE gathering
```

**For Peer2:**

```javascript
await peer2.setRemoteDescription(peer1.localDescription);  // Step 3
const answer = await peer2.createAnswer();                // Step 4
await peer2.setLocalDescription(answer);                  // Step 5 → triggers ICE gathering
await peer1.setRemoteDescription(peer2.localDescription); // Step 6
```

When exactly is `onicecandidate` triggered?  -- When you call:  `await peer1.setLocalDescription(offer);`

* WebRTC starts **gathering ICE candidates** asynchronously in the background.
* As candidates are found, these callback fires:

```javascript
peer1.onicecandidate = e => { if (e.candidate) peer2.addIceCandidate(e.candidate); };
peer2.onicecandidate = e => { if (e.candidate) peer1.addIceCandidate(e.candidate); };
```

* ICE candidates are automatically exchanged between the peers.
* This is crucial for NAT traversal to find a working route for media.
* 👉WebRTC stops calling `onicecandidate` when:     `event.candidate === null`

 ⭐ **Logging Peer States --**

```javascript
logPeerConnectionStates(peer1, 'Peer1');
logPeerConnectionStates(peer2, 'Peer2');
```

The function `logPeerConnectionStates()` attaches listeners to important connection state changes:

**⭐ Inside `logPeerConnectionStates` --**

* `connectionstatechange` → logs `peer.connectionState` (`new`, `connecting`, `connected`, `disconnected`, etc.)
* `iceconnectionstatechange` → logs `peer.iceConnectionState` (`new`, `checking`, `connected`, etc.)
* `icegatheringstatechange` → logs how ICE candidates are being gathered
* `signalingstatechange` → logs offer/answer negotiation progress (`stable`, `have-local-offer`, etc.)
* Polling every 3 seconds shows:
  * `localDescription` / `remoteDescription`
  * `pendingLocalDescription` / `currentLocalDescription`
  * `canTrickleIceCandidates` – whether the peer supports trickle ICE
  * `sctp` – whether data channel is available (useful for P2P chat/games)

**⭐ Receiving Media on Peer2 --**

```javascript
peer2.ontrack = (event) => {
  console.log("[Peer2] Received track:", event.streams[0]);
};
```

> When `peer1` sends its audio track, `peer2` receives it via `ontrack`.

### ✅ Final Full Lifecycle Summary

* `navigator.mediaDevices.getUserMedia(...)`

  → 🎤 Gets local audio/video stream from user
* `new RTCPeerConnection(...)` on both sides

  → 🔧 Initializes peer1 and peer2 connection objects
* `peer1.addTrack(...)`

  → 📡 Adds local media tracks to peer1 for sending
* `peer1.createOffer()`

  → 📝 Generates SDP offer describing media capabilities
* `peer1.setLocalDescription(offer)`

  → 🚀 Starts ICE gathering and sets peer1's offer
* `peer1.onicecandidate → peer2.addIceCandidate(...)`

  → 🌐 Sends peer1's ICE candidates to peer2 as they are discovered
* `peer2.setRemoteDescription(offer)`

  → 📥 Accepts peer1's offer and prepares to respond
* `peer2.createAnswer()`

  → 📝 Creates SDP answer compatible with peer1's offer
* `peer2.setLocalDescription(answer)`

  → 🚀 Starts ICE gathering and sets peer2's answer
* `peer2.onicecandidate → peer1.addIceCandidate(...)`

  → 🌐 Sends peer2's ICE candidates to peer1
* `peer1.setRemoteDescription(answer)`

  → 📥 Accepts peer2's answer and completes handshake
* **ICE completes → connection established**

  → ✅ Peers are connected, media/data can now flow
* `peer2.ontrack()` receives media

  → 🎧 peer2 receives audio/video tracks from peer1
* *(optional)* `peer1.createDataChannel(...)` / `peer2.ondatachannel(...)`

  → 💬 Setup data channel for chat or file transfer (optional)

# RTCDataChannel

The **`RTCDataChannel`** is a WebRTC interface that lets you send arbitrary data **directly between two peers** — **peer-to-peer (P2P)** — without needing a server (after the connection is established).

It works like a WebSocket, but runs over  **WebRTC** . This means:

* It's **fast** and  **low-latency** .
* It supports **reliable and unreliable** delivery (like TCP vs UDP).
* It works  **only after the RTCPeerConnection is established** .

### 🔧 How Do You Create It?

You use the method:

```javascript
const dataChannel = peerConnection.createDataChannel("chat", options);  // Here "chat" is the label
```

Where `options` can include:

* `ordered`: Whether messages should be received in order (default is `true`).
* `maxRetransmits`: Max number of times to retransmit a message (for unreliable mode).
* `maxPacketLifeTime`: Max time in milliseconds to attempt sending a message (alternative to retransmits).

📌 Why use a label? (In the example above "chat" is the label)

* For **identification** of the channel, especially if you have multiple channels for different types of data.
* You might use:
  * `"chat"` for text messages
  * `"file"` for binary files
  * `"telemetry"` for sensor data

### 📥 How Do You Receive It?

The receiving peer uses:

```javascript
peerConnection.ondatachannel = (event) => {
  const receiveChannel = event.channel;

  receiveChannel.onmessage = (e) => {
    console.log("Received:", e.data);
  };

  receiveChannel.onopen = () => console.log("Data channel open");
  receiveChannel.onclose = () => console.log("Data channel closed");
};

```

### 🔁 Basic Example

**Sender (Client 1)**

```javascript
const peer = new RTCPeerConnection();
const dataChannel = peer.createDataChannel("chat"); // The "chat" is just a label you assign to the channel

dataChannel.onopen = () => {
  dataChannel.send("Hello from peer 1!");
};

dataChannel.onmessage = (e) => {
  console.log("Peer 1 received:", e.data);
};
```

**Receiver (Client 2)**

```javascript
const peer = new RTCPeerConnection();

peer.ondatachannel = (event) => {
  const receiveChannel = event.channel;

  receiveChannel.onmessage = (e) => {
    console.log("Peer 2 received:", e.data);
    receiveChannel.send("Hi Peer 1!");
  };
};
```

### ✅ Properties:

| Property              | Description                                    |
| --------------------- | ---------------------------------------------- |
| `id`                | The ID of the data channel                     |
| `label`             | Label for the data channel                     |
| `ordered`           | Whether messages are ordered                   |
| `maxPacketLifeTime` | Max time in ms before dropping message         |
| `maxRetransmits`    | Max number of retransmissions                  |
| `protocol`          | Subprotocol                                    |
| `readyState`        | `connecting`,`open`,`closing`,`closed` |
| `bufferedAmount`    | Amount of data queued                          |
| `negotiated`        | Whether it was manually negotiated or not      |
| `binaryType`        | `'blob'`or `'arraybuffer'`                 |

✅ `readyState`

Represents the current state of the data channel.

Possible values:

* **`"connecting"`** : The connection is being established.
* **`"open"`** : The connection is open and ready to send/receive messages.
* **`"closing"`** : The connection is in the process of closing.
* **`"closed"`** : The connection has been closed or could not be opened.

> 🔍 Most commonly checked before using `dataChannel.send()` to ensure it's `"open"`.

✅ `protocol`

A string representing the **sub-protocol** used with the data channel, if specified during creation.

* It is set using the `protocol` option when calling `createDataChannel(label, options)`.
* If not specified, it defaults to an empty string `""`.

Example:    `peer.createDataChannel("chat", { protocol: "json" });`

✅ `id`

A unique identifier (integer) for the data channel on the local peer.

* If `negotiated: false`, it's assigned automatically by the browser.
* If `negotiated: true`, you must manually assign a specific `id`.

> IDs must be between 0 and 65534. This ID must be unique per connection.

✅ `negotiated`

Indicates whether the channel was manually negotiated (`true`) or automatically during offer/answer exchange (`false`).

* **`false`** (default): Data channel is negotiated automatically via signaling.
* **`true`** : You must manually configure both peers with matching `id`.

Example of manual negotiation:

```javascript
peer1.createDataChannel("manual", { negotiated: true, id: 0 });
peer2.createDataChannel("manual", { negotiated: true, id: 0 });
```

✅ `binaryType`

Defines how binary data is handled by the data channel.

Possible values:

* **`"blob"`** : Binary data received as a `Blob` object.
* **`"arraybuffer"`** : Binary data received as an `ArrayBuffer`.

Example: `dataChannel.binaryType = "arraybuffer";`

> ⚠️ You can only set this after the data channel is created.

### ✅ Methods:

| Method         | Description                 |
| -------------- | --------------------------- |
| `send(data)` | Sends data over the channel |
| `close()`    | Closes the channel          |

### 🎯 All **RTCDataChannel Events**

| Event                                      | Triggered when...                                                      |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| `open`                                   | The data channel is ready to send/receive messages                     |
| `close`                                  | The data channel is closed                                             |
| `message`                                | A message is received from the remote peer                             |
| `error`                                  | An error occurs on the data channel (rare)                             |
| *(in older specs)* `bufferedamountlow` | Triggered when `bufferedAmount`falls below a threshold (rarely used) |

Example--

```javascript
const dataChannel = peer.createDataChannel("chat");

dataChannel.onopen = () => {
  console.log("Data channel is open");
};

dataChannel.onmessage = (event) => {
  console.log("Received message:", event.data);
};

dataChannel.onclose = () => {
  console.log("Data channel is closed");
};

dataChannel.onerror = (error) => {
  console.error("Data channel error:", error);
};

```

### 💻 Example Program-

**Shows two peers exchanging messages using a custom-configured data channel.**

```javascript
<!DOCTYPE html>
<html>
<head>
  <title>RTCDataChannel Example</title>
</head>
<body>
  <h2>RTCDataChannel: Ordered & Unreliable Example</h2>
  <button id="send">Send Message</button>
  <pre id="log"></pre>

  <script>
    const log = (msg) => {
      document.getElementById('log').textContent += msg + '\n';
    };

    const peer1 = new RTCPeerConnection(); 
	// Not passed any IceServers-- This works because both peers are in the same context (same browser tab), so no STUN 	server is needed to discover public IPs.But in real-world usage (especially across devices or networks), you need to help 	peers discover how to reach each other using ICE.

    const peer2 = new RTCPeerConnection(); // Comment - Same as above 

    // Create a data channel on peer1
    const dataChannel = peer1.createDataChannel("unreliable-chat", {
      ordered: false,              // allow out-of-order delivery
      maxPacketLifeTime: 3000      // drop messages older than 3 seconds
    });

    // Set binaryType (can be "blob" or "arraybuffer")
    dataChannel.binaryType = "arraybuffer";

    // Display data channel properties
    log(`Channel label: ${dataChannel.label}`);
    log(`Ordered: ${dataChannel.ordered}`);
    log(`Max Packet LifeTime: ${dataChannel.maxPacketLifeTime}`);
    log(`Ready State: ${dataChannel.readyState}`);

    // Set up event handlers
    dataChannel.onopen = () => log("DataChannel is open");
    dataChannel.onclose = () => log("DataChannel is closed");
    dataChannel.onerror = (e) => log("DataChannel error: " + e.message);

    // When peer2 receives a data channel
    peer2.ondatachannel = (event) => {
      const receiveChannel = event.channel;
      log(`Peer2 received channel: ${receiveChannel.label}`);
  
      receiveChannel.onmessage = (e) => {
        log("Peer2 received message: " + e.data);
      };
    };

    // ICE Candidate Exchange
    peer1.onicecandidate = (e) => {
      if (e.candidate) peer2.addIceCandidate(e.candidate);
    };
    peer2.onicecandidate = (e) => {
      if (e.candidate) peer1.addIceCandidate(e.candidate);
    };

    // SDP Offer/Answer
    peer1.createOffer()
      .then(offer => peer1.setLocalDescription(offer))
      .then(() => peer2.setRemoteDescription(peer1.localDescription))
      .then(() => peer2.createAnswer())
      .then(answer => peer2.setLocalDescription(answer))
      .then(() => peer1.setRemoteDescription(peer2.localDescription));

    // Send message when the channel is open
    document.getElementById('send').onclick = () => {
      if (dataChannel.readyState === "open") {
        dataChannel.send("Hello from Peer1! Time: " + new Date().toLocaleTimeString());
        log("Peer1 sent message.");
      } else {
        log("Channel not open yet.");
      }
    };
  </script>
</body>
</html>

```
