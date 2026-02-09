## Rails Application Configuration — Important Interview Q&A

---

### 1. **Why is ActiveRecord disabled in this Rails app?**
ActiveRecord is commented out because the application uses **Mongoid** for database persistence instead of SQL.  
Disabling unused frameworks improves boot time and reduces memory usage.

---

### 2. **Why are custom autoload paths added?**
```ruby
config.autoload_paths += %W(#{config.root}/config/routes)
config.autoload_paths += %W(#{config.root}/lib)
```
These paths are added so Rails (using Zeitwerk) can automatically load classes/modules from custom folders without requiring manual `require` statements.

---

### 3. **What does `Rack::Deflater` do in middleware?**
```ruby
config.middleware.use Rack::Deflater
```
It compresses HTTP responses using gzip, reducing response size and improving performance in production.

---

### 4. **What does `Mongoid::QueryCache::Middleware` do?**
```ruby
config.middleware.use Mongoid::QueryCache::Middleware
```
It caches repeated MongoDB queries during a single request, reducing database load and improving performance.

---

### 5. **Why set a custom application time zone?**
```ruby
config.time_zone = 'West Central Africa'
```
This ensures all timestamps (created_at, updated_at, background job times) follow a consistent time zone for the entire application.

---

### 6. **Why configure I18n available locales and default locale?**
```ruby
config.i18n.available_locales = %w(fr en ar)
config.i18n.default_locale = 'fr'
```
These settings enable a multilingual application and ensure Rails loads the correct translation files.

---

### 7. **What does `exceptions_app = self.routes` mean?**
```ruby
config.exceptions_app = self.routes
```
This tells Rails to route errors (404, 500, etc.) through the application's router, allowing fully customized error pages instead of static HTML files.

---

### 8. **Why are CSP headers added?**
```ruby
config.action_dispatch.default_headers = {
  'Content-Security-Policy' => "frame-ancestors 'self' https://altibbi.com"
}
```
This is a security measure.  
`frame-ancestors` restricts which domains can embed the app in an iframe, protecting against clickjacking attacks.

---

### 9. **Why exclude certain paths from SSL redirection?**
```ruby
config.ssl_options = { 
  redirect: { 
    exclude: -> request { ['/payment/cmi', '/payment/fatourati'].any? { |link| request.path_info.include?(link) } }
  } 
}
```
Some external payment gateways require **HTTP** callback URLs.  
If Rails forced these routes to HTTPS, payment callbacks would fail.

---

### 10. **Why are some Rails frameworks commented out?**
```ruby
# require "action_text/engine"
# require "action_mailbox/engine"
# require "active_storage/engine"
```
They are disabled because the application does not use them.  
Removing unused frameworks reduces memory usage and improves application boot speed.

---

### 11. **What is a Railtie and why is it important?**
A Railtie is the mechanism Rails uses to let frameworks and gems hook into the Rails initialization process.  
ActiveRecord, Mongoid, Sidekiq Rails integration, and Devise all use Railties to load configuration, middleware, and initializers.

---

### 12. **What does Bundler.require(*Rails.groups) do?**
```ruby
Bundler.require(*Rails.groups(assets: %w(development test)))
```
It loads gems from the Gemfile depending on the environment.  
This allows asset-related gems to load only in development and test, improving production performance.

---

### 13. **Why prefer `Time.zone.now` / `Time.current` instead of `Time.now`?**
`Time.now` uses the **server’s system time zone**.  
`Time.zone.now` (or `Time.current`) uses the **Rails app time zone** (`config.time_zone`).

**Rule:** In Rails apps, prefer `Time.current`.

---

### 14. **What does `config.time_zone` do?**
It sets the **default time zone for your application** (how Rails displays and interprets times).

Example:
```ruby
config.time_zone = "UTC"
```

---

### 15. **How do you display time in a user’s time zone?**
Store times in a consistent zone (usually UTC), then convert on display:

```ruby
Time.use_zone(user.time_zone) { @post.created_at.in_time_zone }
```

---

### 16. **What is `I18n.t`?**
`I18n.t` translates a key into the current language.

Example:
```ruby
I18n.t("welcome.title")
```

---

### 17. **Where are translation files stored?**
Usually in:
```
config/locales/*.yml
```

Example:
```yml
en:
  welcome:
    title: "Welcome"
```

---

### 18. **How do you switch locales?**
Set `I18n.locale` per request (often in a `before_action`).

Example:
```ruby
I18n.locale = params[:locale] || I18n.default_locale
```

---

