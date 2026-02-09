### 1. **What is Hotwire?**
Hotwire is a Rails framework for building **modern, fast applications without heavy JavaScript**.

It consists of:
- **Turbo** (Drive, Frames, Streams)
- **Stimulus** (small JS controllers)
- **Strada** (mobile integration, optional)

Hotwire keeps HTML server-driven while updating the page dynamically.

---

### 2. **What is Turbo Drive?**
Turbo Drive intercepts link clicks and form submissions to:
- load new pages via AJAX  
- replace only the `<body>`  
- preserve state  
- avoid full page reloads  

This makes navigating feel faster, like a SPA.

---

### 3. **What are Turbo Frames?**
Allows updating **parts** of the page independently.

Example:

```erb
<turbo-frame id="comments">
  <%= render @comments %>
</turbo-frame>
```

Submitting a form inside the frame updates **only that frame**.

---

### 4. **What are Turbo Streams?**
Real-time page updates using SSE (Server-Sent Events) or WebSockets.

Example partial:

```erb
<turbo-stream action="append" target="messages">
  <template>
    <%= render @message %>
  </template>
</turbo-stream>
```

Rails controllers can broadcast updates:

```ruby
Turbo::StreamsChannel.broadcast_append_to(
  "messages",
  partial: "messages/message",
  locals: { message: @message }
)
```

---

### 5. **What is Stimulus?**
A lightweight JavaScript framework that:
- enhances HTML  
- adds small behaviors  
- replaces heavy frameworks like React/Angular for small interactions  

Example:

```html
<div data-controller="hello">
  <button data-action="click->hello#greet">Say Hi</button>
</div>
```

Controller:

```javascript
export default class extends Controller {
  greet() {
    alert("Hello!")
  }
}
```

---

### 6. **How do Turbo Streams + ActionCable work together?**
Broadcast updates in real-time:

```ruby
broadcast_append_to "comments", target: "comments", partial: "comments/comment"
```

Client receives updates without refreshing.

---

### 7. **Why choose Hotwire instead of a SPA (React/Vue)?**
Because:
- fewer API endpoints  
- faster development  
- no JSON serialization overhead  
- better SEO  
- easier debugging  
- Rails server renders HTML → less frontend code  

Hotwire is often enough for:
- dashboards  
- admin panels  
- messaging systems  
- booking apps  
- ecommerce UIs  

---

### 8. **How do Turbo Streams help with forms?**
Auto-updates parts of the page after form submissions.
