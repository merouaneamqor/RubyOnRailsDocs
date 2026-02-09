### 1. **What is a service object?**
A plain Ruby class that holds business logic that doesn’t belong in a model or controller.

**Goal:** keep controllers/models small.

Example idea:
- `CreateOrder.call(user:, params:)`

---

### 2. **What is a form object?**
A plain Ruby object that handles form logic (validation + saving), especially when one form updates multiple models.

**Use it when:** the form doesn’t map cleanly to a single model.

---

### 3. **What is a decorator / presenter?**
An object that adds presentation logic (formatting) without polluting the model.

**Example use cases:**
- `full_name`
- formatted money/date strings

---

### 4. **What is a query object?**
A plain Ruby class that encapsulates a complex query.

**Benefit:** keeps controllers/models clean and makes queries reusable.

---

### 5. **What is an interactor?**
A pattern for “one business action” that returns a success/failure result.

**Think:** command object with a clear outcome (`success?`, `errors`).

---

### 6. **What is a PORO?**
PORO = Plain Old Ruby Object.

It’s any Ruby class not tightly coupled to Rails (no ActiveRecord inheritance required).

---

### 7. **Multi-tenancy approaches (high level)**
Common approaches:
- **Row-based**: add `account_id` to tables and always scope queries.
- **Subdomain-based**: tenant from `tenant1.example.com`.
- **Schema/database per tenant**: heavier, used less often.

---

### 8. **Soft delete strategies**
Common ways:
- **`deleted_at` timestamp** + scope to hide deleted rows
- **boolean flag** like `archived: true`
- **Gem** like Paranoia (wraps the `deleted_at` pattern)

####  `deleted_at` timestamp
- Add `deleted_at` column
- Set it when deleting
- Use a scope to hide deleted records

Most common approach.