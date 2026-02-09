### 1. **What is ActiveStorage?**
ActiveStorage is Rails’ built-in system for:
- file uploads  
- attaching images/videos/docs to models  
- processing variants  
- cloud storage integrations (S3, GCS, Azure)

---

### 2. **How do you attach files to models?**

#### One attachment:
```ruby
class User < ApplicationRecord
  has_one_attached :avatar
end
```

#### Multiple attachments:
```ruby
class Post < ApplicationRecord
  has_many_attached :images
end
```

---

### 3. **How do you upload files from a form?**

```erb
<%= form.file_field :avatar %>
```

Files are saved by calling:

```ruby
user.update(avatar: params[:avatar])
```

---

### 4. **How do you generate variants (image resizing)?**

```ruby
user.avatar.variant(resize_to_limit: [300, 300]).processed
```

Then render:

```erb
<%= image_tag user.avatar.variant(resize_to_limit: [300, 300]) %>
```

Requires ImageMagick / libvips.

---

### 5. **What are direct uploads?**
Uploads from the browser **straight to S3** (skips Rails server).
Improves performance & reduces server load.

```
data-direct-upload="true"
```

Rails handles:
- signed blob URLs  
- upload progress  
- attaching after upload  

---

### 6. **How do you generate a secure URL for downloads?**

Signed URL:

```ruby
user.avatar.url
```

Or:

```ruby
rails_blob_url(user.avatar, disposition: "attachment")
```

These URLs automatically:
- expire  
- prevent unauthorized access  

---

### 7. **How does ActiveStorage store metadata?**
In two DB tables:
- `active_storage_blobs`  
- `active_storage_attachments`  

Metadata includes:
- filename  
- content type  
- byte size  
- checksums  

Actual file lives in:
- local disk (`storage/`)  
- S3  
- cloud bucket  

---

### 8. **How do you purge (delete) an attachment?**

```ruby
user.avatar.purge
```

Deletes record + file.

---

### 9. **How do you check if a model has an attached file?**

```ruby
user.avatar.attached?
```

---

### 10. **How do you validate attachment types?**
Using ActiveStorage validations (gem: `active_storage_validations`):

```ruby
validates :avatar, content_type: ['image/png', 'image/jpg']
```

---

### 11. **What is the difference between `purge` and `purge_later`?**
- `purge` → deletes immediately  
- `purge_later` → deletes in a background job  

```ruby
user.avatar.purge_later
```

---

### 12. **Why use ActiveStorage instead of CarrierWave/Paperclip?**
- Built into Rails  
- Simple API  
- Direct uploads  
- Flexible cloud integration  
- Variant processing built-in  

---

## ActiveRecord vs Mongoid — Core Concepts (Quick Notes)

### ActiveRecord (SQL)
- Fields are defined **in the database**, not in the model
- Models **do not declare fields**
- Fields come from migrations + `schema.rb`
- Model focuses on:
  - validations
  - associations
  - callbacks
  - business logic
- Database is the **single source of truth**

---

### Mongoid (MongoDB)
- Fields are defined **inside the model**
- No strict database schema
- Model defines structure using `field`
- Common to add fields via concerns

---

### Migrations (ActiveRecord)
- A migration is a **historical record**
- Once run, it **should never be edited**
- Schema changes are done via **new migrations**
- `schema.rb` represents the **current state**
- Migrations = history, Schema = now

---

### Why fields are NOT in ActiveRecord models
- Avoids schema duplication
- Prevents mismatches between code and DB
- Ensures consistency across environments
- Rails favors correctness over local readability

---

### Readability in ActiveRecord
- Use `schema.rb` to see fields
- Use `annotate` gem for schema comments
- IDEs read schema for autocomplete

---

### Key Mental Models
- ActiveRecord model = **behavior**
- Database schema = **structure**
- Migration = **change over time**
- Mongoid model = **structure + behavior**

---

### One-line Interview Answer
> ActiveRecord defines fields at the database level via migrations, while Mongoid defines fields directly in the model due to MongoDB’s schemaless nature.
