### 1. **How do you handle exceptions globally in Rails?**
Use `rescue_from` in `ApplicationController`:

```ruby
class ApplicationController < ActionController::Base
  rescue_from ActiveRecord::RecordNotFound, with: :not_found

  def not_found
    render json: { error: "Not Found" }, status: :not_found
  end
end
```

This prevents repetitive `begin/rescue` blocks.

---

### 2. **What is Sentry / Rollbar and why use them?**
Error tracking services that:
- record exceptions  
- capture stack traces  
- notify you instantly  
- show context (params, user, environment)  

Allows real-time monitoring of production issues.

---

### 3. **What are log levels in Rails?**

| Level | Meaning |
|-------|----------|
| `debug` | detailed info |
| `info` | general app flow |
| `warn` | something unexpected |
| `error` | an error occurred |
| `fatal` | system crash / impossible to continue |

Set level:

```ruby
config.log_level = :info
```

---

### 4. **What is Lograge?**
A gem that makes Rails logs **clean and structured**.

Default Rails logs are noisy.  
Lograge produces one-line logs per request:

```ruby
config.lograge.enabled = true
```

This helps with:
- performance analysis  
- debugging  
- production monitoring  

---

### 5. **What is a Request ID and why is it important?**
Each request receives a unique identifier:

```
X-Request-ID: abc123
```

Used to:
- trace logs across services  
- correlate background jobs to HTTP requests  
- make debugging easier  

Access in Rails:

```ruby
request.request_id
```

---

### 6. **How do you notify external services (Sentry, Slack) of errors?**
Inside `rescue_from` or custom middleware:

```ruby
Sentry.capture_exception(e)
```

Or Slack webhook:

```ruby
HTTParty.post(slack_url, body: { text: e.message }.to_json)
```

---

### 7. **What is Application Monitoring (APM)?**
Tools like:
- New Relic  
- Datadog  
- Skylight  

Track:
- slow requests  
- slow DB queries  
- memory leaks  
- CPU usage  
- error frequency  

These tools help diagnose production performance issues.

---

### 8. **How do you track slow queries in real-time?**
Use ActiveSupport notifications:

```ruby
ActiveSupport::Notifications.subscribe("sql.active_record") do |_, _, _, _, details|
  Rails.logger.warn "Slow Query: #{details[:sql]}" if details[:duration] > 50
end
```

---

### 9. **How do you prevent the app from crashing on unexpected errors?**
Use middleware to catch unhandled exceptions.

Example (Rack middleware):

```ruby
class ErrorHandler
  def call(env)
    @app.call(env)
  rescue => e
    Rails.logger.error(e.message)
    [500, {}, ["Internal Error"]]
  end
end
```

---

### 10. **Why is error monitoring essential for production apps?**
Because:
- Users won’t always report errors  
- Logs alone are not enough  
- Real-time alerts prevent downtime  
- Helps identify rare edge cases  
- Reduces debugging time drastically  

---

