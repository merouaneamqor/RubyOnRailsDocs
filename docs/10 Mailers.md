### **What is ActionMailer? (Short & Important)**

**ActionMailer** is Rails’ system for **sending emails**.

It works like a controller:
- mailer method = controller action  
- email views = controller views  
- sends emails instead of HTTP responses  

---

#### **Basic example**

```ruby
class UserMailer < ApplicationMailer
  def welcome_email(user)
    @user = user
    mail(to: @user.email, subject: "Welcome!")
  end
end
```
#### **How it works (must know)**
- The mailer method prepares data (`@user`)
- Rails renders email views from:
  ```
  app/views/user_mailer/welcome_email.html.erb
  app/views/user_mailer/welcome_email.text.erb
  ```
- If both views exist, Rails sends a **multipart email** automatically
#### **How you send the email**

```ruby
UserMailer.welcome_email(user).deliver_later
```

- `deliver_now` → sends immediately (blocking)
- `deliver_later` → background job (**best practice**)

#### **One-line mental model**
ActionMailer is a controller-like system that prepares data, renders email templates, and sends emails instead of web responses.

---

### 2. **What is the difference between `deliver_now` and `deliver_later`?**

| Method | Behavior |
|------|---------|
| `deliver_now` | Sends the email **immediately** and blocks the request |
| `deliver_later` | Enqueues the email as a background job |

✅ **Best practice:** always use `deliver_later` in real applications to avoid slowing requests.

---

### 3. **What are mailer previews?**
Mailer previews let you view emails in the browser **without sending them**.

```ruby
# test/mailers/previews/user_mailer_preview.rb
class UserMailerPreview < ActionMailer::Preview
  def welcome_email
    UserMailer.welcome_email(User.first)
  end
end
```

Accessible at:
```
http://localhost:3000/rails/mailers
```

---

### 4. **How do you add attachments to emails?**
Attachments are added **inside the mailer method**, not inline in `mail`.

```ruby
def invoice_email(user)
  attachments["invoice.pdf"] = File.read("path/to/invoice.pdf")
  mail(to: user.email, subject: "Invoice")
end
```

With ActiveStorage:

```ruby
attachments["photo.jpg"] = user.avatar.download
```

---

### 5. **How do you configure email delivery settings?**
In `config/environments/production.rb`:

```ruby
config.action_mailer.smtp_settings = {
  address: "smtp.gmail.com",
  port: 587,
  user_name: ENV["SMTP_USER"],
  password: ENV["SMTP_PASS"],
  authentication: "plain",
  enable_starttls_auto: true
}
```

---

### 6. **What is `ApplicationMailer`?**
Base mailer class for shared configuration.

```ruby
class ApplicationMailer < ActionMailer::Base
  default from: "no-reply@example.com"
  layout "mailer"
end
```

---

### 7. **What formats can a mailer render?**
- HTML  
- Plain text  
- Multipart (HTML + text) Email client chooses which one to display if it is old it will use text

📌 Rails **automatically creates multipart emails** if both  
`welcome_email.html.erb` and `welcome_email.text.erb` exist.

The format block is only needed for custom logic:

```ruby
mail(to: user.email, subject: "Welcome") do |format|
  format.html
  format.text
end
```

---

### 8. **How do you localize email content?**
Using I18n in views:

```erb
<%= t(".greeting", name: @user.name) %>
```

YAML:

```yaml
en:
  user_mailer:
    welcome_email:
      greeting: "Hello, %{name}"
```

---

### 9. **How do you test mailers?**
Mailer tests verify:
- email is sent  
- correct recipient  
- correct subject/body  

Example (Minitest):

```ruby
test "welcome email" do
  email = UserMailer.welcome_email(users(:one))

  assert_emails 1 do
    email.deliver_now
  end

  assert_equal ["to@example.com"], email.to
  assert_match "Welcome", email.subject
end
```

You can also test:
- `email.from`
- `email.body.encoded`
- attachments (`email.attachments.count`)

---

### 10. **How do you handle email errors and failures?**
Email delivery is **not guaranteed**. Failures must be handled explicitly.

#### 1️⃣ Background job retries
When using `deliver_later`, retries are handled by the job backend (e.g. Sidekiq).

- Temporary failures (SMTP timeout, network error) → retried automatically
- Retry count and delay are configurable in Sidekiq

```ruby
class ApplicationJob < ActiveJob::Base
  retry_on StandardError, attempts: 5
end
```

---

#### 2️⃣ Dead letter queues (DLQ)
If retries are exhausted:
- Job is moved to **Dead Jobs** (Sidekiq)
- Email is **not delivered**
- Requires manual inspection

This prevents infinite retry loops and system overload.

---

#### 3️⃣ Logging failed deliveries
Always log failures for visibility and monitoring.

```ruby
rescue_from StandardError do |error|
  Rails.logger.error("Email delivery failed: #{error.message}")
  raise error
end
```

In production, failures are usually tracked via:
- Sidekiq Web UI  
- Logs  
- Error monitoring (Sentry, Bugsnag)

---

#### 4️⃣ Best practice summary
- Always use `deliver_later`
- Rely on background job retries
- Monitor dead jobs
- Log and alert on repeated failures
