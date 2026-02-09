### 1. **What is a Rails Engine?**
A Rails engine is a **mini Rails application** that can be mounted inside another Rails app.

Engines can contain:
- models  
- controllers  
- views  
- routes  
- migrations  

Engines allow you to build modular, reusable components.

Examples:
- Devise  
- Sidekiq Web UI  
- Spree (ecommerce platform)

---

### 2. **What is a Mountable Engine?**
A type of engine that:
- has isolated namespace  
- includes its own routes, assets, controllers  

Routes are mounted inside the host app:

```ruby
mount Blog::Engine, at: "/blog"
```

Path structure:

```
/blog/posts
/blog/comments
```

---

### 3. **What is the difference between a Full Engine and a Mountable Engine?**

| Type | Description |
|------|-------------|
| **Full Engine** | Acts like a full Rails app (e.g., CMS) |
| **Mountable Engine** | Has isolated namespace; embedded into host app |

Mountable engines are more common.

---

### 4. **When should you use a Rails Engine?**
When you need:
- a reusable feature across multiple apps  
- plugin-like modular structure  
- an isolated domain (e.g., analytics dashboard)  
- internal packages within a large monolith  

Good use cases:
- Admin dashboard  
- Chat system  
- Payment service  
- Survey module  
- Feature to be shared between internal projects  

---

### 5. **When should you NOT use an engine?**
Avoid engines when:
- the code is small and simple  
- you don’t reuse the component in multiple apps  
- the app is already complicated  
- the separation hides logic unnecessarily  

Service objects or concerns are often simpler alternatives.

---

### 6. **How do you create an engine?**

```bash
rails plugin new blog --mountable
```

This generates:
```
blog/app/
blog/lib/blog/engine.rb
blog/config/routes.rb
```

---

### 7. **How do you add migrations from an engine to the parent app?**

```bash
rails blog:install:migrations
```

This copies engine migrations into the host app.

---

### 8. **How do engines keep namespaces isolated?**
In `lib/myengine/engine.rb`:

```ruby
module Myengine
  class Engine < ::Rails::Engine
    isolate_namespace Myengine
  end
end
```

This prevents naming conflicts with the main app.

---

### 9. **How does routing work inside an engine?**

Engine:

```ruby
Myengine::Engine.routes.draw do
  resources :posts
end
```

Host app:

```ruby
mount Myengine::Engine, at: "/blog"
```

---

### 10. **Can engines have dependencies?**
Yes — they can declare dependencies in their `.gemspec`.

For example:

```ruby
spec.add_dependency "devise"
spec.add_dependency "cancancan"
```

---

