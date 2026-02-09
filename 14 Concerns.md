### 1. **What are concerns in Rails?**
Concerns are modules stored in:

```
app/models/concerns/
app/controllers/concerns/
```

They allow you to extract reusable logic from models or controllers.

Example:

```ruby
module Trackable
  extend ActiveSupport::Concern

  included do
    before_create :set_uuid
  end

  def set_uuid
    self.uuid = SecureRandom.uuid
  end
end
```

Use it:

```ruby
class Order < ApplicationRecord
  include Trackable
end
```

---

### 2. **Why use concerns?**
- DRY shared logic  
- Cleaner, smaller classes  
- Encapsulation of related behavior  

Useful for:
- shared validation logic  
- callbacks  
- shared scopes  
- controller filters  

---

### 3. **When NOT to use concerns?**
Avoid concerns when:
- It holds **too much logic**  
- The behavior is **not truly shared**  
- The code mixes unrelated responsibilities  

Concerns can cause:
- hidden dependencies  
- difficult debugging  
- unclear flow  

If code in a concern gets too complex → move to a **service object** instead.

---

### 4. **What is the difference between a concern and a module?**
A concern is a Rails-specific module with lifecycle hooks:

```ruby
included do
  # runs inside the class
end
```

Standard Ruby modules don't provide this structure.

Concerns make mixing behavior more predictable and readable.

---

### 5. **How do concerns help with models?**
They let you group related model logic:

- formatting  
- soft-delete behavior  
- auditing  
- tagging  
- search helpers  

Example:

```ruby
module SoftDeletable
  extend ActiveSupport::Concern

  included do
    scope :active, -> { where(deleted_at: nil) }
  end

  def soft_delete
    update(deleted_at: Time.current)
  end
end
```

---

### 6. **How do concerns help with controllers?**
They help reuse:
- authentication  
- authorization  
- filters  
- shared JSON rendering methods  

Example:

```ruby
module ApiResponder
  extend ActiveSupport::Concern

  def render_success(data)
    render json: { success: true, data: data }
  end
end
```

---

