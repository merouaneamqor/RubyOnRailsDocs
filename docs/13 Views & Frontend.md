## 13. Views & Frontend (Rails)

---

### 1. **What is ERB in Rails?**
ERB (Embedded Ruby) allows Ruby code inside HTML templates.

```erb
<p>Hello <%= @user.name %></p>
```

- `<% %>` → executes Ruby (no output)
- `<%= %>` → executes Ruby and outputs the result

ERB is how Rails mixes HTML with Ruby logic.

---

### 2. **What are partials?**
Partials are reusable fragments of views.

Instead of repeating HTML, you extract it into a partial.

```erb
<%= render "user", user: @user %>
```

Naming convention:
- File name starts with `_`
- Stored inside the views folder

Path example:
```text
app/views/users/_user.html.erb
```

---

### 3. **How do you pass locals to partials?**

Long form:
```erb
<%= render partial: "user", locals: { user: @user } %>
```

Short form (preferred):
```erb
<%= render "user", user: @user %>
```

Inside the partial, `user` is available as a local variable.

---

### 4. **Difference between layouts and partials?**

| Type | Purpose |
|----|----|
| Layout | Wraps the entire page (HTML skeleton) |
| Partial | Small reusable view fragment |

- Layouts define the page structure
- Partials reuse pieces of HTML

---

### 5. **What is `yield` in Rails?**

**yield in view can be used render content coming from child view from content_for and inserts it into layout** 

`yield` is a Ruby keyword used to execute a block.

In Rails layouts:
- `yield` inserts the main view
- `yield :name` inserts named content

Example layout:
```erb
<html>
  <body>
    <%= yield %>
  </body>
</html>
```

Rails renders the view first, then inserts it where `yield` appears.

---

### 6. **What is `content_for` used for?**
`content_for` lets a view send optional content to a layout.

Layout defines slots:
```erb
<head>
  <%= yield :head %>
</head>
```

View stores content:
```erb
<% content_for :head do %>
  <script src="page.js"></script>
<% end %>
```

Rails:
- renders the view
- stores the content
- injects it into the layout at `yield :head`

Used for:
- page-specific JS
- page-specific CSS
- meta tags

---

### 7. **Do views render before layouts?**
Conceptually, yes.

Rails flow:
1. Render the view
2. Capture `content_for`
3. Render the layout
4. Insert the view at `yield`
5. Insert named content at `yield :name`

This is why `content_for` works.

---

### 8. **How does Rails choose which layout to use?**
Rails selects layouts in this order:

1. Explicit `layout` in controller
2. Layout defined in `ApplicationController`
3. Controller-specific layout
4. Default fallback: `application.html.*`

Example:
```ruby
class PublicController < ApplicationController
  layout "application-public"
end
```

The browser does NOT choose layouts.  
Layouts are decided entirely on the server.

---

### 9. **What are view helpers?**
View helpers are Ruby methods used ONLY in views.

They are meant for:
- formatting
- display logic
- presentation helpers

Examples:
```ruby
number_to_currency(100)
time_ago_in_words(5.minutes.ago)
```

Defined in:
```text
app/helpers/
```

---

### 10. **How are helpers connected to views?**
Rails automatically includes helpers into views.

Rules:
- `ApplicationHelper` → available in all views
- `UsersHelper` → available in UsersController views

Example:
```ruby
module ApplicationHelper
  def format_price(amount)
    number_to_currency(amount, unit: "MAD")
  end
end
```

Helpers are NOT available in controllers by default.

Helpers should NOT contain:
- database queries
- business logic
- state changes

---

### 11. **What is the Asset Pipeline?**
The Asset Pipeline manages:
- CSS
- JavaScript
- Images

It provides:
- bundling
- minification
- fingerprinting (cache busting)

Assets are loaded only if:
- allowed in `manifest.js`
- required in an entry file
- linked in a layout

---

### 12. **What is `manifest.js`?**
`manifest.js` is an allowlist.

It tells Rails:
- which assets are allowed to be compiled
- which assets can be served in production

Example:
```javascript
//= link_tree ../images
//= link_directory ../stylesheets .css
//= link_directory ../javascripts .js
```

It does NOT load assets.  
It only allows them.

---

### 13. **What are `application.css` and `application.js`?**
They are ENTRY POINTS (bundles).

Their job:
- require other CSS / JS files
- define what goes into the final bundle

Example:
```css
/*
 *= require header
 *= require footer
 *= require assistant/booking_invoices
 */
```

Files are NOT loaded just because they exist.
They must be:
- required
- bundled
- linked in a layout

---

### 14. **How to load page-specific CSS or JS?**

Layout:
```erb
<%= yield :assets %>
```

View:
```erb
<% content_for :assets do %>
  <%= javascript_include_tag "assistant/booking_invoices" %>
  <%= stylesheet_link_tag "assistant/booking_invoices" %>
<% end %>
```

Only this page loads those assets.

---

### 15. **What are ViewComponents?**
ViewComponents are Ruby objects representing UI components.

Example:
```ruby
class ButtonComponent < ViewComponent::Base
  def initialize(text:)
    @text = text
  end
end
```

Benefits:
- testable
- encapsulated
- reusable
- object-oriented

---

### 16. **What is Turbo?**
Turbo is part of Hotwire.

It:
- sends HTML instead of JSON
- updates the DOM without full reloads
- reduces custom JavaScript

Turbo keeps Rails server-driven while feeling like an SPA.

---

### 17. **What is server-side rendering in Rails?**
👉 **Rails builds the HTML on the server and sends it to the browser ready to display.**
Rails renders HTML on the server.

Benefits:
- better SEO
- faster first load
- simpler debugging
### SPA / client-side rendering
- API returns JSON
- JS receives JSON
- JS transforms JSON
- JS builds HTML
Bug could be in:
- backend API
- frontend state
- frontend rendering logic
- async timing
- race conditions
More layers → harder debugging.

Turbo enhances SSR with partial updates.

---

### 18. **How to render a collection efficiently?**
```erb
<%= render @users %>
```

Rails:
- loops automatically
- renders `_user.html.erb`
- assigns each item as `user`

Cleaner and faster than manual loops.

---

### 19. **Difference between `render` and `redirect_to`?**

| Method | Purpose |
|----|----|
| render | Outputs HTML |
| redirect_to | Sends browser to another URL |

Rules:
- `render` → views
- `redirect_to` → controllers only
- Never redirect inside a view

---







