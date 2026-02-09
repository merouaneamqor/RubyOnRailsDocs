### 1. **What types of associations exist in Rails?** (ActiveRecord-specific)
- `belongs_to`
- `has_one`
- `has_many`
- `has_many :through`
- `has_and_belongs_to_many` (HABTM)
- `has_one :through`
- polymorphic associations
#### ActiveRecord vs Mongoid Associations (Side-by-Side)

|Association Type|ActiveRecord|Mongoid|
|---|---|---|
|`belongs_to`|✅|✅ (`belongs_to` — reference only)|
|`has_one`|✅|✅ (`has_one`)|
|`has_many`|✅|✅ (`has_many`)|
|`has_many :through`|✅|❌ _(not supported)_|
|`has_one :through`|✅|❌ _(not supported)_|
|`has_and_belongs_to_many` (HABTM)|✅|✅ (`has_and_belongs_to_many` — ID arrays)|
|Polymorphic associations|✅|✅ (`polymorphic: true`)|
|Embedding (one)|❌|✅ (`embeds_one`)|
|Embedding (many)|❌|✅ (`embeds_many`)|

---

### 2. **What does `belongs_to` mean?** (Both)
The model contains the foreign key.

```ruby
class Comment < ApplicationRecord
  belongs_to :user
end
```

---

### 3. **What does `has_many` mean?** (Both)
Defines a one-to-many relationship.

```ruby
class User < ApplicationRecord
  has_many :comments
end
```

---

### 4. **What is `has_many :through`?** (ActiveRecord-specific)
Used when a relationship goes **through another model** (join model).

```ruby
class Doctor < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments
end
```

Better than HABTM because you can add attributes to the join table.

---

### 5. **What is HABTM (`has_and_belongs_to_many`)?** (Both)
A direct many-to-many relationship **without a join model**.  
Not recommended for complex apps.

```ruby
has_and_belongs_to_many :tags
```

---

### 6. **What is a polymorphic association?** (Both)
One model can belong to **multiple other models**.

```ruby
class Comment < ApplicationRecord
  belongs_to :commentable, polymorphic: true
end
```

Used for shared features (comments, likes, attachments).

---

### 7. **What does `dependent:` do?** (Both)
Controls what happens to child records when parent is destroyed.
#### Options

- `:destroy` → delete children + run callbacks (safe, slow)
- `:delete_all` → delete children, no callbacks (fast)
- `:nullify` → keep children, set FK to NULL
- `:restrict_with_error` → block delete, add error
- `:restrict_with_exception` → block delete, raise error
#### Difference (main ones)

- `:destroy` = callbacks
- `:delete_all` = speed
- `:nullify` = keep data

---

### 8. **What is `inverse_of` and why is it useful?** (Both)
Tells Rails both sides of the relationship, enabling:
- fewer queries  
- proper caching  
- correct building of nested models  

```ruby
class User < ApplicationRecord
  has_many :posts, inverse_of: :user
end
```

---

### 9. **What is eager loading?** (Both)
Preloads associated records in fewer queries.

```ruby
User.includes(:posts)
```

Prevents N+1 queries.

---

### 10. **What is lazy loading?** (Both)
Rails only loads the association **when you access it**.

```ruby
user.posts # triggers query
```

---

### 11. **What is the N+1 problem?** (Both)
When Rails loads each associated record separately:

```ruby
User.all.each { |u| puts u.posts.count }
```

→ 1 query for users + 1 query per user. Very slow.

---

### 12. **How do you fix N+1?** (Both)

```ruby
User.includes(:posts)
```

→ Loads all posts in 1 extra query.

---

### 13. **Can you validate associated records?** (Both)

```ruby
validates_associated :posts
```

But discouraged — use form objects instead.

---

### 14. **What is `has_one :through`?** (ActiveRecord-specific)
Fetches a single associated model through another model.

```ruby
has_one :profile, through: :account
```

common in mongoid do this
```ruby
class User
  include Mongoid::Document
  has_one :account

  def profile
    account&.profile
  end
end
```

---

### 15. Circular Dependencies in Associations

## What is it?
A circular dependency happens when:
- a model references itself, or
- two models reference each other

Rails cannot guess the relationship correctly.
## How to fix it

Use these options:

- `class_name` → tells Rails which model to use
- `foreign_key` → tells Rails where the ID is stored
- `inverse_of` → tells Rails both sides are the same relationship
## Example (self-referential)

```ruby
class User < ApplicationRecord
  has_many :subordinates,
           class_name: "User",
           foreign_key: "manager_id",
           inverse_of: :manager

  belongs_to :manager,
             class_name: "User",
             foreign_key: "manager_id",
             inverse_of: :subordinates,
             optional: true
end
```
## What this solves

- prevents infinite loops
- avoids extra queries
- fixes nested object building
## Memory rule

Circular association = Rails needs hints

---

