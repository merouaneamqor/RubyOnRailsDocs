### 1. **What format do Rails APIs commonly return?**
JSON.  
Often using:
- Jbuilder  
- ActiveModel::Serializer  
- FastJSONAPI  
- Blueprinter  

Example:

```ruby
render json: { id: @user.id, name: @user.name }
```

---

### 2. **How do you build an API-only Rails app?**

```bash
rails new myapi --api
```

This:
- removes views  
- disables helpers/layouts  
- configures middleware for JSON responses  

---

### 3. **What is `render json:` used for?**

```ruby
render json: @user, status: :ok
```

Rails converts Ruby objects to JSON.

---

### 4. **How do you handle authentication in an API?**
Common methods:
- JWT  
- Doorkeeper (OAuth2)  
- API tokens  
- Devise with `devise-jwt`  

JWT example:

```ruby
jwt = JWT.encode(payload, secret_key)
```

Then:

```ruby
before_action :authenticate_user!
```

---

### 5. **What are strong parameters and why needed in APIs?**

```ruby
params.require(:user).permit(:name, :email)
```

Prevents mass assignment attacks.

---

### 6. **How to return proper HTTP status codes?**

```ruby
render json: { error: "Not found" }, status: :not_found
```

Most common:

| Code | Meaning |
|-------|---------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable Entity |
| 500 | Server Error |

---

### 7. **What is CORS and why required?**
CORS allows APIs to be called from other domains (like a frontend app).

Enable in:

```ruby
# config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "*"
    resource "*", headers: :any, methods: [:get, :post, :patch, :put, :delete]
  end
end
```

---

### 8. **What are serializers?**
Serialize models into JSON.

Example (ActiveModel::Serializer):

```ruby
class UserSerializer < ActiveModel::Serializer
  attributes :id, :email
end
```

Render:

```ruby
render json: @user, serializer: UserSerializer
```

---

### 9. **How do you version your API?**
By namespacing:

```ruby
namespace :api do
  namespace :v1 do
    resources :users
  end
end
```

Controllers in:

```
app/controllers/api/v1/users_controller.rb
```

---

### 10. **How do you paginate API results?**
Using gems like:
- Kaminari  
- Pagy  
- WillPaginate  

Example (Pagy):

```ruby
pagy, users = pagy(User.all)
render json: { users: users, pagy: pagy_metadata(pagy) }
```

---

### 11. **How to document APIs?**
Tools:
- RSwag (OpenAPI/Swagger generator)  
- Apipie  
- Postman collections  

Swagger is common for enterprise apps.

---

### 12. **How do you prevent over-fetching in APIs?**( Active record )
Use `select`:

```ruby
User.select(:id, :name).limit(20)

User.only(:id, :email)  #MONGOID
```

Reduce payload size.

---

### 13. **How do you optimize N+1 in API endpoints?**

```ruby
User.includes(:posts).limit(10)
```

---

### 14. **How do you return partial fields?**
Rails 7 supports:

```ruby
render json: @user, only: [:id, :email]
```

Or except:

```ruby
render json: @user, except: [:password_digest]
```

---

