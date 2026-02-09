## ERB & HAML — Basics You MUST Know (Interview Q&A)

---

# PART 1 — ERB (Embedded Ruby)

---

### Q1. **What is ERB?**
ERB lets you write Ruby inside HTML templates.
Rails processes ERB and sends plain HTML to the browser.

---

### Q2. **What is the difference between `<% %>` and `<%= %>`?**

```erb
<% @user.name %>    <!-- runs Ruby, prints NOTHING -->
<%= @user.name %>  <!-- runs Ruby and prints result -->
```

- `<% %>` → logic only
- `<%= %>` → output

This is the **most common interview question**.

---

### Q3. **How does data reach an ERB view?**
Through **instance variables** set in the controller.

Controller:
```ruby
def index
  @users = User.all
end
```

View:
```erb
<%= @users.count %>
```

Only instance variables (`@users`) are available in views.

---

### Q4. **Can a view use variables from another action?**
❌ No.

Each controller action renders **its own view**.
Variables exist only for that request.

---

### Q5. **How do you loop in ERB?**

```erb
<% @users.each do |user| %>
  <p><%= user.name %></p>
<% end %>
```

ERB does not change Ruby syntax.

---

### Q6. **How do you render a partial?**

```erb
<%= render "user", user: user %>
```

Partial file:
```text
app/views/users/_user.html.erb
```

---

### Q7. **How does `<%= render @users %>` work?**

```erb
<%= render @users %>
```

Rails automatically:
- loops over `@users`
- looks for `_user.html.erb`
- assigns each item as `user`

Partial:
```erb
<p><%= user.name %></p>
```

This is the **preferred and cleanest way**.

---

### Q8. **What happens if `_user.html.erb` does not exist?**
Rails raises an error.

Convention matters.

---

### Q9. **How do you write comments in ERB?**

HTML comment (visible):
```erb
<!-- visible in page source -->
```

ERB comment (hidden):
```erb
<%# not rendered at all %>
```

---

### Q10. **Does ERB escape HTML by default?**
✅ Yes (to prevent XSS).

```erb
<%= "<b>Hi</b>" %>
```

Output:
```text
&lt;b&gt;Hi&lt;/b&gt;
```

Unsafe (use carefully):
```erb
<%= "<b>Hi</b>".html_safe %>
```

---

### Q11. **What are helpers in ERB?**
Helpers are Ruby methods used in views.

```erb
<%= number_to_currency(100) %>
<%= time_ago_in_words(Time.now - 1.hour) %>
```

They keep logic out of templates.

---

### Q12. **What does `yield` do in ERB layouts?**

```erb
<%= yield %>
```

It inserts the rendered view into the layout.

---

# PART 2 — HAML (HTML Abstraction Markup Language)

---

### Q13. **What is HAML?**
HAML is an alternative to ERB with:
- no closing tags
- indentation-based structure
- less noise

Same result, different syntax.

---

### Q14. **ERB vs HAML — basic comparison**

| ERB | HAML |
|----|----|
| HTML + Ruby | Ruby-style markup |
| Verbose | Minimal |
| Explicit tags | Indentation-based |
| More common | Cleaner but opinionated |

---

### Q15. **Basic HTML in HAML**

ERB:
```erb
<p>Hello</p>
```

HAML:
```haml
%p Hello
```

---

### Q16. **How do you output Ruby in HAML?**

```haml
%p= user.name
```

- `=` → output
- `-` → logic only

---

### Q17. **How do you write logic in HAML?**

```haml
- if @user.admin?
  %p Admin
```

Equivalent ERB:
```erb
<% if @user.admin? %>
  <p>Admin</p>
<% end %>
```

---

### Q18. **How do you loop in HAML?**

```haml
- @users.each do |user|
  %p= user.name
```

Same Ruby, cleaner syntax.

---

### Q19. **How do you render a partial in HAML?**

```haml
= render "user", user: user
```

Partial:
```text
app/views/users/_user.html.haml
```

Rails chooses `.haml` automatically if it exists.

---

### Q20. **How does `render @users` work in HAML?**

```haml
= render @users
```

Partial `_user.html.haml`:
```haml
%p= user.name
```

Same Rails behavior as ERB.

---

### Q21. **How do you write comments in HAML?**

Rendered comment:
```haml
/ visible comment
```

Hidden comment:
```haml
-# not rendered
```

---

### Q22. **Can ERB and HAML coexist in the same app?**
✅ Yes.

Rails chooses based on file extension:
- `.html.erb`
- `.html.haml`

---

### Q23. **When should you use ERB?**
- team familiarity
- explicit HTML
- easier onboarding
- default Rails choice

---

### Q24. **When should you use HAML?**
- cleaner templates
- less markup noise
- experienced team
- strong conventions

---

## FINAL MENTAL MODEL (MEMORIZE)

- ERB = HTML + Ruby
- HAML = indentation + Ruby
- Controllers pass data via `@variables`
- Views render data
- Partials reuse UI
- `render @collection` is preferred
- Same Rails rules apply to ERB and HAML

---

