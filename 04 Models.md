### 1. What is ActiveRecord?

ActiveRecord is Rails’ ORM (Object-Relational Mapping) layer.

It allows you to interact with the database using Ruby objects instead of writing SQL.

ActiveRecord:
- Maps database tables to Ruby classes
- Maps table rows to Ruby objects
- Provides CRUD operations (Create, Read, Update, Delete)
- Handles validations, associations, and callbacks

Example:
```ruby
class User < ApplicationRecord
end

---

### 2. **What is ORM?**
Object-Relational Mapping converts relational DB rows into Ruby objects so you interact with them using Ruby, not SQL.

---

### 3. **What naming conventions does Rails use for models/tables?**
- Model class → **singular** (`User`)  
- Table name → **plural** (`users`)  

Rails uses this to auto-detect which table belongs to which model.

---

### 4. **What is Single Table Inheritance (STI)?**
A technique where multiple Ruby subclasses share **one database table**.

Example:

```ruby
class User < ApplicationRecord; end
class Admin < User; end
```

DB table must have a `type` column.

---

### 5. **Difference between `save`, `save!`, `create`, and `create!`?**

**Yes — conceptually they behave the SAME in both ActiveRecord and Mongoid.**  
The differences are internal (SQL vs MongoDB), but **behavior, intent, and usage are identical**.
### Common behavior (ActiveRecord ✅ / Mongoid ✅)

| Method     | Runs validations | Persists data | Raises exception on failure |
|------------|------------------|---------------|-----------------------------|
| `save`     | ✅ yes           | ✅ yes        | ❌ no (returns `false`)     |
| `save!`    | ✅ yes           | ✅ yes        | ✅ yes (`Exception`)        |
| `create`   | ✅ yes           | ✅ yes        | ❌ no (returns invalid obj) |
| `create!`  | ✅ yes           | ✅ yes        | ✅ yes (`Exception`)        |
Use `!` versions in service code so failures aren’t silent.

---

### 6. **What is the difference between `update` and `update!`?**
Same as above: `update!` raises an exception if validations fail.

---

### 7. What are callbacks?

Callbacks are methods that run automatically at specific points in a model’s lifecycle  
(when an object is validated, saved, created, updated, or destroyed).

They allow you to execute logic **without manually calling it**.

## Common callbacks

- `before_validation` → runs before validations
- `before_save` → runs before create or update
- `after_create` → runs only after creation
- `after_commit` → runs after DB transaction is committed
- `after_destroy` → runs after deletion

## Example

``` ruby
class User < ApplicationRecord
  before_validation :normalize_email
  after_create :send_welcome_email

  private

  def normalize_email
    self.email = email.downcase.strip
  end

  def send_welcome_email
    WelcomeMailer.send_later(id)
  end
end
```

### 8. Why can callbacks be dangerous?

Callbacks are dangerous because they run **automatically** and can hide important logic.

- They hide business logic (code runs without being obvious)
- They make testing harder
- They can cause unexpected side effects
- They may trigger extra database writes
- They make code harder to understand and maintain

Many teams prefer **service objects** instead, because the logic is explicit and easier to control.

**Summary:** callbacks reduce clarity and can cause bugs if overused.

---
### 9. **What is a transaction?**
A transaction groups multiple database operations into a **single unit of work**.

Rule:  
**All operations succeed, or none are saved.**

```ruby
ActiveRecord::Base.transaction do
  user.save!
  charge.create!
end
```

- If **all operations succeed** → transaction is committed  
- If **any operation fails** (raises an exception) → everything is rolled back  

This prevents partial data (for example: a user saved without a charge).

**Important:**
- Use `save!` / `create!` so failures raise exceptions  
- Transactions rely on exceptions to trigger rollback  

**Used for:**
- payments  
- transfers  
- multi-step updates  
- operations requiring strong data consistency  

---

### 10. **Difference between model validations and DB validations**

**Model validations**
```ruby
validates :email, presence: true
```
- Checked in Rails (Ruby)
- Used for user-friendly errors
- **Can be bypassed**

Bypassed when:
```ruby
user.save(validate: false)
User.update_all(email: nil)
```

**Database validations (constraints)**
```sql
email NOT NULL UNIQUE
```
- Enforced by the database
- **Cannot be bypassed**
- Always guarantees data integrity

**Rule:**  
Use model validations for UX, DB constraints for safety.


---

### 11. **How do errors get stored?**
When a model fails validation, Rails stores the errors in the model’s `errors` object.

Example:
```ruby
user = User.new(email: nil)
user.valid?   # => false
```

Rails fills:
```ruby
user.errors
```

You can read them with:
```ruby
user.errors.full_messages
```

Which returns:
```ruby
["Email can't be blank"]
```

**Important:**
- Errors are added when `valid?`, `save`, or `save!` runs
- Errors live **only in memory** (not saved in DB)
- Errors are cleared on the next validation attempt

---

### 12. **What does `accepts_nested_attributes_for` do?**
It allows you to **create or update a parent record and its associated records at the same time** using one form submission.

Example:
```ruby
class Order < ApplicationRecord
  has_many :items
  accepts_nested_attributes_for :items
end
```

This lets Rails accept parameters like:
```ruby
{
  order: {
    customer_name: "Adam",
    items_attributes: [
      { name: "Book", price: 10 },
      { name: "Pen", price: 2 }
    ]
  }
}
```

Rails will:
- save the `Order`
- save the associated `Items`

**Used for:**
- forms with nested fields
- parent + children creation/update
- avoiding multiple form submissions

**Important:**
- Works with `has_many`, `has_one`
- Requires strong params (`items_attributes`)

---

### 13. **What is the `attribute` API?**
Used in **ActiveRecord** to define typed attributes and defaults.

```ruby
attribute :active, :boolean, default: false
```

- Can be backed by a database column  
- Can also be **virtual** (not stored in DB)  
- Mainly used for type casting and defaults  
#### Is this the same as Mongoid `field`?**
**Similar purpose, not the same thing.**

Mongoid equivalent:
```ruby
field :active, type: Boolean, default: false
```

**Key difference:**
- `attribute` (ActiveRecord) → can be virtual or persisted  
- `field` (Mongoid) → always persisted in MongoDB  

**Interview one-liner:**
ActiveRecord `attribute` can define virtual attributes, while Mongoid `field` always defines a stored document field.

---

### 14. **What is `enum` in models?**
`enum` maps **readable names** to stored values and adds helper methods.
## **ActiveRecord**
```ruby
class User < ApplicationRecord
  enum status: { pending: 0, active: 1, archived: 2 }
end
```

- DB stores integers (`0`, `1`, `2`)
- Ruby uses names (`pending`, `active`, `archived`)

```ruby
user.active!
user.active?
User.pending
```
## **Mongoid**
```ruby
class User
  include Mongoid::Document
  field :status, type: Integer, default: 0

  enum status: { pending: 0, active: 1, archived: 2 }
end
```

- Stored as integer in MongoDB
- Same Ruby helpers as ActiveRecord

---

**Why use enum?**
- Cleaner code
- Faster than strings
- Prevents magic numbers

---

### 15. **What is the purpose of validations?**
To ensure data correctness before saving to the database.
They are used to:

- prevent bad or incomplete data
- show helpful error messages to users
- stop invalid records from being saved
---

### 16. **What is dirty tracking?**
Dirty tracking is a built-in ActiveRecord feature that **automatically tracks changes to model attributes**.

Rails generates helper methods for every attribute — you do not write them yourself.

```ruby
user = User.find(1)
user.name            # "Old"

user.name = "New"    # change in memory (not saved yet)

user.name_changed?   # true
user.name_was        # "Old"
user.changes         # { "name" => ["Old", "New"] }

user.save

user.previous_changes
# { "name" => ["Old", "New"], "updated_at" => [...] }
```

**Why it’s useful:**
- detect what changed
- access old vs new values
- run logic only when a field changes

**Interview one-liner:**  
Dirty tracking lets Rails track attribute changes before and after saving and exposes helper methods like `*_changed?`, `*_was`, and `changes`.

---

### 17. **Difference between `update`, `update_attribute`, and `update_column`?** (ActiveRecord)

| Method | Validations | Callbacks | Touches timestamps |
|--------|-------------|-----------|--------------------|
| `update` | ✅ yes | ✅ yes | ✅ yes |
| `update_attribute` | ❌ no | ✅ yes | ✅ yes |
| `update_column` | ❌ no | ❌ no | ❌ no |

**Rule:** use `update` by default. Use bypass methods only when you really mean it.

---

### 18. **What are virtual attributes?** 
Attributes that exist in Ruby but are **not stored** in the database.

Example:
```ruby
class User < ApplicationRecord
  attr_accessor :admin_note
end
```

---

### 19. **What does `delegate` do?**
It forwards methods to another object (often an association), making code cleaner.

Example:
```ruby
class Order < ApplicationRecord
  belongs_to :user
  delegate :email, to: :user, prefix: true
end

order.user_email
```

---

### 20. **What are `serialize` and `store` used for?** (ActiveRecord)
They let you store **structured data** in one column.

- `serialize` (older): store Ruby objects in a text column (often YAML/JSON).
- `store` / `store_accessor`: store key-value data in a JSON/JSONB column.

Use cases:
- settings/preferences
- metadata fields

---

### 21. **What is callback order (high level)?**
Callbacks run in a sequence (simplified):
- validations (`before_validation`, validations, `after_validation`)
- saving (`before_save`, `before_create`/`before_update`, save, `after_save`)
- commit (`after_commit`) when the transaction is committed

Callbacks also run in the order they are defined in the model.

---

### 22. **`after_save` vs `after_commit`**
- `after_save`: runs after the record is saved (but the DB transaction might still roll back later).
- `after_commit`: runs only after the DB transaction is successfully committed.

**Rule:** use `after_commit` for side effects that must not happen if rollback occurs (emails, external APIs).

---

### 23. **What are readonly records?** {ActiveRecord}
Readonly records cannot be updated or saved.

Example:
```ruby
user = User.first.readonly!
user.save # raises ActiveRecord::ReadOnlyRecord
```

---

### **Transactions vs Atomicity (SQL vs MongoDB)**

**ActiveRecord (SQL):**
Transactions group multiple operations into one atomic unit.

```ruby
ActiveRecord::Base.transaction do
  user.save!
  payment.save!
end
```

- Works across multiple tables/records  
- If one operation fails → everything is rolled back  

---

**MongoDB (default behavior):**
MongoDB is **atomic at the document level**, not across documents.

```ruby
user.update!(name: "Adam", age: 30)
```

- A single document update is always atomic  
- Either all fields update or none do  
- This is **NOT** a transaction

---

**MongoDB design approach:**
Instead of transactions, MongoDB apps usually **embed related data**.

```ruby
class User
  include Mongoid::Document
  embeds_many :payments
end
```

```ruby
user.payments.create!(amount: 100)
```

- One document  
- One atomic operation  
- No transaction needed  

---

**MongoDB transactions (SQL-like behavior):**
Available only on **replica sets or sharded clusters**.

```ruby
User.with_session do |session|
  session.start_transaction
  user.save!(session: session)
  payment.save!(session: session)
  session.commit_transaction
end
```

- Equivalent to SQL transactions  
- Slower and used sparingly  

---

**Key difference:**
- SQL relies on **transactions**
- MongoDB relies on **document atomicity and embedding**

**Interview one-liner:**
MongoDB guarantees atomicity per document, so applications usually embed related data instead of relying on multi-document transactions.
