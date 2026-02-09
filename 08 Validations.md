### 1. **What are the common validation types in Rails?** (Both)
Rails provides built-in validation helpers:

```ruby
validates :name, presence: true
validates :email, uniqueness: true
validates :age, numericality: { greater_than: 18 }
validates :status, inclusion: { in: %w[pending active archived] }
validates :description, length: { maximum: 200 }
```

---

### 2. **How does `validates_uniqueness_of` work internally?** (Both)
It performs a **SELECT query** before saving to check if another record exists.

⚠️ Note: Not safe alone.  
Must pair with a **unique DB index** to avoid race conditions.

---

### 3. **What are custom validators?** (Both)

Custom validators let you write **your own validation logic**
when built-in validations aren’t enough.

They run during validation and add errors to the model.

#### Inline (one-off)

```ruby
validate :email_must_be_gmail

def email_must_be_gmail
  errors.add(:email, "must be Gmail") unless email&.end_with?("@gmail.com")
end
```

#### Reusable (class-based)

```ruby
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    record.errors.add(attribute, "invalid email") unless value&.include?("@")
  end
end

validates :email, email: true
```

**Use inline** for simple, model-specific rules.  
**Use class-based** when the rule is reusable.

---

### 4. **What does `errors.add` do?** (Both)
Adds an error message to a specific attribute.

```ruby
errors.add(:name, "cannot be blank")
```

Accessible via:

```ruby
user.errors.full_messages
```

---

### 5. **What are conditional validations?** (Both)
Validations that run only if a condition is true.

```ruby
validates :discount, presence: true, if: -> { premium? }
```

Or using symbols:

```ruby
validates :age, numericality: true, unless: :guest?
```

---

### 6. **What is the difference between `valid?` and `invalid?`?** (Both)
- `valid?` → returns true if **no** errors  
- `invalid?` → returns true if **any** errors

```ruby
user.valid?
user.invalid?
```

---

### 7. **How do you skip validations?** (Both)

```ruby
user.save(validate: false)
```

⚠️ Dangerous — may create invalid data.

---

### 8. **What is `validate :method_name` used for?** (Both)
Runs a custom validation method.

```ruby
validate :check_date_range
```

```ruby
def check_date_range
  errors.add(:end_date, "must be after start date") if end_date < start_date
end
```

---

### 9. **How to validate associated models?** (Both)

```ruby
validates_associated :comments
```

⚠️ Usually discouraged — use form objects instead.

---
