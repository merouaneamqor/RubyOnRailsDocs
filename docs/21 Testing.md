### 1. **What testing frameworks does Rails use?**
Rails includes **Minitest** by default.

Most teams use:
- **RSpec** → most popular Rails testing library  
- **FactoryBot** → factories  
- **Faker** → test data  
- **Capybara** → integration tests  

---

### 2. **What types of tests exist in Rails?**

| Type | Purpose |
|-------|---------|
| **Model tests** | validate logic, associations, validations |
| **Controller tests** | verify actions & responses |
| **Request tests** | full HTTP flow |
| **System tests** | simulate browser (Capybara) |
| **Feature tests** | user flows |
| **Unit tests** | plain Ruby classes |

---

### 3. **What are fixtures?**
YAML-based seed data used for tests.

```yaml
users:
  adam:
    email: "adam@example.com"
```

But most teams prefer **FactoryBot**.

---

### 4. **What is FactoryBot and why is it better than fixtures?**
FactoryBot generates dynamic test data.

Example factory:

```ruby
FactoryBot.define do
  factory :user do
    name { "Adam" }
    email { Faker::Internet.email }
  end
end
```

Create user:

```ruby
create(:user)
```

Better because:
- flexible  
- reusable  
- avoids fixture coupling  

---

### 5. **How do you test model validations?**

```ruby
it "validates presence of email" do
  user = User.new
  expect(user).not_to be_valid
  expect(user.errors[:email]).to include("can't be blank")
end
```

---

### 6. **How do you test controller actions?** (RSpec request specs preferred)

```ruby
get "/users"
expect(response).to have_http_status(:ok)
```

Test POST:

```ruby
post "/users", params: { user: { name: "Adam" } }
expect(response).to redirect_to(user_path(User.last))
```

---

### 7. **What are Request Specs (RSpec)?**
Request specs test API endpoints or controller routes end-to-end.

```ruby
describe "GET /users" do
  it "returns success" do
    get users_path
    expect(response).to have_http_status(:ok)
  end
end
```

---

### 8. **How do you test background jobs?**

```ruby
ActiveJob::Base.queue_adapter = :test

expect {
  SendEmailJob.perform_later(user)
}.to have_enqueued_job
```

---

### 9. **How do you test mailers?**

```ruby
email = UserMailer.welcome_email(user)
expect(email.to).to eq([user.email])
expect(email.subject).to eq("Welcome!")
```

---

### 10. **What is Capybara used for?**
System tests simulating browser interaction.

```ruby
visit "/login"
fill_in "Email", with: "test@example.com"
click_button "Log in"
expect(page).to have_content("Welcome")
```

---

### 11. **How do you stub external services?**

Using WebMock:

```ruby
stub_request(:get, "https://api.example.com")
  .to_return(body: { ok: true }.to_json)
```

Or using VCR to record HTTP responses.

---

### 12. **How do you test exceptions?**

```ruby
expect { dangerous_method }.to raise_error(MyError)
```

---

### 13. **How do you test JSON API responses?**

```ruby
get "/api/users"
json = JSON.parse(response.body)
expect(json["success"]).to eq(true)
```

Use `json.dig(...)` for nested structures.

---

### 14. **How do you test model callbacks?**
Prefer testing behavior, not callbacks directly.

Example:

```ruby
it "sets uuid on create" do
  user = create(:user)
  expect(user.uuid).not_to be_nil
end
```

---

### 15. **What is Test Database Transaction Rollback?**
Each test runs inside a DB transaction → rolled back after finishing.  
This ensures **clean database state** for every test.

Enabled by default in Rails.

---




## Example:
## 1️⃣ User model (Mongoid example)

```ruby
# app/models/user.rb
class User
  include Mongoid::Document
  include Mongoid::Timestamps

  field :email, type: String
  field :admin, type: Boolean, default: false
  field :active, type: Boolean, default: true

  validates :email, presence: true, uniqueness: true

  def admin?
    admin
  end

  def deactivate!
    update!(active: false)
  end
end
```

---

## 2️⃣ Fabricator (factory)

```ruby
# spec/fabricators/user_fabricator.rb
Fabricator(:user) do
  email { Faker::Internet.email }
  admin false
  active true
end

Fabricator(:admin_user, from: :user) do
  admin true
end
```

### What this gives you

- `Fabricate(:user)` → normal user  
- `Fabricate(:admin_user)` → admin user  

---

## 3️⃣ RSpec model spec

```ruby
# spec/models/user_spec.rb
require "rails_helper"

RSpec.describe User, type: :model do
  describe "validations" do
    it "is valid with valid attributes" do
      user = Fabricate.build(:user)
      expect(user).to be_valid
    end

    it "is invalid without an email" do
      user = Fabricate.build(:user, email: nil)
      expect(user).to be_invalid
    end
  end

  describe "#admin?" do
    it "returns false for a normal user" do
      user = Fabricate(:user)
      expect(user.admin?).to be false
    end

    it "returns true for an admin user" do
      admin = Fabricate(:admin_user)
      expect(admin.admin?).to be true
    end
  end

  describe "#deactivate!" do
    it "sets active to false" do
      user = Fabricate(:user)

      user.deactivate!

      expect(user.active).to be false
    end
  end
end
```

---

## 4️⃣ What each tool is doing (very important)

### RSpec
- Runs the tests  
- Provides `describe`, `it`, `expect`

### Fabrication
- Creates test users  
- Keeps tests clean  
- Avoids manual setup

### Mongoid
- Persists documents  
- Handles validations  
- Updates fields



## Youtube Vid's
https://www.youtube.com/watch?v=zVAsZFcoUt0
https://www.youtube.com/watch?v=4ljXbJyh1t8&list=PLbTv9eGiI03snRYGF-WNU6NooyAxELPT1&index=3


![[Pasted image 20251215211443.png]]