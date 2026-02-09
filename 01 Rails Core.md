### 1. **What is MVC in Rails?**
MVC separates application responsibilities:
- **Model** → data + business rules  
- **View** → rendering HTML/JSON  
- **Controller** → handles requests, updates models, chooses views  

---

### 2. **Explain the Rails request lifecycle.**
1. Browser sends HTTP request  
2. Router matches route  
3. Controller action runs  
4. Model operations (optional)  
5. View rendered  
6. Rails returns HTTP response  

---

### 3. **What is “Convention over Configuration”?**
Rails provides default rules for naming and structure.  
If you follow these conventions, Rails automatically connects your controllers, views, models, and routes without extra configuration.
#### **Example — Controller → View folder**
If you have a controller:

```ruby
class UsersController < ApplicationController
  def index; end
end
```

Rails automatically searches for the view in:

```
app/views/users/index.html.erb
```

You don't need to tell Rails where the file is — the name of the controller decides the view folder.

---

#### **Example — REST routes generated automatically**
Using:

```ruby
resources :users
```

Rails automatically creates a full set of routes mapped to `UsersController`:

- GET /users → index  
- GET /users/:id → show  
- POST /users → create  
- PATCH /users/:id → update  
- DELETE /users/:id → destroy  

No manual route definitions needed.



---

### 4. **What is DRY in Rails?**
DRY means **“Don’t Repeat Yourself.”**  
Rails encourages removing duplicated logic by moving it into reusable components.

**Examples:**

- **Helper method reuse**
```ruby
# app/helpers/application_helper.rb
def formatted_date(date)
  date.strftime("%d/%m/%Y")
end
```

- **Partial reuse**
```erb
<%= render "shared/form_errors", object: @user %>
```

- **Concern reuse**
```ruby
module Trackable
  extend ActiveSupport::Concern
  included { before_save :set_updated_by }
end
```

- **Service object reuse**
```ruby
class SendEmail
  def self.call(user)
    UserMailer.welcome(user).deliver_later
  end
end
```


---

### 5. **What is Rack?**
Rack is the interface between the web server and Rails.  
It takes the raw HTTP request from the server, processes it through middleware, and then passes it to your Rails app.

Rails uses Rack middleware for:
- logging  
- parsing params  
- handling cookies and sessions  
- CSRF/security layers  

Rack prepares the request → Rails handles it → Rack returns the response.

---

### 6. **What is Zeitwerk?**
Zeitwerk is Rails’ autoloader.  
It automatically loads classes and modules by matching constant names to file paths.

Examples:
- `app/models/user.rb` → `class User`
- `app/services/payments/process_payment.rb` → `Payments::ProcessPayment`

Zeitwerk removes the need for manual `require` statements and enforces clean project structure.


---

### 7. **Purpose of `config/application.rb`?**
`config/application.rb` holds global configuration for the entire Rails app.

It controls:
- autoload/eager load paths  
- default time zone  
- which Rails frameworks to load (ActiveRecord, ActiveJob, etc.)  
- Rack middleware settings  
- generators configuration  

This file defines how the whole application behaves. 

---
### 8. **What is Puma?**
Puma is the **web server** that receives HTTP requests from the browser.  
It uses Rack to communicate with Rails.

Chain:  
Browser → **Puma (server)** → **Rack (interface)** → **Rails (your app)**
