# 🌐 How the Internet Works

## Basic Flow

```text
Your Computer
      ↓
    Router
      ↓
     ISP
      ↓
   Internet
      ↓
    Server
      ↓
Your Computer
```

---

## 🔑 Important Concepts

### DNS — "Where is this website?"

DNS converts a domain name into an IP address.

```text
example.com → 93.184.x.x
```

---

### IP — "Which computer?"

An IP address identifies a device or server on a network.

```text
192.168.1.10
```

Think of it like a **computer's address**.

---

### Port — "Which service?"

Ports identify services running on a computer.

```text
22  → SSH
80  → HTTP
443 → HTTPS
```

Think of a port as a **door to a particular service**.

---

### HTTP — "How do they communicate?"

HTTP is the protocol used for communication between a browser and a web server.

```text
Browser → HTTP Request → Server
Browser ← HTTP Response ← Server
```

Example:

```http
GET /profile HTTP/1.1
```

---

### Cookie — "Information stored in the browser"

A cookie is a small piece of data stored by a website in your browser.

Example:

```text
session_id=abc123
```

The browser can send it back to the server with requests.

---

### Session — "How the website remembers you"

A session allows a website to maintain your login/state.

```text
Login
  ↓
Server creates session
  ↓
Browser receives session ID
  ↓
Browser sends session ID
  ↓
Server recognizes the user
```

---

### Headers — "Extra information"

Headers contain additional information in HTTP requests and responses.

Example:

```http
Host: example.com
User-Agent: Chrome
Cookie: session=abc123
```

---

### Proxy — "Middleman"

A proxy sits between the browser and server.

**Without proxy:**

```text
Browser → Server
```

**With proxy:**

```text
Browser → Proxy → Server
```

Tools such as **Burp Suite** can work as a proxy for authorized web security testing.

---

### Server — "Processes requests"

A server receives requests, processes them, and sends responses.

```text
Browser
   ↓
Server
   ↓
Response
   ↓
Browser
```

---

### Database — "Stores application data"

A database stores and retrieves information used by an application.

```text
Browser
   ↓
Server
   ↓
Database
   ↓
Server
   ↓
Browser
```

Example:

```text
Users
----------------
ID    Name
1     Gobinda
2     Rahul
```

---

# 🧠 Easy Memory

```text
DNS      → Where is this website?
IP       → Which computer?
Port     → Which service?
HTTP     → How do browser and server communicate?
Cookie   → What data is stored in the browser?
Session  → How does the website remember you?
Headers  → What extra information is sent?
Proxy    → Who is the middleman?
Server   → Who processes the request?
Database → Where is the application data stored?
```

## 🔥 Complete Flow

```text
Browser
   ↓
DNS → Find IP
   ↓
IP → Find server
   ↓
Port → Find service
   ↓
HTTP/HTTPS → Send request
   ↓
Headers + Cookies
   ↓
Server
   ↓
Session
   ↓
Database
   ↓
Server
   ↓
HTTP Response
   ↓
Browser
```

> **Understand the flow of data. That's the foundation of web security.**
