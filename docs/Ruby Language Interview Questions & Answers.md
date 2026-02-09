### 1. **What are symbols in Ruby?**

A symbol is a lightweight, immutable identifier that is stored only once in memory.  
Symbols are faster than strings for comparisons and are commonly used as hash keys and method names.

**Example:**

```ruby
:name
"adam".to_sym  # => :adam
```

---

### 2. **What is the difference between a class variable and an instance variable?**

- **Instance variable (`@var`)** → each object stores its **own copy**  
- **Class variable (`@@var`)** → **shared** across all instances of the class  
  Useful for tracking class-wide counters, configuration, or limits.

**Example:**

```ruby
class User
  @@count = 0

  def initialize(name)
    @name = name
    @@count += 1
  end
end
```

---

### 3. **What is the difference between `nil?`, `empty?`, and `blank?`?**

- **`nil?`** → value is literally `nil`  
- **`empty?`** → object exists but has no elements (`""`, `[]`, `{}`)  
- **`blank?`** → Rails method; true for `nil`, empty, or whitespace (`"   "`)

**Example:**

```ruby
nil.nil?        # true
"".empty?       # true
" ".blank?      # true  # Rails
```

---

### 4. **What is a module in Ruby?**

A module is a container for methods, constants, and namespaces.  
Modules cannot be instantiated and are mainly used for:

- **Mixins** (`include`, `extend`)  
- **Organizing code under namespaces**

**Example:**

```ruby
module Loggable
  def log(msg)
    puts "[LOG] #{msg}"
  end
end
```

---

### 5. **What is the difference between `include`, `extend`, and `prepend`?**

- **`include`** → adds module methods as **instance methods**  
- **`extend`** → adds module methods as **class methods**  
- **`prepend`** → adds module methods as **instance methods BUT they override the class’s methods** (module methods run first)

`prepend` changes the method lookup order so the module takes priority over the class.  
This is useful for wrapping or modifying existing behavior without monkey patching.

**Example:**

```ruby
module Greetings
  def hello
    "Hello from module!"
  end
end

class User
  include Greetings
end

class Admin
  extend Greetings
end

user = User.new
user.hello   # => "Hello from module!"
Admin.hello  # => "Hello from module!"
```

---

**Example of `prepend` overriding a class method:**

```ruby
module Logger
  def info
    "Module info"
  end
end

class Service
  prepend Logger

  def info
    "Class info"
  end
end

Service.new.info  # => "Module info"
```

`prepend` inserts the module **before the class** in the method lookup chain.

---

### 6. **What does `self` mean in Ruby?**

`self` refers to the **current object** in context.

- Inside instance methods → the instance  
- Inside class methods / class scope → the class itself  
- Top level → the main object

**Example:**

```ruby
class User
  # Instance method → self = the object
  def full_info
    "My object ID is #{self.object_id}"
  end

  # Class method → self = the class
  def self.find
    "class method called on #{self}"
  end
end

u = User.new
u.full_info     # instance method
User.find       # class method
```

---

### 7. **What is metaprogramming in Ruby?**

Metaprogramming means writing code that **creates or modifies other code** at runtime.  
Ruby provides:

- `define_method`
- `method_missing`
- Reopening classes

This allows Rails DSLs like `belongs_to`, `validates`, etc.

**Example:**

```ruby
class User
  define_method(:greet) { "Hello" }
end
```

---

### 8. **What is the difference between a block, a proc, and a lambda?**

- **Block** → anonymous code passed to methods  
- **Proc** → saved block; flexible arguments  
- **Lambda** → strict arguments; behaves like a method

**Example:**

```ruby
#block 
[1, 2, 3].each do |n|
  puts n
end

#proc
my_proc = Proc.new { |x| puts x }
my_proc.call(1)     # prints 1
my_proc.call(1, 2)  # prints 1 (extra args ignored)

# lambda
lam = ->(x) { x * 2 }
lam.call(5)  # => 10
```

---

### 9. **How does Ruby garbage collection work?**

Ruby uses **Mark and Sweep** GC:

1. Mark reachable objects  
2. Sweep and remove unreachable ones  

**Example:**

```ruby
GC.start
```

---

### 10. **Difference between `==`, `===`, and `eql?`**

- `==` → checks value equality  
- `===` → used in `case` expressions (membership logic)  
- `eql?` → checks value AND type

**Example:**

```ruby
1 == 1.0      # true
1.eql?(1.0)   # false
String === "hello"   # true
(1..5) === 3   # true
```

---

### 11. **What is a mixin?**

A mixin is a module included into a class to share behavior without inheritance.

**Example:**

```ruby
class Bird
  include Flyable
end
```

---

### 12. **What is monkey patching?**

Reopening existing classes to add or modify methods (powerful but dangerous).

**Example:**

```ruby
class String
  def shout
    upcase + "!"
  end
end
```

---

### 3. **What is the difference between `load` and `require`?**

Ruby has two ways to bring external files into your program:

- **`require`** → loads a file **only once** (even if you call it multiple times)  
  Used for loading libraries, gems, or your own classes/modules.

- **`load`** → loads and executes a file **every time** you call it  
  Used when you want a file to be re-read dynamically (e.g., reloading config).

`require` prevents duplicated loading; `load` does not.

**Example:**

```ruby
require_relative "user"   # loads user.rb only once

load "settings.rb"        # reloads settings.rb every time
```


---

### 14. **What are Ruby iterators?**

Iterators are methods that loop over collections (arrays, hashes, ranges) and run a block of code for each element. They return different results depending on the iterator used.

- **`each`** → loops and returns the original collection (used for side effects)  
- **`map`** → transforms values and returns a **new array**  
- **`select`** → keeps elements where the block returns true  
- **`reject`** → removes elements where the block returns true  
- **`reduce`** / **`inject`** → combines all elements into a **single value**  
- **`times`** → runs a block N times  

Ruby supports two block styles:
- `{}` for **inline** one-liners  
- `do...end` for **multi-line** blocks

**Example:**

```ruby
[1, 2, 3].map { |n| n * 2 }
# => [2, 4, 6]
```

---

### 15. **Explain Ruby exception handling**

Ruby handles errors using:

- `begin` → code that may fail  
- `rescue` → code that handles errors  
- `ensure` → code that always runs (cleanup)

You can rescue a specific error type or rescue all `StandardError`s.  
`ensure` runs whether an error occurs or not.

**Example:**

```ruby
begin
  1 / 0
rescue ZeroDivisionError
  puts "Cannot divide by zero"
ensure
  puts "This always runs"
end
```

---

### 16. **Difference between symbols and strings**

| Symbols | Strings |
|--------|---------|
| Immutable | Mutable |
| Stored once | Many copies |
| Fast comparisons | Slower |
| Used as identifiers | Used for data |

**Example:**

```ruby
:name.object_id == :name.object_id # => true
```

---
### 17.0.**Why do some Ruby methods end with `?`?**

Ruby uses `?` at the end of method names as a convention for **boolean methods**  
(methods that return `true` or `false`).

Examples:
- `empty?`
- `nil?`
- `even?`
- `odd?`
- `zero?`
- `valid?`
- `include?`
- `has_key?`

It makes Ruby code read naturally:

```ruby
if user.active?
  ...
end
```

**Important:**
You only use `?` if the method name *actually ends* with `?`.

```ruby
user.name?   # ❌ error (method doesn't exist)
user.name    # ✔ correct
```

Ruby naming conventions:

- **`?` methods** → return boolean  
- **`!` methods** → dangerous / destructive versions   (like map! it modifies array that used it)
- **`=` methods** → setters (e.g., `user.name = "Adam"`)

---
### 17. **What are splat (`*`) and double splat (`**`) operators?**

Ruby uses splat operators to handle flexible numbers of arguments.

- `*args` collects **positional arguments** into an array  
- `**kwargs` collects **keyword arguments** into a hash  
- They can also unpack arrays/hashes when calling a method

**Examples:**

```ruby
def sum(*nums)
  nums.sum
end

sum(1, 2, 3)   # => 6
```

```ruby
def print_user(**info)
  p info
end

print_user(name: "Adam", age: 25)
# => {:name=>"Adam", :age=>25}
```

---

### 18. **What is `method_missing`?**

Called when invoking an undefined method.

every Ruby object inherits `method_missing` automatically.
Here’s why:
All Ruby classes eventually inherit from:
`BasicObject → Kernel → Object → YourClass`
`Kernel` defines a default `method_missing`.
So even if you never define it yourself

**Example:**

```ruby
def method_missing(name, *args)
  "Called #{name}"
end
```

---

### 19.  What are `freeze`, `dup`, and `clone`?

- **`freeze`** → makes an object read-only (cannot be modified)
- **`dup`** → makes a shallow copy you can modify safely
- **`clone`** → like `dup` but also copies frozen state and singleton methods (rarely used)

**Examples:**

```ruby
arr = [1, 2, 3].freeze
arr << 4       # ❌ error

copy = arr.dup
copy << 4      # ✔ works
```

---

### 20. **What is Duck Typing?**

Duck typing means Ruby checks an object's **behavior** (whether it responds to a method), not its **class type**.  
If an object can perform the needed action, Ruby accepts it — even if the class is different.

This is very different from languages like Java or C#, where the type must match exactly.

**Example:**

```ruby
class Dog
  def sound
    "Woof"
  end
end

class Car
  def sound
    "Vroom"
  end
end

def make_sound(obj)
  obj.sound   # Ruby only cares that "sound" exists
end
```

Both `Dog` and `Car` work because they respond to `sound`, regardless of class.

---

### 21. **Difference between `raise` and `fail`**

In Ruby, **`raise` and `fail` do the same thing** — they both trigger an exception.  
The difference is **semantic**, not functional.

- **`raise`** → used when you *create* or *throw* a new error  
- **`fail`** → preferred when you are *inside a rescue block* and want to **rethrow** the error

So Ruby developers use:

- `raise` → starting an exception
- `fail` → continuing an exception

**Example (starting an error):**

```ruby
raise "Something went wrong"
```

**Example (rethrowing inside rescue):**

```ruby
begin
  1 / 0
rescue => e
  puts "Handling error..."
  fail  # re-raises the same exception
end
```

`fail` here makes your intention clear: the method **failed**, and you're letting the error continue.


---

### 22. **Difference between public, private, and protected methods**

- **public** → accessible anywhere  
- **private** → no explicit receiver  
- **protected** → accessible by objects of same class

**Example:**

```ruby
private def secret; end
```

Private = “only inside the class, no receiver allowed”  
Protected = “inside the class, receiver allowed if same class”

---

### 23. **What is an Enumerator?**

An Enumerator is an object that represents an iteration when no block is given.  
It allows controlled or lazy iteration.

**Example:**

```ruby
enum = [1, 2, 3].each
enum.next  # => 1
enum.next  # => 2
```

---

### 24. **What is the difference between `map` and `each`?**

- `each` → iterates but returns original array  
- `map` → returns a transformed array

**Example:**

```ruby
[1,2,3].map { |n| n * 2 }
```

---

### 25. **What are frozen objects? (`freeze`)**

Frozen objects cannot be modified.

**Example:**

```ruby
str = "hello".freeze
```
Freezing an object does NOT freeze the variable.

```ruby
str = "hello".freeze
str = "ddd"   # OK, because we assign a new object
```

`freeze` only prevents modification of the frozen object itself, not reassignment of the variable.

---
### 26. **What is the spaceship operator `<=>`?**

The `<=>` operator compares two values and returns:

- **-1** → left is smaller  
- **0** → equal  
- **1** → left is greater  

Used mainly in sorting.

**Example:**

```ruby
5 <=> 10   # => -1
10 <=> 10  # => 0
100 <=> 10 # => 1
```

---
### 27. **What is the difference between class methods and instance methods?**

Ruby has **two kinds of methods**, depending on *who* the method belongs to:

#### ✅ **Instance methods → behavior for individual objects**

These methods require you to **create an instance** of the class.

They describe actions *a single object* can perform.

```ruby
class User
  def greet
    "Hello, I'm a user"
  end
end

u = User.new
u.greet   # => "Hello, I'm a user"
```

Use instance methods when the method depends on the object's data  
(e.g., `name`, `age`, `email`, etc.).

---

#### 🟦 **Class methods → behavior on the class itself**

These do **NOT** require an instance.  
They belong to the **class**, not to individual objects.

```ruby
class User
  def self.find(id)
    "Finding user #{id}"
  end
end

User.find(1)  # => "Finding user 1"
```

Use class methods for:

- finders (`User.find`)
- builders (`User.create`)
- utility methods (`User.authenticate`)
- configuration

Rails uses class methods **everywhere** in ActiveRecord:

- `User.all`
- `User.where`
- `User.find_by`
- `User.count`

---

#### 🧠 Why the difference?

### Instance method:
> “I need information stored *inside this object*.”

### Class method:
> “I need functionality related to the whole class, not a specific object.”

---

## 📝 Summary

- **Instance methods (`def method`)**  
  → must call on an object  
  → used for per-object behavior

- **Class methods (`def self.method`)**  
  → called on the class  
  → used for global/class-level behavior

---

### 28. **How does Ruby handle keywords vs positional arguments?**

Ruby supports **positional** and **keyword** arguments.

#### 1. Positional arguments
Order matters.

```ruby
def greet(name, age)
  "#{name} is #{age}"
end

greet("Adam", 25)
```

#### 2. Keyword arguments
Names matter, not order.

```ruby
def greet(name:, age:)
  "#{name} is #{age}"
end

greet(age: 25, name: "Adam")
```

Optional defaults:

```ruby
def register(name:, role: "user"); end
```

#### 3. Mixed arguments
Positional first, then keyword.

```ruby
def info(id, name:, active:); end
info(10, name: "Adam", active: true)
```

#### **Summary**
- Positional → order matters  
- Keyword → names matter, safer, more readable  


---

### 29. **What is the difference between mutable and immutable objects in Ruby?**

- **Mutable objects** → can be changed  
  Examples: **String, Array, Hash**

```ruby
str = "hi"
str << "!"   # => "hi!"
```

- **Immutable objects** → cannot be modified  
  Examples: **Integer, Float, Symbol, true/false/nil, frozen objects**

```ruby
n = 5
n += 1       # creates a NEW Integer (6)
```

- Mutating methods (`<<`, `upcase!`, `push`) only work on mutable objects  
- Immutable objects always create new copies when "changed"


---

### 30. **What is a singleton method?**

A **singleton method** is a method defined on **one specific object**, not on the entire class.  
Only that one object can call the method.

**Example:**

```ruby
obj = Object.new

def obj.say_hi
  "hi"
end

obj.say_hi        # => "hi"
Object.new.say_hi # ❌ NoMethodError
```

#### 🧠 Why use singleton methods?

- To give **one object** special behavior  
- Used internally by Ruby to create **class methods**  
  (class methods are singleton methods on the class object)

**Example (class method is a singleton method):**

```ruby
class User
  def self.find(id)
    # ...
  end
end
```

#### 📝 Summary

- Singleton method = method for **one** object  
- Other objects of the same class **cannot** use it  
- Ruby uses this technique to define **class methods**

---

### 31. **Explain Ruby’s method lookup path**

When calling a method, Ruby searches in this order:

1. Singleton class (methods defined on the single object)
2. Class
3. Included modules (last included first)
4. Superclass
5. Kernel
6. BasicObject

Ruby stops as soon as it finds the method.

**Example:**

```ruby
def obj.greet; "singleton"; end
obj.greet  # found in singleton class
```


---

### 32. **What is the difference between `require_relative` and `require`?**

- `require` → load from load path  
- `require_relative` → load relative to file

**Example:**

```ruby
require_relative "user"
```

---

### 33. **What are keyword argument defaults?**

You can define default values for keyword arguments.

**Example:**

```ruby
def book(title:, pages: 100); end
```

---

### 34. **What is a refinement in Ruby?**

Refinements allow you to modify classes **only inside a specific file or scope**, making them a safer alternative to monkey patching.

You must activate them with `using`.

**Example:**

```ruby
module MyRefinement
  refine String do
    def shout
      upcase + "!"
    end
  end
end

using MyRefinement

"hi".shout   # => "HI!"
```

Outside this scope, `"hi".shout` does not exist.


---

### 35. **What is the difference between `!=` and `!`?**

- `!=` → inequality  
- `!` → boolean negation  

**Example:**

```ruby
!true   # false
```

---
### 36. ### **Safe Navigation & Nil Handling in Ruby**

Ruby (and Rails) provide several tools to safely handle `nil` without raising:

`NoMethodError: undefined method 'x' for nil:NilClass`

Below are all related features together in one place.

---

#### 1. Safe Navigation Operator (`&.`) — Ruby
Calls a method **only if the object is not nil**.  
If the object is nil → returns `nil`.

```ruby
user&.email
invoice.user&.profile&.email
```

Prevents crashes when `user` is nil.

---

#### 2. `.try` — Rails only
Older Rails helper similar to `&.`.

```ruby
user.try(:email)
user.try(:update, name: "Adam")   # works with arguments
```

Not part of Ruby.  
Prefer `&.` unless you need argument support.

---

#### **3. `dig`** — Ruby (Hash / Array)
Safely extracts nested values from Hashes/Arrays.  
Returns `nil` if any key/index is missing.

**Hash example:**
```ruby
data = { user: { profile: { email: "a@b.com" } } }

data.dig(:user, :profile, :email)   # "a@b.com"
data.dig(:user, :settings, :theme)  # nil
```

**Array example:**
```ruby
arr = [[1, 2], [3, [4, 5]]]

arr.dig(1, 1, 0)   # 4
arr.dig(2, 0)      # nil
```

**Mixed structure:**
```ruby
data = { users: [{ name: "Adam" }] }

data.dig(:users, 0, :name)   # "Adam"
```

---

#### **4. Nil guard (`return unless user`)** — Ruby
Stops execution early if value is nil.

```ruby
def greet(user)
  return unless user
  puts "Hello #{user.name}"
end
```

Good for multi-line logic where `&.` isn't enough.

---

#### **Summary**
- `&.` → Ruby’s safe method call  
- `.try` → Rails version of safe call  
- `dig` → Safely access nested data  
- `return unless user` → Exit early if nil  

All prevent nil-related crashes in different scenarios.
```
