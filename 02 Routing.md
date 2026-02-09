### 1. **What does `resources` generate?**
It generates **7 RESTful routes**:  
index, show, new, edit, create, update, destroy.

```ruby
resources :users
```

---

### 2. **Difference between `resource` and `resources`?**
**`resources` (plural)**  
Used for collections with many records.  
Generates all 7 RESTful routes and uses `:id` in URLs.

Examples:  
- `/users`  
- `/users/1`  
- `/users/1/edit`

**`resource` (singular)**  
Used when there is only one instance per user or per app (profile, settings, cart).  
Does NOT generate an index route and does NOT use `:id` in URLs.

Examples:  
- `/profile`  
- `/profile/edit`

Rails loads the correct record using `current_user`, so an ID is not needed.

---

### 3. **What are nested routes?**
Routes inside another resource—used when a model belongs to another.

```ruby
resources :posts do
  resources :comments
end
```

Bad when nesting more than 1 level (becomes too deep).

---

### 4. **What is a `member` route?**
A `member` route is a custom action that applies to **one specific record**.  
The URL always includes an `:id` because it refers to a single item.

Example:
```ruby
resources :users do
  member do
    get :profile
  end
end
```

Generates routes like:
- `/users/:id/profile`

---

### 5. **What is a `collection` route?**
A `collection` route defines a custom action for the **entire resource**, not a single record.  
Because it doesn't refer to one item, the URL does **not** include an `:id`.

Example:
```ruby
resources :users do
  collection do
    get :search
  end
end
```

Generated route:
- `/users/search`

---

### 6. **What are route concerns?**
Route concerns allow you to define reusable routing blocks and share them across multiple resources.  
They are used when different models need the same nested routes.

Example:
```ruby
concern :commentable do
  resources :comments
end

resources :posts,  concerns: :commentable
resources :photos, concerns: :commentable
```

This generates:
- `/posts/:id/comments`
- `/photos/:id/comments`

The concern keeps the routing DRY and avoids repeating the same nested routes.

---

### 7. **How to create a custom route?**

```ruby
get "login", to: "sessions#new"
post "logout", to: "sessions#destroy"
```

---

### 8. **What happens if a route exists but the controller action does not?**
Rails raises:

```
AbstractController::ActionNotFound
```

Resulting in a **500 error**.

---

### 9. **How to list all routes?**

```bash
rails routes
rails routes -g users
```

`-g` filters by keyword (useful for debugging).

---

### 10. **Difference between `scope`, `namespace`, and `controller` routing blocks**

#### **`scope`**
- Changes **URL only**  
- Does **NOT** change folder structure  
- Controller stays in `app/controllers/`

Example:
```ruby
scope :auth do
  get "login",  to: "sessions#new"
end
```
URL: `/auth/login`  
Controller file:
```
app/controllers/sessions_controller.rb
```

#### **`namespace`**
- Changes **URL**, **folder**, and **module**  
- Controller must be inside a namespaced folder

Example:
```ruby
namespace :auth do
  get "login", to: "sessions#new"
end
```

Controller location:
```
app/controllers/auth/sessions_controller.rb
```

Controller class:
```ruby
module Auth
  class SessionsController < ApplicationController; end
end
```

URL: `/auth/login`

#### **`controller` block**
- Shorter syntax  
- Does not change folder or namespace

Example:
```ruby
controller :sessions do
  get "login", action: :new
end
```

Controller stays in:
```
app/controllers/sessions_controller.rb
```

URL: `/login`


### 11. **What is `root` and how do you define it?**
Defines the homepage of the application (`/`).

```ruby
root "posts#index"
```

---

### 12. **What are route constraints?**
Used to restrict routes using conditions (regex, subdomain, format, custom classes).

```ruby
get "photos/:id", to: "photos#show", constraints: { id: /\d+/ }
```

Only matches if `:id` is numeric.

---

### 13. **What is the difference between `match` and specific HTTP verbs?**
`match` allows a route to respond to **multiple HTTP methods**.

```ruby
match "posts/:id", to: "posts#show", via: [:get, :post]
```

This is usually **not recommended** because it breaks REST conventions and can accidentally expose actions to unsafe HTTP verbs.

**Preferred approach: use explicit verbs**

```ruby
get  "posts/:id", to: "posts#show"
post "posts/:id", to: "posts#create"
```

Specific verbs make routing **clearer, safer, and more maintainable**.


---

### 14. **What are shallow nested routes?**
Shallow routes avoid overly long URLs by nesting only the routes that need the parent ID.

```ruby
resources :posts do
  resources :comments, shallow: true
end
```

**Nested (needs post_id):**
```
/posts/:post_id/comments      # index, create
```

**Shallow (only needs comment id):**
```
/comments/:id                 # show, edit, update, destroy
```

This avoids deep URLs like:
```
/posts/5/comments/10/edit
```

Shallow routing = cleaner, shorter URLs.

---

### 15. **How do you skip or limit RESTful routes?**
Limit routes with `only` or `except`.

```ruby
resources :users, only: [:index, :show]
resources :users, except: [:destroy]
```

---

### 16. **What are route helpers and how do you use them?**

`resources :posts` generates helpers:

```
posts_path       # /posts       (relative)
posts_url        # http://example.com/posts   (absolute)
```

Use:

- `_path` → inside the app (links, redirects)
- `_url` → emails, external redirects, API callbacks


---

### 17. **Difference between redirects in routes vs controllers**

**Route redirect (static):**
```ruby
get "/old", to: redirect("/new")
```
Always the same redirect. No logic.

**Controller redirect (dynamic):**
```ruby
redirect_to dashboard_path
```
Can use params, current_user, conditions, etc.

---

### 18. **How to handle different formats (HTML, JSON)?**

Rails chooses the template based on the request format:

- `/posts/5` → HTML (`show.html.erb`)
- `/posts/5.json` → JSON (`show.json.jbuilder`)

You can also control formats with `respond_to`:

```ruby
def show
  @post = Post.find(params[:id])

  respond_to do |format|
    format.html                # renders show.html.erb
    format.json { render json: @post }
  end
end
```

Use `defaults: { format: :json }` when your endpoints should **always** return JSON (API mode):

```ruby
resources :posts, defaults: { format: :json }
```

This makes `/posts` return JSON even without `.json` in the URL.


---

### 19. **What is `direct` for custom URL helpers?**

`direct` creates custom URL helpers without needing a route or controller.

```ruby
direct :homepage do
  "https://example.com"
end

homepage_url   # => https://example.com
```

Useful for external links or generating special URLs.


---

### 20. **What happens if two routes match the same URL?**
Rails uses the **first matching route**.  
Order matters.

```ruby
get "posts/:id", to: "posts#show"
get "posts/recent", to: "posts#recent"   # never matched
```

Correct order:

```ruby
get "posts/recent", to: "posts#recent"
get "posts/:id",     to: "posts#show"
```

Always place **specific routes above dynamic ones**.

---
### 21 How to create routes with params

**Basic param:**
```ruby
get "posts/:id", to: "posts#show"
```
`params[:id]`

**Multiple params:**
```ruby
get "users/:user_id/posts/:id", to: "posts#show"
```
`params[:user_id]`, `params[:id]`

**Custom param name:**
```ruby
get "products/:slug", to: "products#show"
```
`params[:slug]`

**Optional param:**
```ruby
get "search(/:query)", to: "search#index"
```
`params[:query]`

**Query params (automatic):**
`/posts?page=2` → `params[:page]`

**Constraints:**
```ruby
get "posts/:id", constraints: { id: /\d+/ }
```

**Route helper:**
```ruby
post_path(10)  # "/posts/10"
```
---
### 22. **What are glob routes and when would you use them?**
Glob routes capture multiple URL segments into a single parameter.

```ruby
get "pages/*path", to: "pages#show"
```

Examples:
- `/pages/about` → `params[:path] = "about"`
- `/pages/docs/api/v1` → `params[:path] = "docs/api/v1"`

Used for:
- CMS pages  
- Documentation sites  
- Catch-all routing
