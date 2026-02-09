### 1. **What is Mongoid?** (Mongoid-specific)
Mongoid is the official ODM (Object-Document Mapper) for MongoDB in Ruby.

It replaces ActiveRecord and maps:
- Ruby classes → MongoDB collections  
- Ruby objects → BSON documents  

---

### 2. **How do you define fields in Mongoid?** (Mongoid-specific)

```ruby
class User
  include Mongoid::Document

  field :name, type: String
  field :age, type: Integer
end
```

Unlike ActiveRecord, Mongoid doesn't use migrations.

---

### 3. **What are Mongoid relations?** (Both)
Mongoid supports:

```ruby
belongs_to
has_one
has_many
has_and_belongs_to_many
embeds_one
embeds_many
```

Plus embedded relations unique to MongoDB.

---

### 4. **What is the difference between `embeds_many` and `has_many`?** (Mongoid-specific)

| Relation | Stored Where? | Use Case |
|----------|----------------|----------|
| `embeds_many` | inside parent document | close, nested data |
| `has_many` | in separate collection | large or independent records |

Example:

```ruby
embeds_many :comments
```

This stores all comments **inside** the document — super fast for reads.

---

### 5. **How do you query documents in Mongoid?** (Both)

```ruby
User.where(name: "Adam")
User.where(:age.gt => 20)
User.where(:created_at.gte => 1.week.ago)
```

Mongoid uses MongoDB operators.

---

### 6. **How do you update documents?** (Both)

```ruby
user.update(name: "New Name")
User.where(active: true).update_all(active: false)
```

---

### 7. **What is the difference between `update`, `set`, and `push`?** (Mongoid-specific)

MongoDB-specific operations:

```ruby
user.set(name: "Adam")          # updates only the field
user.push(tags: "ruby")         # pushes to array
```

Efficient because they don't rewrite the full document.

---

### 8. **How do Mongoid validations work?** (Both)
Same as ActiveModel:

```ruby
validates :email, presence: true
validates :age, numericality: true
```

---

### 9. **Does Mongoid support transactions?** (Both)
Yes — but **only when using replica sets**.

```ruby
User.with_session do |session|
  session.start_transaction
  # operations
  session.commit_transaction
end
```

---

### 10. **How do Mongoid indexes work?** (Both)
Defined inside the model:

```ruby
index({ email: 1 }, { unique: true })
```

Create indexes:

```bash
rails db:mongoid:create_indexes
```

---

### 11. **How do you use scopes in Mongoid?** (Both)

```ruby
scope :active, -> { where(active: true) }
```

---

### 12. **How do you embed documents?** (Mongoid-specific)

```ruby
class User
  include Mongoid::Document
  embeds_many :addresses
end

class Address
  include Mongoid::Document
  embedded_in :user
end
```

Embeds are fast but cannot be queried independently unless using `$elemMatch`.

---

### 13. **How does Mongoid handle IDs?** (Mongoid-specific)
Mongoid uses BSON ObjectIds by default:

```ruby
_id: BSON::ObjectId(“...”)
```

These are globally unique and generated client-side.

---

### 14. **What is `only` and `without` used for in Mongoid?** (Mongoid-specific)
Mongoid supports projections to control which fields are returned.

**`only` → include these fields:**

```ruby
User.only(:id, :name)
```

**`without` → exclude these fields:**

```ruby
User.without(:created_at, :updated_at)
```

Benefits:
- reduces payload size  
- improves query performance  
- essential for optimizing API JSON responses  

---

### 15. **What is `pluck` in Mongoid?** (Both)

```ruby
User.pluck(:email)
```

Returns an **array** of values directly from the DB (very fast).

---

### 16. **How do you use atomic updates in Mongoid?** (Mongoid-specific)
Atomic operations modify fields without rewriting the whole document.

Examples:

```ruby
inc(views: 1)         # increment
set(active: true)     # set field
push(tags: "ruby")    # array push
pull(tags: "java")    # array remove
```

---

### 17. **How do you handle relations in Mongoid without N+1?** (Both)
Use `.includes` for referenced associations:

```ruby
User.includes(:posts)
```

Embeds never have N+1 because they're inside the document.

---

### 18. **How do you order queries?** (Both)

```ruby
User.order_by(name: :asc)
User.order_by(age: :desc)
```

---

### 19. **How do you paginate in Mongoid?** (Both)
Typical gems work:
- Kaminari  
- Pagy  

Example (Kaminari):

```ruby
@users = User.page(params[:page]).per(20)
```

---

### 20. **How do you perform aggregations in Mongoid?** (Both)

```ruby
User.collection.aggregate([
  { "$match": { active: true } },
  { "$group": { _id: "$role", count: { "$sum": 1 } } }
])
```

Mongoid uses MongoDB’s aggregation pipeline.

---

### 21. **How do you perform text search?** (Both)
Enable text index:

```ruby
index({ name: "text", description: "text" })
```

Query:

```ruby
User.where(:$text => { :$search => "doctor" })
```

---

### 22. **Does Mongoid support joins?** (Mongoid-specific)
No — MongoDB has no joins.  
Equivalent approaches:
- Embedding  
- Manual referencing  
- Aggregation lookup (`$lookup`)

Example lookup:

```ruby
User.collection.aggregate([
  { "$lookup": {
      from: "orders",
      localField: "_id",
      foreignField: "user_id",
      as: "orders"
  }}
])
```

---

### 23. **When should you NOT embed documents?** (Mongoid-specific)
Avoid embedding when:
- documents grow unbounded (ex: logs)  
- frequent updates needed inside arrays  
- embedded documents exceed MongoDB's 16MB limit  
- documents must be queried independently  

---

### 24. **Pros and Cons of Mongoid vs ActiveRecord?** (Both)

#### **Pros**
- no migrations  
- flexible schema  
- embedded documents  
- fast writes  
- great for unstructured data  

#### **Cons**
- no joins  
- weaker consistency  
- fewer relational guarantees  
- more complex querying  
- less tooling  

---

### 25. **When is MongoDB a good choice?** (Both)
- analytics  
- logs  
- flexible data (variably shaped documents)  
- caching layers  
- events and notifications  
- apps requiring high write throughput  

Not ideal for financial transactions or heavy relational logic.

---

### 26. **What callbacks does Mongoid support?** (Both)
Mongoid includes the same callbacks as ActiveModel:

```ruby
before_validation
after_validation
before_save
after_save
before_create
after_create
before_update
after_update
before_destroy
after_destroy
```

**Important differences:**
- Callbacks on **embedded documents** run when the parent document saves.
- MongoDB operations using atomic updates (`set`, `inc`, etc.) **do NOT trigger callbacks**.
