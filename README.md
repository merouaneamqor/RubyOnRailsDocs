# Ruby on Rails — Documentation & Interview Prep

> Structured reference and Q&A for Rails core, patterns, and production practices. **418+ questions** across 25+ topics.

---

## Overview

This repo is a **curated Rails knowledge base** combining:

- **Concept explanations** — MVC, request lifecycle, conventions, Rack, Zeitwerk
- **Interview-style Q&A** — with code examples and concise answers
- **Production topics** — security, performance, background jobs, APIs, monitoring
- **Ecosystem** — Hotwire (Turbo/Stimulus), ActiveStorage, ActionCable, Mongoid, credentials

Use it for **study**, **quick lookup**, or **interview prep**.

---

## Table of contents

### Core

| # | Topic | Description |
|---|--------|-------------|
| 01 | [Rails Core](01%20Rails%20Core.md) | MVC, request lifecycle, Convention over Configuration, DRY, Rack, Zeitwerk, Puma |
| 02 | [Routing](02%20Routing.md) | `resources`, nested/shallow routes, member/collection, concerns, constraints, route helpers |
| 03 | [Controllers](03%20Controllers.md) | Actions, `before_action`, strong params, CSRF, `render` vs `redirect_to`, `respond_to` |
| 04 | [Models](04%20Models.md) | ActiveRecord basics, callbacks, scopes, single-table inheritance |
| 05 | [ActiveRecord Queries](05%20ActiveRecord%20Queries.md) | Query interface, `where`, joins, includes, N+1, pluck, find_each |
| 06 | [Associations](06%20Associations.md) | `has_many`, `belongs_to`, `has_many :through`, polymorphic, dependent options |
| 07 | [Migrations & Database](07%20Migrations%20and_Database.md) | Schema changes, indexes, rollback, reversible migrations |
| 08 | [Validations](08%20Validations.md) | Presence, uniqueness, custom validations, error messages |

### Application layer

| # | Topic | Description |
|---|--------|-------------|
| 09 | [Background Jobs](09%20Background%20Jobs.md) | Active Job, Sidekiq/Resque, retries, idempotency |
| 10 | [Mailers](10%20Mailers.md) | Action Mailer, delivery, previews, async sending |
| 11 | [Security](11%20Security.md) | CSRF, XSS, SQL injection, mass assignment, authentication/authorization |
| 12 | [Performance & Caching](12%20Performance%20%26%20Caching.md) | Fragment/page caching, Russian doll, query optimization |
| 13 | [Views & Frontend](13%20Views%20%26%20Frontend.md) | ERB, helpers, layouts, asset pipeline |
| 14 | [Concerns](14%20Concerns.md) | Shared behavior in models and controllers |

### Features & infra

| # | Topic | Description |
|---|--------|-------------|
| 15 | [ActiveStorage](15%20ActiveStorage.md) | File uploads, attachments, variants, cloud storage |
| 16 | [ActionCable](16%20ActionCable.md) | WebSockets, channels, real-time features |
| 17 | [Hotwire (Turbo & Stimulus)](17%20Hotwire%20(Turbo%20%26%20Stimulus).md) | Turbo Drive/Frames/Streams, Stimulus controllers |
| 18 | [Credentials & Secrets](18%20Credentials%20%26%20Secrets%20Management.md) | Encrypted credentials, env-based config |
| 19 | [Rails Engines](19%20Rails%20Engines.md) | Mountable apps, isolation, routes, assets |
| 20 | [Error Handling & Monitoring](20%20Error%20Handling%20%26%20Monitoring.md) | Rescue_from, logging, error tracking |
| 21 | [Testing](21%20Testing.md) | Minitest/RSpec, fixtures/factories, system tests |
| 22 | [API Development](22%20API%20Development.md) | JSON APIs, versioning, authentication |
| — | [Architecture Patterns](22%20Architecture%20Patterns.md) | Service objects, form objects, query objects |

### Advanced & reference

| # | Topic | Description |
|---|--------|-------------|
| 23 | [Practical Scenario Questions](23%20Practical%20Scenario%20Questions.md) | Real-world design and “how would you…” questions |
| 24 | [Mongoid](24%20Mongoid.md) | MongoDB ODM in Rails context |
| 25 | [Rails Application Configuration](25%20Rails%20Application%20Configuration.md) | Environments, initializers, config best practices |
| — | [ERB & HAML](ERB%20%26%20HAML.md) | Template syntax and comparison |
| — | [Rails Application Configuration — Interview Q&A](Rails%20Application%20Configuration%20—%20Important%20Interview%20Q%26A.md) | Config-focused interview Q&A |
| — | [Ruby Language Interview Q&A](Ruby%20Language%20Interview%20Questions%20%26%20Answers.md) | Ruby language questions relevant to Rails |

---

## How to use

- **By role**
  - **Backend / full‑stack:** Core → Controllers, Models, ActiveRecord, Associations → Security, Performance, Background Jobs → API/Architecture.
  - **Frontend (Rails views):** Core, Controllers → Views & Frontend → Hotwire (Turbo & Stimulus) → ERB & HAML.
  - **DevOps / reliability:** Migrations, Credentials → Error Handling & Monitoring → Performance & Caching.
- **By goal**
  - **Interview prep:** Start with [List of all questions](list%20of%20all%20questions.md), then open the linked file for each topic.
  - **Deep dive:** Pick a numbered file (e.g. 06 Associations) and work through every Q&A.
  - **Quick lookup:** Use this README’s table of contents and open the topic file; each doc uses `###` headings for easy search.

---

## Quick reference

| Need to… | See |
|----------|-----|
| Understand a request from browser to response | [01 Rails Core](01%20Rails%20Core.md) — request lifecycle |
| Design RESTful routes | [02 Routing](02%20Routing.md) |
| Protect forms and APIs | [11 Security](11%20Security.md), [03 Controllers](03%20Controllers.md) (strong params, CSRF) |
| Avoid N+1 and speed up queries | [05 ActiveRecord Queries](05%20ActiveRecord%20Queries.md), [12 Performance & Caching](12%20Performance%20%26%20Caching.md) |
| Add file uploads | [15 ActiveStorage](15%20ActiveStorage.md) |
| Build real-time features | [16 ActionCable](16%20ActionCable.md) |
| Keep secrets out of code | [18 Credentials & Secrets](18%20Credentials%20%26%20Secrets%20Management.md) |
| Structure complex logic | [22 Architecture Patterns](22%20Architecture%20Patterns.md) |

---

## Conventions in this repo

- **Headings:** Each question is a `###` heading; sub-answers may use `####`.
- **Code:** Ruby/ERB examples are in fenced blocks with `ruby` or `erb`.
- **Cross-links:** Related topics are mentioned by name; use this README to jump to the right file.
- **Rails version:** Content is written for **Rails 6+ / 7+**; Zeitwerk, Active Job, Hotwire, and modern security defaults are assumed.

---

## Stats

- **Topics:** 25+ markdown docs  
- **Questions:** 418+ (see [list of all questions](list%20of%20all%20questions.md))  
- **Scope:** Core → DB → Jobs → Security → Performance → APIs → Architecture → Config & Ruby

---

*Use the table of contents above to open any topic. For a flat list of every question, use [list of all questions](list%20of%20all%20questions.md).*
