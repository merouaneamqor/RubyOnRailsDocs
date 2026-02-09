### 1. **How would you implement soft delete in Rails?**
Two approaches:

#### A) Add `deleted_at` column
```ruby
scope :active, -> { where(deleted_at: nil) }

def soft_delete
  update(deleted_at: Time.current)
end
```

#### B) Use gem: Paranoia
```ruby
acts_as_paranoid
```

---

### 2. **How do you design a tagging system?**
Models:

```ruby
class Tag < ApplicationRecord
  has_many :taggings
end

class Tagging < ApplicationRecord
  belongs_to :tag
  belongs_to :taggable, polymorphic: true
end
```

Allows tagging any model (posts, photos, comments).

---

### 3. **How would you implement role-based authorization?**
Using **Pundit**:

```ruby
class PostPolicy < ApplicationPolicy
  def update?
    user.admin? || record.user_id == user.id
  end
end
```

Controller:

```ruby
authorize @post
```

---

### 4. **How do you implement search efficiently?**
Options:
- basic: `where("name ILIKE ?", "%term%")`  
- advanced: PostgreSQL full-text search  
- very advanced: Elasticsearch / Meilisearch / Algolia  

---

### 5. **How to handle file uploads for many users?**
Use:
- **ActiveStorage**  
- **Direct uploads to S3**  
- **Background jobs for processing**  

Avoid storing large files on the Rails server.

---

### 6. **How would you design a notification system?**
Components:
- Notification model  
- ActionCable or Turbo Streams broadcasting  
- Background jobs for async delivery  

Example model:

```ruby
belongs_to :user
enum kind: [:new_message, :payment_received]
```

---

### 7. **How to avoid race conditions when updating counters?**

Use database atomic operations:

```ruby
Post.increment_counter(:views_count, post_id)
```

Or transactions:

```ruby
Post.transaction do
  post.lock!
  post.update!(count: post.count + 1)
end
```

---

### 8. **How would you implement audit logging?**
Store changes in an `AuditLog` table.

Use PaperTrail gem:

```ruby
has_paper_trail
```

Tracks:
- who made the change  
- what changed  
- when  

---

### 9. **How to design a multi-tenant app?**
Approaches:

#### A) Subdomain-based
`tenant1.example.com`

#### B) Row-based scoping
Every model includes `account_id`.

```ruby
default_scope { where(account_id: Current.account.id) }
```

#### C) Schema per tenant
Using Apartment gem.

---

### 10. **How do you prevent duplicate form submissions?**
Options:
- disable submit button  
- idempotency keys  
- server-side uniqueness validation  

Server example:

```ruby
Order.create_with(idempotency_key: key).find_or_create_by(key: key)
```

---

### 11. **How do you structure a large Rails refactor?**
Steps:
1. Write tests  
2. Extract service objects  
3. Remove callbacks  
4. Split fat models  
5. Introduce interactors or form objects  
6. Extract modules or engines if very large  

---

### 12. **How to safely migrate a large table in production?**
- add columns without default  
- backfill using background jobs  
- update application to use new field  
- remove old column later  

Avoid locking the table.

---

### 13. **How would you design a real-time chat?**
Use:
- ActionCable channels  
- Turbo Streams  
- background jobs for message delivery  

Steps:
1. ChatChannel streams messages  
2. Controller saves messages  
3. Broadcast message  
4. Turbo updates UI instantly  

---

### 14. **How do you handle API rate limiting?**
Use Rack Attack:

```ruby
Rack::Attack.throttle("req/ip", limit: 100, period: 1.minute) do |req|
  req.ip
end
```

---

### 15. **How to paginate a very large dataset?**
Use cursor-based pagination:

```ruby
User.where("id > ?", last_seen_id).limit(50)
```

Or use Pagy gem (fastest).

---

### 16. **How do you bulk insert/update records?**
Using ActiveRecord import:

```ruby
User.insert_all([{name: "A"}, {name: "B"}])
```

Or upsert:

```ruby
User.upsert_all(data)
```

---

### 17. **How to debug a memory leak in Rails?**
Tools:
- memory_profiler  
- rack-mini-profiler  
- NewRelic  
- derailed_benchmarks  

Look for:
- large cached objects  
- infinite loops  
- ActiveRecord loading too many objects  

---

### 18. **How do you test performance bottlenecks?**
Use:

```bash
rails benchmark
```

Or:

```ruby
Benchmark.measure { User.includes(:posts).to_a }
```

Or profiling tools.

---

### 19. **How would you design an email verification system?**
Steps:
1. Generate token  
2. Store token (hashed)  
3. Send email  
4. Verify link  
5. Activate user  

Example:

```ruby
user.update(confirmation_token: SecureRandom.hex)
```

---

### 20. **How would you handle large JSON responses?**
- pagination  
- compression (Rack::Deflater)  
- selective fields (`select`, `only`)  
- streaming JSON in chunks  

---

