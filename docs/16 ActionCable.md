### 1. **What is ActionCable?**
ActionCable is Rails’ built-in library for creating **real-time features** using WebSockets.

It allows:
- live chat  
- notifications  
- real-time counters  
- live updates (without reload)  

---

### 2. **How does ActionCable work?**
It creates a persistent **WebSocket connection** between client and server.

Flow:
1. Client subscribes to a channel  
2. Server streams data  
3. Client receives updates instantly  

---

### 3. **What is a Channel?**
A Ruby class that handles WebSocket logic.

Example:

```ruby
class ChatChannel < ApplicationCable::Channel
  def subscribed
    stream_from "chat_#{params[:room]}"
  end
end
```

Client subscribes:

```js
consumer.subscriptions.create(
  { channel: "ChatChannel", room: 1 },
  { received(data) { console.log(data) } }
)
```

---

### 4. **What is Broadcasting?**
Sending real-time updates to all clients subscribed to a stream.

```ruby
ActionCable.server.broadcast("chat_1", { message: "Hello!" })
```

The client receives the message immediately.

---

### 5. **What is `stream_from`?**
Ties a channel subscription to a broadcast stream.

```ruby
stream_from "notifications_#{current_user.id}"
```

Whenever something is broadcasted to `"notifications_1"`, user 1 sees it live.

---

### 6. **What is the difference between WebSockets and Polling?**

| Method | Description |
|--------|--------------|
| **Polling** | Browser sends repeated requests (“Anything new?”) |
| **WebSockets** | A persistent connection; server pushes updates instantly |

WebSockets are:
- faster  
- cheaper  
- real-time  

---

### 7. **How do you send data from client to server?**

```js
channel.perform("speak", { text: "Hello" })
```

Channel:

```ruby
def speak(data)
  Message.create!(content: data["text"])
end
```

---

### 8. **How do you authenticate ActionCable connections?**
In `connection.rb`:

```ruby
identified_by :current_user

def connect
  self.current_user = find_verified_user
end
```

---

### 9. **Where does ActionCable run?**
Two modes:
- **inline** (same process as Rails server)  
- **standalone** (recommended for production using Redis)  

---

### 10. **Common use cases for ActionCable**
- chat apps  
- notifications  
- live counters (likes, views, online users)  
- collaborative editing  
- dashboards with live updates  

---

