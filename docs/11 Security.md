### 1. **What is CSRF? (Cross-Site Request Forgery)**

**Definition**  
CSRF happens when an attacker tricks a **logged-in user** into performing an action they didn’t intend.

**Example**
- You are logged into your bank
- You visit a malicious website
- That site submits a hidden POST request to your bank
- Your browser sends cookies automatically
- The action succeeds without your consent

**Rails Protection**
Rails uses **authenticity tokens** to verify requests.

```ruby
protect_from_forgery with: :exception
```

How it works:
- Rails stores a token in the session
- Rails inserts the token into every form
- On POST/PUT/PATCH/DELETE, Rails verifies the token
- If invalid or missing → request is rejected

This protection is **enabled by default** in Rails.

---

### 2. **What is XSS? (Cross-Site Scripting)**

**Definition**  
XSS occurs when malicious JavaScript is injected into a page and executed in users’ browsers.

**Example**
```html
<script>alert('hacked')</script>
```

**Rails Protection**
Rails automatically escapes output in views.

```erb
<%= user.name %>   <!-- SAFE -->
```

This prevents scripts from running.

**Dangerous methods**
```erb
<%= raw user.bio %>
<%= user.bio.html_safe %>
```

Only use these if the content is **trusted and sanitized**.

---

### 3. **What is SQL Injection and how does Rails prevent it?**

**Definition**  
SQL Injection happens when user input is treated as executable SQL.

**Bad example (unsafe)**
```ruby
User.where("email = '#{params[:email]}'")
```

If the user sends:
```sql
' OR 1=1 --
```

The query becomes dangerous.

**Rails Solution**
Use parameter binding:

```ruby
User.where("email = ?", params[:email])
```

Rails:
- escapes user input
- sends it separately from SQL
- prevents execution as SQL code

ActiveRecord query methods are safe by default.

---

### 4. **What are Strong Parameters?**

**Problem**
Users can submit extra parameters you didn’t expect.

**Example payload**
```json
{
  "user": {
    "email": "a@test.com",
    "admin": true
  }
}
```

**Rails Solution**
Explicitly whitelist attributes:

```ruby
params.require(:user).permit(:email, :name)
```

Only permitted attributes are assigned.
Unpermitted fields are ignored.

---

### 5. **What is Mass Assignment?**

**Definition**
Mass assignment is assigning many attributes at once from user input.

```ruby
User.new(params[:user])
```

This is dangerous because attackers could modify protected fields like:
- admin
- role
- balance

**Solution**
Strong parameters prevent mass assignment vulnerabilities.

---

### 6. **How does Rails handle password security?**

Rails uses **BCrypt** via `has_secure_password`.

```ruby
class User < ApplicationRecord
  has_secure_password
end
```

What this provides:
- virtual `password` attribute
- `password_confirmation`
- `authenticate(password)` method

Rails stores:
- NOT the password
- a `password_digest` (hashed + salted)

BCrypt:
- slow by design (brute-force resistant)
- salted hashes
- industry standard

---

### 7. **Authentication vs Authorization**

| Concept | Meaning |
|------|------|
| Authentication | Who are you? |
| Authorization | What can you access? |

Examples:
- Authentication → login, sessions, Devise
- Authorization → permissions, roles, Pundit, CanCanCan

Authentication happens **before** authorization.

---

### 8. **What is Session Hijacking?**

**Definition**
Session hijacking occurs when an attacker steals a user’s session cookie.

**Causes**
- No HTTPS
- XSS
- insecure cookies

**Rails Mitigation**
- encrypted cookies
- secure and httpOnly flags
- HTTPS
- reset session on login

```ruby
cookies.encrypted[:session_id]
```

```ruby
reset_session
```

---

### 9. **What is Parameter Tampering?**

**Definition**
Users modify parameters sent from the client.

Example:
```html
<input type="hidden" name="price" value="10">
```

User changes it to `1`.

**Mitigation**
- strong parameters
- server-side validations
- calculate sensitive values on the backend

Never trust client input.

---

### 10. **What does Rails automatically escape?**

Rails automatically escapes:
- HTML output in ERB
- unsafe strings
- injected scripts

Rails does NOT protect you from:
- raw SQL strings
- `raw`
- `html_safe`
- JavaScript inside script tags
- manual string interpolation

Security is a mix of **Rails defaults + developer discipline**.

---

## Interview Tip

If you can clearly explain:
- what the attack is
- why it is dangerous
- how Rails prevents it

You are already at **mid-level Rails interview readiness**.
