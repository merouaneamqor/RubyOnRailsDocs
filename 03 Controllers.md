### 1. **What is a controller in Rails?**
A controller:
- receives the request  
- loads/updates models  
- selects a view  
- returns an HTTP response  

Acts as the middle layer between browser → models → views.

---

### 2. **What is `before_action`?**
A filter that runs **before controller actions**.

Used for:
- authentication  
- loading records  
- permission checks  

```ruby
before_action :set_user, only: [:show, :edit]
```

---

### 3. **What is `skip_before_action`?**
It prevents a filter from running on specific actions.

```ruby
skip_before_action :authenticate_user!, only: :index
```

---

### 4. **What is `around_action`?**
Wraps the action execution.

```ruby
around_action :log_request

def log_request
  puts "Before"
  yield
  puts "After"
end
```

---

### 5. **Difference between `render` and `redirect_to`?**

| Method | Meaning |
|--------|---------|
| **render** | Returns a view **immediately**, same request |
| **redirect_to** | Issues a **new HTTP request** to a different URL |

Example:

```ruby
render :show
redirect_to user_path(@user)
```

---

### **6. Difference between `params`, `session`, and `cookies`**

**params**
- Only for _one request_
    
- Comes from URL/form/JSON
    
- Used to read user input
- 
**session**
- Lasts until browser closes or expires
    
- Stored in browser as an **encrypted cookie**
    
- Only the server can read it
    
- Used for login state

**cookies**

- Stored in browser
    
- Can last days or years (you can controll it)
    
- User can read/modify (unless encrypted)
    
- Used for preferences or “remember me”
- 
---

### 7. **What are strong parameters?**
Whitelisting attributes to avoid mass assignment vulnerabilities.

```ruby
params.require(:user).permit(:name, :email)
```

---

### 8. **What is CSRF protection?**
Rails includes a hidden authenticity token in forms to prevent **malicious form submissions** from other sites.

Automatically handled by:

```ruby
protect_from_forgery with: :exception
```

---

### 9. **How do you handle exceptions globally?**
Use `rescue_from` in `ApplicationController` to catch errors across all controllers.

```ruby
class ApplicationController < ActionController::Base
  rescue_from ActiveRecord::RecordNotFound, with: :not_found

  private

  def not_found
    render json: { error: "Not Found" }, status: :not_found
  end
end

```

---

### 10. **What is `respond_to` / `format.json`?**
Allows actions to output different formats (HTML / JSON / XML).

```ruby
respond_to do |format|
  format.html
  format.json { render json: @user }
end
```

---

### 11. **How to send custom headers?**

```ruby
response.set_header("X-Request-ID", SecureRandom.uuid)
```

---

### 12. **What is `head :ok` and `render`?**
`head` sends only an HTTP status with **no response body at all**.

Examples:
```ruby

head :no_content   # 204 No Content
head :ok           # 200 OK
```
Useful for API endpoints that don't need to return data.

Difference from `render`:
render status: :ok, plain: ""   # Sends an empty *string* body, not truly empty

---

### 14. **What is `after_action`?**
Runs after a controller action finishes. Useful for logging or cleanup.

```ruby
after_action :log_activity

def log_activity
  puts "Action completed"
end
```

---

### 15. **What is the difference between `flash` and `flash.now`?**
- `flash` → persists to the **next request** (use with redirect_to)  
- `flash.now` → only for the **current request** (use with render)

```ruby
redirect_to users_path, notice: "User created"   # flash
flash.now[:alert] = "Invalid data"               # flash.now
```

---

### 16. **What is `ApplicationController`?**
Base controller. Used for:
- shared filters
- global exception handling
- shared helper methods

```ruby
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
end
```

---

### 17. **What do `only:` and `except:` do in filters?**

```ruby
before_action :authorize, only: [:edit, :update]
before_action :log_request, except: [:index]
```

---

### 18. **What is controller inheritance?**

```ruby
class AdminController < ApplicationController
  before_action :require_admin
end

class Admin::UsersController < AdminController
  # inherits require_admin
end
```

---

### 19. **How do you send HTTP status codes?**

```ruby
render json: @user, status: :created                          # 201
render json: { error: "Invalid" }, status: :unprocessable_entity  # 422
head :no_content                                              # 204
```

---

### 20. **What is `prepend_before_action`?**
Runs BEFORE inherited before_actions.

```ruby
prepend_before_action :check_maintenance
```

---

### 21. **What are controller concerns?**

```ruby
module Authenticable
  extend ActiveSupport::Concern

  included do
    before_action :authenticate_user!
  end
end
```
Usage:

```ruby
class AdminController < ApplicationController   
	include Authenticable 
end
```

---

### 22. **What are the `request` and `response` objects?**

```ruby
request.url
request.method
request.headers["User-Agent"]

response.set_header("X-Custom", "123")
```
