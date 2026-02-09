### 1. **What is ActiveJob?**

**ActiveJob** is Rails’ **abstraction layer** for background jobs.

It does **not execute jobs itself**.  
Instead, it provides a **standard API** that lets Rails delegate background work to different job systems (called *adapters*).

With ActiveJob, you:
- write jobs **once**
- keep Rails-style conventions
- can switch job backends without rewriting job code

Example:

```ruby
class SendEmailJob < ApplicationJob
  def perform(user)
    UserMailer.welcome_email(user).deliver_now
  end
end
```

Enqueue the job:

```ruby
SendEmailJob.perform_later(user)
```

What happens:
- Rails enqueues the job
- ActiveJob hands it off to the configured backend
- A background worker executes `perform`

ActiveJob is **about standardization**, not performance.

---

### 2. **What is Sidekiq?**

**Sidekiq** is a **background job processor**, not an abstraction.

It:
- runs as a **separate Ruby process**
- uses **Redis** to store jobs ⚠️⛔🆘⚠️ if sidekik is down jobs **can be lost**
- executes jobs **concurrently**
- handles retries, failures, scheduling, and monitoring

Benefits:
- high concurrency (threads)
- automatic retries
- scheduled jobs
- Web UI dashboard
- production-grade performance

Sidekiq can be used in **two ways**:

#### A) As an ActiveJob backend
Rails → ActiveJob → Sidekiq

```ruby
config.active_job.queue_adapter = :sidekiq
```

You enqueue jobs with:

```ruby
SomeJob.perform_later(...)
```

#### B) Directly (no ActiveJob)
Rails → Sidekiq directly

```ruby
class SomeWorker
  include Sidekiq::Worker
end
```

You enqueue jobs with:

```ruby
SomeWorker.perform_async(...)
```

This approach gives **full Sidekiq control** and bypasses ActiveJob entirely.

---

### Key Difference (Exam-Safe Summary)

- **ActiveJob** = Rails interface (how jobs are defined)
- **Sidekiq** = Execution engine (how jobs are run)
- You can use **Sidekiq with or without ActiveJob**
- In direct Sidekiq usage, **ActiveJob is not involved at all**

---

### 3. **What is idempotency in jobs?**
A job is **idempotent** if running it multiple times does NOT cause unwanted duplicates.

Example problem:
- Email sent twice  
- Payment charged twice  

Solution:
- Locking  
- Checking status before execution  
- Unique job strategies  

---

### 4. **How do retries work in Sidekiq?**
Sidekiq automatically retries failed jobs with **exponential backoff**.

You can customize:

```ruby
sidekiq_options retry: 5
```

---

### 5. **How do you schedule jobs?**
Using `perform_later` schedules it immediately (asynchronously):

```ruby
SendEmailJob.perform_later(user)
```

Or with gem `sidekiq-scheduler`:

```yaml
send_daily_report:
  cron: "0 7 * * *"
  class: "DailyReportJob"
```

---

### 6. **When should work go into a background job?**
Whenever an operation is:
- slow  
- external (email, API call)  
- CPU-heavy  
- triggers many DB writes  

Examples:
✓ Sending emails  
✓ Generating PDFs  
✓ Sending WhatsApp/SMS  
✓ Triggering webhooks  
✓ Image processing  

---

### 7. **How do you define job queues?** (Sidekiq)

In this project, jobs use **Sidekiq directly**, so queues are defined on the worker.

```ruby
class SyncJob
  include Sidekiq::Worker
  sidekiq_options queue: 'critical'

  def perform(*args)
    # work
  end
end
```

This means:
- the job goes into the **`critical`** queue
- higher priority than `default` or `low
#### Queue priority

Queue priority is set when Sidekiq starts.

Example:

```bash
bundle exec sidekiq -q critical -q default -q low
```

Sidekiq will always try:
```
critical → default → low
```
#### Default behavior

If you don’t set a queue:

```ruby
include Sidekiq::Worker
```

Sidekiq uses the **`default`** queue.
#### One-line summary

> In Sidekiq, queues are defined with `sidekiq_options queue: 'name'`, and priority depends on the order Sidekiq processes queues.

---

### 8. **How to prevent duplicate jobs?**
Use a uniqueness gem like `sidekiq-unique-jobs`.

Example:

```ruby
sidekiq_options unique: :until_executed
```

---

### 9. **What is `perform_now` vs `perform_later`?**
| Method          | System    | How it runs                           |
| --------------- | --------- | ------------------------------------- |
| `perform_now`   | ActiveJob | Runs immediately (sync, same thread)  |
| `perform_later` | ActiveJob | Runs in background (async)            |
| `perform_async` | Sidekiq   | Runs in background ASAP               |
| `perform_in`    | Sidekiq   | Runs in background after a delay      |
| `perform_at`    | Sidekiq   | Runs in background at a specific time |

---

### 10. **What is a job retry dead letter queue?**
If a job keeps failing after max retries, Sidekiq moves it to the **dead jobs queue** for manual inspection.

---

