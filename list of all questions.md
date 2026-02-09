## List of all questions (generated from files)

**Total questions (### headings): 418**

## 01 Rails Core.md
- 1. **What is MVC in Rails?**
- 2. **Explain the Rails request lifecycle.**
- 3. **What is “Convention over Configuration”?**
- 4. **What is DRY in Rails?**
- 5. **What is Rack?**
- 6. **What is Zeitwerk?**
- 7. **Purpose of `config/application.rb`?**
- 8. **What is Puma?**

## 02 Routing.md
- 1. **What does `resources` generate?**
- 2. **Difference between `resource` and `resources`?**
- 3. **What are nested routes?**
- 4. **What is a `member` route?**
- 5. **What is a `collection` route?**
- 6. **What are route concerns?**
- 7. **How to create a custom route?**
- 8. **What happens if a route exists but the controller action does not?**
- 9. **How to list all routes?**
- 10. **Difference between `scope`, `namespace`, and `controller` routing blocks**
- 11. **What is `root` and how do you define it?**
- 12. **What are route constraints?**
- 13. **What is the difference between `match` and specific HTTP verbs?**
- 14. **What are shallow nested routes?**
- 15. **How do you skip or limit RESTful routes?**
- 16. **What are route helpers and how do you use them?**
- 17. **Difference between redirects in routes vs controllers**
- ### 18. **How to handle different formats (HTML, JSON)?**
- 19. **What is `direct` for custom URL helpers?**
- 20. **What happens if two routes match the same URL?**
- 21 How to create routes with params
- 22. **What are glob routes and when would you use them?**

## 03 Controllers.md
- 1. **What is a controller in Rails?**
- 2. **What is `before_action`?**
- 3. **What is `skip_before_action`?**
- 4. **What is `around_action`?**
- 5. **Difference between `render` and `redirect_to`?**
- **6. Difference between `params`, `session`, and `cookies`**
- 7. **What are strong parameters?**
- 8. **What is CSRF protection?**
- 9. **How do you handle exceptions globally?**
- 10. **What is `respond_to` / `format.json`?**
- 11. **How to send custom headers?**
- 12. **What is `head :ok` and `render`?**
- 14. **What is `after_action`?**
- 15. **What is the difference between `flash` and `flash.now`?**
- 16. **What is `ApplicationController`?**
- 17. **What do `only:` and `except:` do in filters?**
- 18. **What is controller inheritance?**
- 19. **How do you send HTTP status codes?**
- 20. **What is `prepend_before_action`?**
- 21. **What are controller concerns?**
- 22. **What are the `request` and `response` objects?**

## 04 Models.md
- 1. What is ActiveRecord?
- 2. **What is ORM?**
- 3. **What naming conventions does Rails use for models/tables?**
- 4. **What is Single Table Inheritance (STI)?**
- 5. **Difference between `save`, `save!`, `create`, and `create!`?**
- Common behavior (ActiveRecord ✅ / Mongoid ✅)
- 6. **What is the difference between `update` and `update!`?**
- 7. What are callbacks?
- 8. Why can callbacks be dangerous?
- 9. **What is a transaction?**
- 10. **Difference between model validations and DB validations**
- 11. **How do errors get stored?**
- 12. **What does `accepts_nested_attributes_for` do?**
- 13. **What is the `attribute` API?**
- 14. **What is `enum` in models?**
- 15. **What is the purpose of validations?**
- 16. **What is dirty tracking?**
- 17. **Difference between `update`, `update_attribute`, and `update_column`?**
- 18. **What are virtual attributes?**
- 19. **What does `delegate` do?**
- 20. **What are `serialize` and `store` used for?**
- 21. **What is callback order (high level)?**
- 22. **`after_save` vs `after_commit`**
- 23. **What are readonly records?**
- **Transactions vs Atomicity (SQL vs MongoDB)**

## 05 ActiveRecord Queries.md
- 1. **What is the difference between `find` and `find_by`?**
- 2. **What does `.where` return?**
- 3. **Difference between `.select` and `.pluck`?**
- 4. **What is the difference between `.joins` and `.includes`?**
- 5. **What is eager loading vs lazy loading?**
- 6. **Difference between `includes`, `preload`, and `eager_load`?**
- 7. **How to perform raw SQL?**
- 8. **What are scopes?**
- 9. **Why is `default_scope` dangerous?**
- 10. **How does ActiveRecord lazy loading work?**
- 11. **What does `.distinct` do?**
- 12. **Difference between `.count`, `.size`, `.length`?**
- 13. **What does `.group` / `.having` do?**
- 14. **What does `.order` do?**
- 15. **How do you prevent SQL injection in ActiveRecord?**
- 16. **What is `find_each` and when to use it?**
- 17. **What is optimistic locking?**
- 18. **What is pessimistic locking?**
- 19. **What is counter caching?**
- 20. **What is `touch: true`?**
- 21. **What are composite indexes?**
- 22. **How do you debug slow ActiveRecord queries?**
- 23. **What does `.or` do?**
- 24. **How to use `select` to reduce loaded columns?**
- 25. **How to chain multiple queries?**
- 26. **What is `left_joins` and when do you use it?**
- 27. **What is `merge`?**
- 28. **`find_each` vs `in_batches`**
- 29. **`exists?` vs `any?` vs `present?`**
- 30. **What is `pick`?**
- 31. **What does `unscoped` do?**
- 32. **How does ActiveRecord build SQL queries?**
- 33. **Difference between `.where` and `.find`?**

## 06 Associations.md
- 1. **What types of associations exist in Rails?**
- 2. **What does `belongs_to` mean?**
- 3. **What does `has_many` mean?**
- 4. **What is `has_many :through`?**
- 5. **What is HABTM (`has_and_belongs_to_many`)?**
- 6. **What is a polymorphic association?**
- 7. **What does `dependent:` do?**
- 8. **What is `inverse_of` and why is it useful?**
- 9. **What is eager loading?**
- 10. **What is lazy loading?**
- 11. **What is the N+1 problem?**
- 12. **How do you fix N+1?**
- 13. **Can you validate associated records?**
- 14. **What is `has_one :through`?**
- 15. **How do you handle circular dependencies in associations?**

## 07_Migrations_and_Database.md
- 1. **What is a migration in Rails?**
- 2. **Difference between `change`, `up`, and `down`?**
- 3. **What is a reversible migration?**
- 4. **What is `schema.rb`?**
- 5. **Difference between `schema.rb` and `structure.sql`?**
- 6. **How do you rollback migrations?**
- 7. **How do you add indexes?**
- 8. **What is a foreign key constraint?**
- 9. **What is `rails db:seed` used for?**
- 10. **What is JSONB (PostgreSQL)?**
- 11. **What is PostgreSQL full-text search?**
- 12. **How do you check slow queries using EXPLAIN?**
- 13. **What is the difference between `db:reset` and `db:drop + db:create + db:migrate`?**
- 14. **How do you change column types safely?**
- 15. **When should you add database-level validations?**

## 08 Validations.md
- 1. **What are the common validation types in Rails?**
- 2. **How does `validates_uniqueness_of` work internally?**
- 3. **What are custom validators?**
- 4. **What does `errors.add` do?**
- 5. **What are conditional validations?**
- 6. **What is the difference between `valid?` and `invalid?`?**
- 7. **How do you skip validations?**
- 8. **What is `validate :method_name` used for?**
- 9. **How to validate associated models?**

## 09 Background Jobs.md
- 1. **What is ActiveJob?**
- 2. **What is Sidekiq?**
- 3. **What is idempotency in jobs?**
- 4. **How do retries work in Sidekiq?**
- 5. **How do you schedule jobs?**
- 6. **When should work go into a background job?**
- 7. **How do you define job queues?**
- 8. **How to prevent duplicate jobs?**
- 9. **What is `perform_now` vs `perform_later`?**
- 10. **What is a job retry dead letter queue?**

## 10 Mailers.md
- 1. **What is ActionMailer?**
- 2. **What is the difference between `deliver_now` and `deliver_later`?**
- 3. **What are mailer previews?**
- 4. **How do you add attachments to emails?**
- 5. **How do you configure email delivery settings?**
- 6. **What is `ApplicationMailer`?**
- 7. **What formats can a mailer render?**
- 8. **How do you localize email content?**

## 11 Security.md
- 1. **What is CSRF?**
- 2. **What is XSS?**
- 3. **What is SQL Injection and how does Rails prevent it?**
- 4. **What are Strong Parameters?**
- 5. **What is Mass Assignment?**
- 6. **How does Rails handle password security?**
- 7. **What is Authentication vs Authorization?**
- 8. **What is Session Hijacking?**
- 9. **What is Parameter Tampering?**
- 10. **What does Rails automatically escape?**

## 12 Performance & Caching.md
- 1. **What is caching in Rails?**
- 2. **What is Fragment Caching?**
- 3. **What is Russian Doll Caching?**
- 4. **What are Cache Keys?**
- 5. **What is a Cache Digest?**
- 6. **What is Counter Cache?**
- 7. **What is Memoization (`||=`)?**
- 8. **How do you avoid N+1 queries?**
- 9. **How do you improve performance in Rails APIs?**
- 10. **When should background jobs be used for performance?**
- 11. **How do you cache queries in Rails?**
- 12. **What is HTTP caching?**

## 13 Views & Frontend.md
- 1. **What is ERB in Rails?**
- 2. **What are partials?**
- 3. **How do you pass locals to partials?**
- 4. **Difference between layouts and partials?**
- 5. **What is `content_for` used for?**
- 6. **What are view helpers?**
- 7. **What is the Asset Pipeline?**
- 8. **What are ViewComponents?**
- 9. **What is Turbo?**
- 10. **What is server-side rendering in Rails?**
- 11. **How to render a collection efficiently?**
- 12. **What is the difference between `render` and `redirect_to` in views?**

## 14 Concerns.md
- 1. **What are concerns in Rails?**
- 2. **Why use concerns?**
- 3. **When NOT to use concerns?**
- 4. **What is the difference between a concern and a module?**
- 5. **How do concerns help with models?**
- 6. **How do concerns help with controllers?**

## 15 ActiveStorage.md
- 1. **What is ActiveStorage?**
- 2. **How do you attach files to models?**
- 3. **How do you upload files from a form?**
- 4. **How do you generate variants (image resizing)?**
- 5. **What are direct uploads?**
- 6. **How do you generate a secure URL for downloads?**
- 7. **How does ActiveStorage store metadata?**
- 8. **How do you purge (delete) an attachment?**
- 9. **How do you check if a model has an attached file?**
- 10. **How do you validate attachment types?**
- 11. **What is the difference between `purge` and `purge_later`?**
- 12. **Why use ActiveStorage instead of CarrierWave/Paperclip?**

<details>
<summary><strong>Optional / advanced</strong>: ActionCable (real-time WebSockets)</summary>

## 16 ActionCable.md
- 1. **What is ActionCable?**
- 2. **How does ActionCable work?**
- 3. **What is a Channel?**
- 4. **What is Broadcasting?**
- 5. **What is `stream_from`?**
- 6. **What is the difference between WebSockets and Polling?**
- 7. **How do you send data from client to server?**
- 8. **How do you authenticate ActionCable connections?**
- 9. **Where does ActionCable run?**
- 10. **Common use cases for ActionCable**

</details>

## 17 Hotwire (Turbo & Stimulus).md
- 1. **What is Hotwire?**
- 2. **What is Turbo Drive?**
- 3. **What are Turbo Frames?**
- 4. **What are Turbo Streams?**
- 5. **What is Stimulus?**
- 6. **How do Turbo Streams + ActionCable work together?**
- 7. **Why choose Hotwire instead of a SPA (React/Vue)?**
- 8. **How do Turbo Streams help with forms?**

## 18 Credentials & Secrets Management.md
- 1. **What are Rails credentials?**
- 2. **How do you edit credentials?**
- 3. **Where is the master key stored?**
- 4. **Difference between credentials and environment variables?**
- 5. **How do you access credentials in Rails?**
- 6. **What are environment-specific credentials?**
- 7. **Should API keys be stored in Git repo?**
- 8. **How does Rails secure credentials internally?**
- 9. **What should NOT be stored in credentials?**
- 10. **How do you handle CI/CD environments?**

<details>
<summary><strong>Optional / advanced</strong>: Rails Engines</summary>

## 19 Rails Engines.md
- 1. **What is a Rails Engine?**
- 2. **What is a Mountable Engine?**
- 3. **What is the difference between a Full Engine and a Mountable Engine?**
- 4. **When should you use a Rails Engine?**
- 5. **When should you NOT use an engine?**
- 6. **How do you create an engine?**
- 7. **How do you add migrations from an engine to the parent app?**
- 8. **How do engines keep namespaces isolated?**
- 9. **How does routing work inside an engine?**
- 10. **Can engines have dependencies?**

</details>

## 20 Error Handling & Monitoring.md
- 1. **How do you handle exceptions globally in Rails?**
- 2. **What is Sentry / Rollbar and why use them?**
- 3. **What are log levels in Rails?**
- 4. **What is Lograge?**
- 5. **What is a Request ID and why is it important?**
- 6. **How do you notify external services (Sentry, Slack) of errors?**
- 7. **What is Application Monitoring (APM)?**
- 8. **How do you track slow queries in real-time?**
- 9. **How do you prevent the app from crashing on unexpected errors?**
- 10. **Why is error monitoring essential for production apps?**

## 21 Testing.md
- 1. **What testing frameworks does Rails use?**
- 2. **What types of tests exist in Rails?**
- 3. **What are fixtures?**
- 4. **What is FactoryBot and why is it better than fixtures?**
- 5. **How do you test model validations?**
- 6. **How do you test controller actions?** (RSpec request specs preferred)
- 7. **What are Request Specs (RSpec)?**
- 8. **How do you test background jobs?**
- 9. **How do you test mailers?**
- 10. **What is Capybara used for?**
- 11. **How do you stub external services?**
- 12. **How do you test exceptions?**
- 13. **How do you test JSON API responses?**
- 14. **How do you test model callbacks?**
- 15. **What is Test Database Transaction Rollback?**

## 22 API Development.md
- 1. **What format do Rails APIs commonly return?**
- 2. **How do you build an API-only Rails app?**
- 3. **What is `render json:` used for?**
- 4. **How do you handle authentication in an API?**
- 5. **What are strong parameters and why needed in APIs?**
- 6. **How to return proper HTTP status codes?**
- 7. **What is CORS and why required?**
- 8. **What are serializers?**
- 9. **How do you version your API?**
- 10. **How do you paginate API results?**
- 11. **How to document APIs?**
- 12. **How do you prevent over-fetching in APIs?**
- 13. **How do you optimize N+1 in API endpoints?**
- 14. **How do you return partial fields?**

## 22 Architecture Patterns.md
- 1. **What is a service object?**
- 2. **What is a form object?**
- 3. **What is a decorator / presenter?**
- 4. **What is a query object?**
- 5. **What is an interactor?**
- 6. **What is a PORO?**
- 7. **Multi-tenancy approaches (high level)**
- 8. **Soft delete strategies**

## 23 Practical Scenario Questions.md
- 1. **How would you implement soft delete in Rails?**
- 2. **How do you design a tagging system?**
- 3. **How would you implement role-based authorization?**
- 4. **How do you implement search efficiently?**
- 5. **How to handle file uploads for many users?**
- 6. **How would you design a notification system?**
- 7. **How to avoid race conditions when updating counters?**
- 8. **How would you implement audit logging?**
- 9. **How to design a multi-tenant app?**
- 10. **How do you prevent duplicate form submissions?**
- 11. **How do you structure a large Rails refactor?**
- 12. **How to safely migrate a large table in production?**
- 13. **How would you design a real-time chat?**
- 14. **How do you handle API rate limiting?**
- 15. **How to paginate a very large dataset?**
- 16. **How do you bulk insert/update records?**
- 17. **How to debug a memory leak in Rails?**
- 18. **How do you test performance bottlenecks?**
- 19. **How would you design an email verification system?**
- 20. **How would you handle large JSON responses?**

## 24 Mongoid.md
- 1. **What is Mongoid?**
- 2. **How do you define fields in Mongoid?**
- 3. **What are Mongoid relations?**
- 4. **What is the difference between `embeds_many` and `has_many`?**
- 5. **How do you query documents in Mongoid?**
- 6. **How do you update documents?**
- 7. **What is the difference between `update`, `set`, and `push`?**
- 8. **How do Mongoid validations work?**
- 9. **Does Mongoid support transactions?**
- 10. **How do Mongoid indexes work?**
- 11. **How do you use scopes in Mongoid?**
- 12. **How do you embed documents?**
- 13. **How does Mongoid handle IDs?**
- 14. **What is `only` and `without` used for in Mongoid?**
- 15. **What is `pluck` in Mongoid?**
- 16. **How do you use atomic updates in Mongoid?**
- 17. **How do you handle relations in Mongoid without N+1?**
- 18. **How do you order queries?**
- 19. **How do you paginate in Mongoid?**
- 20. **How do you perform aggregations in Mongoid?**
- 21. **How do you perform text search?**
- 22. **Does Mongoid support joins?**
- 23. **When should you NOT embed documents?**
- 24. **Pros and Cons of Mongoid vs ActiveRecord?**
- 25. **When is MongoDB a good choice?**
- 26. **What callbacks does Mongoid support?**

## 25 Rails Application Configuration.md
- 1. **Why is ActiveRecord disabled in this Rails app?**
- 2. **Why are custom autoload paths added?**
- 3. **What does `Rack::Deflater` do in middleware?**
- 4. **What does `Mongoid::QueryCache::Middleware` do?**
- 5. **Why set a custom application time zone?**
- 6. **Why configure I18n available locales and default locale?**
- 7. **What does `exceptions_app = self.routes` mean?**
- 8. **Why are CSP headers added?**
- 9. **Why exclude certain paths from SSL redirection?**
- 10. **Why are some Rails frameworks commented out?**
- 11. **What is a Railtie and why is it important?**
- 12. **What does Bundler.require(*Rails.groups) do?**
- 13. **Why prefer `Time.zone.now` / `Time.current` instead of `Time.now`?**
- 14. **What does `config.time_zone` do?**
- 15. **How do you display time in a user’s time zone?**
- 16. **What is `I18n.t`?**
- 17. **Where are translation files stored?**
- 18. **How do you switch locales?**

## Rails Application Configuration — Important Interview Q&A.md
- 1. **Why is ActiveRecord disabled in this Rails app?**
- 2. **Why are custom autoload paths added?**
- 3. **What does `Rack::Deflater` do in middleware?**
- 4. **What does `Mongoid::QueryCache::Middleware` do?**
- 5. **Why set a custom application time zone?**
- 6. **Why configure I18n available locales and default locale?**
- 7. **What does `exceptions_app = self.routes` mean?**
- 8. **Why are CSP headers added?**
- 9. **Why exclude certain paths from SSL redirection?**
- 10. **Why are some Rails frameworks commented out?**
- 11. **What is a Railtie and why is it important?**
- 12. **What does Bundler.require(*Rails.groups) do?**

<details>
<summary><strong>Optional</strong>: Ruby deep-dive</summary>

## Ruby Language Interview Questions & Answers.md
- 1. **What are symbols in Ruby?**
- 2. **What is the difference between a class variable and an instance variable?**
- 3. **What is the difference between `nil?`, `empty?`, and `blank?`?**
- 4. **What is a module in Ruby?**
- 5. **What is the difference between `include`, `extend`, and `prepend`?**
- 6. **What does `self` mean in Ruby?**
- 7. **What is metaprogramming in Ruby?**
- 8. **What is the difference between a block, a proc, and a lambda?**
- 9. **How does Ruby garbage collection work?**
- 10. **Difference between `==`, `===`, and `eql?`**
- 11. **What is a mixin?**
- 12. **What is monkey patching?**
- 3. **What is the difference between `load` and `require`?**
- 14. **What are Ruby iterators?**
- 15. **Explain Ruby exception handling**
- 16. **Difference between symbols and strings**
- 17.0.**Why do some Ruby methods end with `?`?**
- 17. **What are splat (`*`) and double splat (`**`) operators?**
- 18. **What is `method_missing`?**
- 19.  What are `freeze`, `dup`, and `clone`?
- 20. **What is Duck Typing?**
- 21. **Difference between `raise` and `fail`**
- 22. **Difference between public, private, and protected methods**
- 23. **What is an Enumerator?**
- 24. **What is the difference between `map` and `each`?**
- 25. **What are frozen objects? (`freeze`)**
- 26. **What is the spaceship operator `<=>`?**
- 27. **What is the difference between class methods and instance methods?**
- Instance method:
- Class method:
- 28. **How does Ruby handle keywords vs positional arguments?**
- 29. **What is the difference between mutable and immutable objects in Ruby?**
- 30. **What is a singleton method?**
- 31. **Explain Ruby’s method lookup path**
- 32. **What is the difference between `require_relative` and `require`?**
- 33. **What are keyword argument defaults?**
- 34. **What is a refinement in Ruby?**
- 35. **What is the difference between `!=` and `!`?**
- 36. ### **Safe Navigation & Nil Handling in Ruby**

</details>

