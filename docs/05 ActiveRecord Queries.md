### 1. **What is the difference between `find` and `find_by`?** (Both)
- `find(id)` → raises error if not found  
- `find_by(attr: value)` → returns **nil** if not found

```ruby
User.find(1)
User.find_by(email: "test@example.com")
```

---

### 2. **What does `.where` return?** (Both)
A **Relation object**, which is chainable and lazily evaluated.

```ruby
User.where(active: true)
```

---

### 3. **Difference between `.select` and `.pluck`?** (Both)
- `.select` → returns ActiveRecord objects  
- `.pluck` → returns raw Ruby array of values  

```ruby
User.select(:id, :name)     # returns objects
User.pluck(:id, :name)      # returns arrays of values => [[1, "Adam"], [2, "Sara"]]
```

---

### 4. **What is the difference between `.joins` and `.includes`?** (ActiveRecord)

- **`.joins`**
  - Uses **INNER JOIN**
  - **Filters records**
  - Does **NOT** prevent N+1
  - Excludes records without associated rows

```ruby
User.joins(:posts)
# => users WITH posts only
```

- **`.includes`**
  - Used for **eager loading**
  - Prevents **N+1 queries**
  - Does **NOT filter** by itself
  - Includes records **with and without** associations

```ruby
User.includes(:posts)
# => all users (posts loaded if they exist)
```

**Important:**
`includes` loads data, `joins` filters data.

**Interview one-liner:**
Use `joins` to filter, use `includes` to avoid N+1.

---

### 5. **What is eager loading vs lazy loading?** (Both)
- **Lazy loading** → queries executed ONLY when data needed  
- **Eager loading** → loads associated data upfront (via `includes`)

---

### 6. **Difference between `includes`, `preload`, and `eager_load`?** (ActiveRecord-specific)
| Method         | Behavior                              |
| -------------- | ------------------------------------- |
| **includes**   | auto-chooses JOIN or separate queries |
| **preload**    | always separate queries               |
| **eager_load** | always LEFT OUTER JOIN                |

---

### 7. **How to perform raw SQL?** (ActiveRecord-specific)

```ruby
User.find_by_sql("SELECT * FROM users WHERE id = 1")
User.connection.execute("DELETE FROM users")
```

---

### 8. **What are scopes?** (Both)
Reusable, chainable query blocks.

```ruby
scope :active, -> { where(active: true) }
```

---

### 9. **Why is `default_scope` dangerous?** (Both)
It applies automatically everywhere → can break queries unexpectedly.
```ruby
class User < ApplicationRecord
  default_scope { where(active: true) }
end
```
---

### 10. **How does ActiveRecord lazy loading work?** (Both)
Queries are only executed when you actually **use** the data:

```ruby
users = User.where(active: true)   # no SQL yet
users.first                        # SQL runs here
```

---

### 11. **What does `.distinct` do?** (Both)
Removes duplicate rows:

```ruby
User.select(:role).distinct
```

---

### 12. **Difference between `.count`, `.size`, `.length`?** (Both)

| Method     | When SQL?                               | Notes |
| ---------- | --------------------------------------- | ----- |
| **count**  | always SQL or mongodb count             | fast  |
| **size**   | SQL if unloaded, array length if loaded | smart |
| **length** | always loads records                    | slow  |

---

### 13. **What does `.group` / `.having` do?** (ActiveRecord)

- **`.group`** is used to **aggregate data** (SQL `GROUP BY`)
- **`.having`** filters **after grouping**

```ruby
Order.group(:status).count
```

Groups orders by `status` and returns counts per status.

```ruby
Order.group(:status).having("COUNT(*) > 5").count
```

Keeps only statuses with more than 5 orders.

**Rule:**
- `where` → filters rows
- `having` → filters grouped results

---

### 14. **What does `.order` do?** (ActiveRecord & Mongoid)
Sorts records.

```ruby
User.order(created_at: :desc)
```

Returns users ordered from newest to oldest.

- Returns a query object
- Can be chained with `where`, `limit`, etc.


---

### 15. **How do you prevent SQL injection in ActiveRecord?**

Rails is safe by default when using hash-style queries:

```ruby
User.where(email: params[:email])
User.find_by(email: params[:email])
```

When raw SQL is required (LIKE, functions, complex conditions), use **parameterized queries**:

```ruby
User.where("email = #{params[:email]}") # ⛔ BADD
User.find_by_sql("SELECT * FROM users WHERE email = '#{params[:email]}'") # ⛔BADD
User.where("email = ?", params[:email])
User.where("email ILIKE ?", "%#{params[:q]}%")
```

Never interpolate user input directly into SQL strings.

**Rule:**  
Use hash queries when possible.  
Use `?` placeholders when SQL is unavoidable.

---

### 16. **What is `find_each` and when to use it?** (ActiveRecord-specific)
Loads records in batches (1000 by default) → prevents memory bloat.

```ruby
User.find_each { |u| puts u.name }
```

---

### 17. **What is optimistic locking?** (ActiveRecord-specific)
Detects stale updates using a `lock_version` column.

Rails raises `ActiveRecord::StaleObjectError` if data changed.

---

### 18. **What is pessimistic locking?** (ActiveRecord-specific)
Locks the row until the transaction finishes.

```ruby
User.lock.find(1)
```

---

### 19. **What is counter caching?** (ActiveRecord-specific)
Stores count of associated records in a column.

```ruby
belongs_to :post, counter_cache: true
```

Adds `posts.comments_count` for faster queries.

---

### 20. **What is `touch: true`?** (ActiveRecord-specific)
Updates the parent’s `updated_at` when the child updates.

```ruby
belongs_to :post, touch: true
```

---

### 21. Are indexes the same in Mongoid and ActiveRecord?

**Idea:** Same concept in both. Indexes speed up queries.  
**Composite index:** index on more than one field. **Order matters**.

**ActiveRecord (SQL):**
```ruby
add_index :users, [:first_name, :last_name]
```

**Mongoid (MongoDB):**
```ruby
class User
  include Mongoid::Document

  field :first_name, type: String
  field :last_name, type: String

  index({ first_name: 1, last_name: 1 })
end
```

**Create Mongoid indexes:**
```bash
rails db:mongoid:create_indexes
```

**Rule (both):**
- Works for: first_name
- Works for: first_name + last_name
- Does NOT work for: last_name alone

**Difference:**
- ActiveRecord → indexes in migrations (tables)
- Mongoid → indexes in models (documents)

---

### 22. How do you debug slow ActiveRecord queries? (Both)

```ruby
Model.where(...).explain
```

Shows **how the database executes the query**.
- Tells if an **index is used**
- No index → query is slow

Rule:  
Slow query → run `.explain`

---

### 23. What does `.or` do? (Both)

```ruby
User.where(active: true).or(User.where(admin: true))
```

Returns records that match **either condition**.

Meaning:
- active users ✅
- admin users ✅
- both ✅

Rule:
- `.where` → AND
- `.or` → OR

---

### 24. **How to use `select` to reduce loaded columns?** (Both)

```ruby
User.select(:id, :name).where(active: true)
```

Improves performance.

---

### 25. **How to chain multiple queries?** (Both)
All AR methods return a Relation until executed.

```ruby
User.where(active: true).order(:created_at).limit(10)
```

---

### 26. **What is `left_joins` and when do you use it?** (ActiveRecord-specific)
`left_joins` performs a LEFT OUTER JOIN.

Use it when you want:
- records **with or without** an association
- to filter on joined tables without excluding rows that have no match

Example:
```ruby
Post.left_joins(:comments).where(comments: { id: nil }) # posts with no comments
```

---

### 27. **What is `merge`?** (ActiveRecord-specific)
`merge` combines scopes from different models.

Example:
```ruby
class Comment < ApplicationRecord
  scope :recent, -> { where("created_at > ?", 1.week.ago) }
end

Post.joins(:comments).merge(Comment.recent)
```

---

### 28. **`find_each` vs `in_batches`** (ActiveRecord-specific)
- `find_each`: yields **records** one by one (loads objects)
- `in_batches`: yields **relations/batches** (more flexible, can call `update_all`, `delete_all`)

Example:
```ruby
User.in_batches(of: 1000) { |batch| batch.update_all(active: true) }
```

---

### 29. **`exists?` vs `any?` vs `present?`** (Both)
- `exists?`: fastest check (SQL `SELECT 1 ... LIMIT 1`)
- `any?`: can be SQL or Ruby depending on whether the relation is loaded
- `present?`: loads records (often slower)

Rule: use `exists?` for “does anything match?”.

---

### 30. **What is `pick`?** (ActiveRecord-specific)
`pick` returns a single value (or row) without building full ActiveRecord objects.

Example:
```ruby
User.where(active: true).pick(:email)
```

---

### 31. **What does `unscoped` do?** (Both)
It removes `default_scope` (and any current scopes) for a query.

Example:
```ruby
Post.unscoped.where(deleted_at: nil)
```

---

### 32. **How does ActiveRecord build SQL queries?** (ActiveRecord-specific)
ActiveRecord builds a chainable `Relation` first, then runs SQL only when needed.

Useful methods:
- `to_sql` (see generated SQL)
- `explain` (see query plan)

Example:
```ruby
User.where(active: true).order(:created_at).to_sql
```
`to_sql` lets you **see the SQL that would be run**, without executing it.

---

### 33. **Difference between `.where` and `.find`?** (Both)
- `.find(id)`: fetch by primary key, raises if not found
- `.where(...)`: returns a relation (0..n records), chainable, lazy

---
### 34. `pick` vs `pluck` (ActiveRecord vs Mongoid)

**`pluck`**
```ruby
User.where(active: true).pluck(:email)
```
- Returns **many values**
- Always returns an **array**
- Exists in **ActiveRecord and Mongoid**

Result example:
["a@mail.com", "b@mail.com"]

---

**`pick`** (ActiveRecord only)
```ruby
User.where(active: true).pick(:email)
```
- Returns **one value**
- Returns a **single value**, not an array
- Faster when you need just one value

Result example:
"a@mail.com"

---

**Mongoid equivalent of `pick`**
```ruby
User.where(active: true).limit(1).pluck(:email).first
```

---

**Rule**
