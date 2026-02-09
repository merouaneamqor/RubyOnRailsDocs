### 1. **What is a migration in Rails?** (ActiveRecord-specific)
A migration is a Ruby class that modifies the database schema over time:
- create tables  
- add columns  
- add indexes  
- change data types  

They act like version control for your database.

---

### 2. **Difference between `change`, `up`, and `down`?** (ActiveRecord-specific)

```ruby
def change
  add_column :users, :age, :integer
end
```

Rails automatically reverses with `rails db:rollback`.  
If Rails can’t reverse the change, you must use:

```ruby
def up; end
def down; end
```

---

### 3. **What is a reversible migration?** (ActiveRecord-specific)
A migration where Rails knows how to automatically undo the change.

Example:  
`add_column`, `create_table`, `add_index` → reversible.

---

### 4. **What is `schema.rb`?** (ActiveRecord-specific)
A snapshot of your current database structure.  
Rails uses it to rebuild the schema quickly (`db:reset`).

---

### 5. **Difference between `schema.rb` and `structure.sql`?** (ActiveRecord-specific)
- **schema.rb** → DB-agnostic, simplified Ruby representation  
- **structure.sql** → SQL dump including DB-specific features (triggers, views, functions)

Use `structure.sql` when using advanced PostgreSQL features.

---

### 6. **How do you rollback migrations?** (ActiveRecord-specific)

```bash
rails db:rollback
rails db:rollback STEP=3
```

To revert everything:

```bash
rails db:migrate:reset
```

---

### 7. **How do you add indexes?** (Both)

```ruby
add_index :users, :email
add_index :orders, [:user_id, :status]
```

Indexes speed up lookup queries dramatically.

---

### 8. **What is a foreign key constraint?** (ActiveRecord-specific)
It enforces relational integrity at the database level.

```ruby
add_foreign_key :comments, :posts
```

Prevents orphaned records.

---

### 9. **What is `rails db:seed` used for?** (ActiveRecord-specific)
To populate the database with initial or sample data.

```ruby
rails db:seed
```

---

### 10. **What is JSONB (PostgreSQL)?** (ActiveRecord-specific)
A binary JSON type that supports:
- indexing  
- fast queries  
- nested filtering  

Example:

```ruby
add_column :products, :metadata, :jsonb, default: {}
```

---

### 11. **What is PostgreSQL full-text search?** (ActiveRecord-specific)
Allows fast text matching using `tsvector` and `tsquery`.

Rails example:

```ruby
where("to_tsvector('english', title) @@ plainto_tsquery(?)", query)
```

---

### 12. **How do you check slow queries using EXPLAIN?** (Both)

```ruby
User.where(active: true).explain
```

Shows query plan, index usage, table scans, and performance bottlenecks.

---

### 13. **What is the difference between `db:reset` and `db:drop + db:create + db:migrate`?** (ActiveRecord-specific)
`db:reset` is faster and uses schema.rb (or structure.sql).  
It DOES NOT run migrations individually.

---

### 14. **How do you change column types safely?** (ActiveRecord-specific)
Avoid dangerous operations on large tables; instead use:

```ruby
change_column :users, :age, :integer, using: 'age::integer'
```

Or use background migration patterns.

---

### 15. **When should you add database-level validations?** (ActiveRecord-specific)
Critical constraints:
- uniqueness  
- not null  
- foreign keys  

Because Rails validations can be bypassed.

---

