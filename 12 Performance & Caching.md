### 1. **What is caching in Rails?**
Caching stores computed data so it can be reused without recalculating.

Types:
- Page cache  
- Action cache  
- Fragment cache  

---

### 2. **What is Fragment Caching?**
Caches the **rendered HTML** of part of a view.

```erb
<% cache @user do %>
  <%= render "profile", user: @user %>
<% end %>
```

**When does it re-render?**  
Rails builds a **cache key** from `@user` (includes `updated_at`).  
If `@user` changes → `updated_at` changes → **cache key changes** → cache miss → Rails re-renders and stores new HTML.

**Important:** Rails doesn’t delete the old cache; it just starts using the new key (old one gets evicted later).

---

### 3. **What is Russian Doll Caching?**
Nested fragment caches.

Useful when:
- Parent depends on child  
- Only the changed child invalidates the parent  

Example:

```erb
<% cache @post do %>
  <% cache [@post, @post.comments] do %>
    ...
  <% end %>
<% end %>
```

---

### 4. **What are Cache Keys?**
Unique identifiers for cache entries.

Rails generates keys automatically using:

- model ID  
- updated_at timestamp  

Custom key:

```ruby
cache_key = "users/#{user.id}"
```

---

### 5. **What is a Cache Digest?**
Rails attaches a digest (hash) representing template dependencies.  
If you change template code → digest changes → cache invalidates.

A **cache digest** is a **fingerprint of the view template code**.

Rails uses it to know if the **HTML code itself changed**, even if the data did not.
#### Why it exists

Fragment caching normally updates when **data changes**.

But if you:

- change the template (`.erb` file)
- and the data stays the same

Rails still needs to **re-render the HTML**.

The cache digest solves this.

---

### 6. **What is Counter Cache?**
Stores the count of associated records to avoid expensive `COUNT(*)` queries.

```ruby
belongs_to :post, counter_cache: true
```

This adds `comments_count` to posts table.

---

### 7. **What is Memoization (`||=`)?**
Caching inside Ruby objects during runtime.

Example:

```ruby
def total
  @total ||= expensive_calculation
end
```

Used to avoid repeating heavy operations.

---

### 8. **How do you avoid N+1 queries?**
Using `includes`:

```ruby
User.includes(:posts)
```

Tools:
- Bullet gem  
- Skylight  
- New Relic  

---

### 9. **How do you improve performance in Rails APIs?**
- Use `select` to reduce columns  
- Use pagination  
- Use serializers efficiently  
- Load associations with `includes`  
- Add DB indexes  
- Move slow tasks to background jobs  

---

### 10. **When should background jobs be used for performance?**
Whenever a request is slow:
- sending emails  
- generating PDFs  
- processing images  
- calling external APIs  
- long DB updates  

This keeps requests fast.

---

### 11. **How do you cache queries in Rails?**
Inside a block:

- If the key exists:
    
    - Rails returns the cached data
        
    - ❌ no database query
        
- If the key does NOT exist:
	- Rails runs `Product.all`

```ruby
Rails.cache.fetch("products_all") do
  Product.all.to_a
end
```

---

### 12. **What is HTTP caching?**
Browser caching usage:
- ETag  
- Last-Modified  
- Cache-Control headers  (Cache-Control: public, max-age=600)

Rails example:

```ruby
fresh_when @post
```

---

## Redis vs Memcached — Quick Comparison (Interview Ready)

### Memcached
- Simple **in-memory cache**
- Key → value (strings only)
- **No persistence** (data lost on restart)
- Extremely fast
- No queues, no pub/sub, no locks
- Best for:
  - fragment caching
  - Rails view caching
  - temporary data that can be regenerated

**Rule:** Losing data is OK.

---

### Redis
- **In-memory data store**
- Supports data structures (lists, sets, hashes)
- **Optional persistence** (data can survive restarts)
- Atomic operations
- Supports:
  - queues (Sidekiq)
  - pub/sub (ActionCable)
  - counters, locks, rate limits
- Best for:
  - background jobs
  - realtime features
  - coordination between servers

**Rule:** Correctness matters.

---

### One-line Interview Answer

> Memcached is a fast, memory-only cache for temporary data, while Redis is a richer in-memory data store that supports persistence, queues, and coordination.

---

### Mental Model

- **Cache** → Memcached  
- **Coordination / Jobs / Realtime** → Redis
