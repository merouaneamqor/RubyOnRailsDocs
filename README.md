# Ruby on Rails — Documentation & Interview Prep

> **A curated Rails knowledge base** — structured reference, interview Q&A, and production practices. 418+ questions across 25+ topics. No installation required; use it in the browser, in your editor, or clone the repo.

---

## What is this repository?

This repository is a **documentation and interview-preparation resource** for [Ruby on Rails](https://rubyonrails.org/). It is **not** a Rails application: it contains only Markdown documentation that you can read anywhere (GitHub, VS Code, Cursor, etc.).

- **Rails concepts** — MVC, request lifecycle, conventions, Rack, Zeitwerk
- **Interview-style Q&A** — short answers with code examples
- **Production topics** — security, performance, background jobs, APIs, monitoring
- **Ecosystem** — Hotwire (Turbo/Stimulus), ActiveStorage, ActionCable, Mongoid, credentials

Use it for **study**, **quick lookup**, or **interview prep**.

---

## Repository description (for GitHub)

Suggested **short description** for your repo settings:

```text
Rails documentation & interview Q&A — 418+ questions on core, patterns, and production practices. No codebase, Markdown only.
```

**Suggested topics / tags:** `ruby-on-rails` `documentation` `rails-guides` `ruby` `web-development` `interview-prep` `rails`

*(Add these in GitHub: Repository → About → Edit → Description & Topics.)*

---

## How to use and navigate

- **By role**
  - **Backend / full‑stack:** [Core](docs/01%20Rails%20Core.md) → [Controllers](docs/03%20Controllers.md), [Models](docs/04%20Models.md), [ActiveRecord](docs/05%20ActiveRecord%20Queries.md), [Associations](docs/06%20Associations.md) → [Security](docs/11%20Security.md), [Performance](docs/12%20Performance%20%26%20Caching.md), [Background Jobs](docs/09%20Background%20Jobs.md) → [API](docs/22%20API%20Development.md) / [Architecture](docs/22%20Architecture%20Patterns.md).
  - **Frontend (Rails views):** [Core](docs/01%20Rails%20Core.md), [Controllers](docs/03%20Controllers.md) → [Views & Frontend](docs/13%20Views%20%26%20Frontend.md) → [Hotwire](docs/17%20Hotwire%20(Turbo%20%26%20Stimulus).md) → [ERB & HAML](docs/ERB%20%26%20HAML.md).
  - **DevOps / reliability:** [Migrations](docs/07%20Migrations%20and_Database.md), [Credentials](docs/18%20Credentials%20%26%20Secrets%20Management.md) → [Error Handling & Monitoring](docs/20%20Error%20Handling%20%26%20Monitoring.md) → [Performance & Caching](docs/12%20Performance%20%26%20Caching.md).
- **By goal**
  - **Interview prep:** Open [List of all questions](docs/list%20of%20all%20questions.md) and use the links to jump to each answer.
  - **Deep dive:** Pick a topic from the table below and work through every Q&A in that doc.
  - **Quick lookup:** Use the table of contents or the quick reference table below.

---

## Directory structure

| Folder      | Purpose |
|------------|---------|
| [**docs/**](docs/) | Main documentation (all topic files and the full question list). |
| [**guides/**](guides/) | Short, focused guides (getting started, workflows). |
| [**examples/**](examples/) | Code snippets and small examples referenced by the docs. |
| [**assets/**](assets/) | Images, diagrams, and other static assets. |

All core content lives under **docs/**.

---

## Table of contents (main docs)

### Core

| # | Topic | Description |
|---|--------|-------------|
| 01 | [Rails Core](docs/01%20Rails%20Core.md) | MVC, request lifecycle, Convention over Configuration, DRY, Rack, Zeitwerk, Puma |
| 02 | [Routing](docs/02%20Routing.md) | `resources`, nested/shallow routes, member/collection, concerns, constraints, route helpers |
| 03 | [Controllers](docs/03%20Controllers.md) | Actions, `before_action`, strong params, CSRF, `render` vs `redirect_to`, `respond_to` |
| 04 | [Models](docs/04%20Models.md) | ActiveRecord basics, callbacks, scopes, single-table inheritance |
| 05 | [ActiveRecord Queries](docs/05%20ActiveRecord%20Queries.md) | Query interface, `where`, joins, includes, N+1, pluck, find_each |
| 06 | [Associations](docs/06%20Associations.md) | `has_many`, `belongs_to`, `has_many :through`, polymorphic, dependent options |
| 07 | [Migrations & Database](docs/07%20Migrations%20and_Database.md) | Schema changes, indexes, rollback, reversible migrations |
| 08 | [Validations](docs/08%20Validations.md) | Presence, uniqueness, custom validations, error messages |

### Application layer

| # | Topic | Description |
|---|--------|-------------|
| 09 | [Background Jobs](docs/09%20Background%20Jobs.md) | Active Job, Sidekiq/Resque, retries, idempotency |
| 10 | [Mailers](docs/10%20Mailers.md) | Action Mailer, delivery, previews, async sending |
| 11 | [Security](docs/11%20Security.md) | CSRF, XSS, SQL injection, mass assignment, authentication/authorization |
| 12 | [Performance & Caching](docs/12%20Performance%20%26%20Caching.md) | Fragment/page caching, Russian doll, query optimization |
| 13 | [Views & Frontend](docs/13%20Views%20%26%20Frontend.md) | ERB, helpers, layouts, asset pipeline |
| 14 | [Concerns](docs/14%20Concerns.md) | Shared behavior in models and controllers |

### Features & infra

| # | Topic | Description |
|---|--------|-------------|
| 15 | [ActiveStorage](docs/15%20ActiveStorage.md) | File uploads, attachments, variants, cloud storage |
| 16 | [ActionCable](docs/16%20ActionCable.md) | WebSockets, channels, real-time features |
| 17 | [Hotwire (Turbo & Stimulus)](docs/17%20Hotwire%20(Turbo%20%26%20Stimulus).md) | Turbo Drive/Frames/Streams, Stimulus controllers |
| 18 | [Credentials & Secrets](docs/18%20Credentials%20%26%20Secrets%20Management.md) | Encrypted credentials, env-based config |
| 19 | [Rails Engines](docs/19%20Rails%20Engines.md) | Mountable apps, isolation, routes, assets |
| 20 | [Error Handling & Monitoring](docs/20%20Error%20Handling%20%26%20Monitoring.md) | Rescue_from, logging, error tracking |
| 21 | [Testing](docs/21%20Testing.md) | Minitest/RSpec, fixtures/factories, system tests |
| 22 | [API Development](docs/22%20API%20Development.md) | JSON APIs, versioning, authentication |
| — | [Architecture Patterns](docs/22%20Architecture%20Patterns.md) | Service objects, form objects, query objects |

### Advanced & reference

| # | Topic | Description |
|---|--------|-------------|
| 23 | [Practical Scenario Questions](docs/23%20Practical%20Scenario%20Questions.md) | Real-world design and “how would you…” questions |
| 24 | [Mongoid](docs/24%20Mongoid.md) | MongoDB ODM in Rails context |
| 25 | [Rails Application Configuration](docs/25%20Rails%20Application%20Configuration.md) | Environments, initializers, config best practices |
| — | [ERB & HAML](docs/ERB%20%26%20HAML.md) | Template syntax and comparison |
| — | [Rails Application Configuration — Interview Q&A](docs/Rails%20Application%20Configuration%20—%20Important%20Interview%20Q%26A.md) | Config-focused interview Q&A |
| — | [Ruby Language Interview Q&A](docs/Ruby%20Language%20Interview%20Questions%20%26%20Answers.md) | Ruby language questions relevant to Rails |

**Full question index:** [List of all questions (with links to answers)](docs/list%20of%20all%20questions.md)

---

## Quick reference

| Need to… | See |
|----------|-----|
| Understand request → response | [01 Rails Core](docs/01%20Rails%20Core.md) — request lifecycle |
| Design RESTful routes | [02 Routing](docs/02%20Routing.md) |
| Protect forms and APIs | [11 Security](docs/11%20Security.md), [03 Controllers](docs/03%20Controllers.md) (strong params, CSRF) |
| Avoid N+1 and speed up queries | [05 ActiveRecord Queries](docs/05%20ActiveRecord%20Queries.md), [12 Performance & Caching](docs/12%20Performance%20%26%20Caching.md) |
| Add file uploads | [15 ActiveStorage](docs/15%20ActiveStorage.md) |
| Build real-time features | [16 ActionCable](docs/16%20ActionCable.md) |
| Keep secrets out of code | [18 Credentials & Secrets](docs/18%20Credentials%20%26%20Secrets%20Management.md) |
| Structure complex logic | [22 Architecture Patterns](docs/22%20Architecture%20Patterns.md) |

---

## Installation and setup

**None required.** This repo is documentation only (Markdown). You can:

- **Read on GitHub** — browse and use the links; heading anchors work in the browser.
- **Clone the repo** — to read locally, search, or contribute:
  ```bash
  git clone <your-repo-url>
  cd "ruby on rails"
  ```
- **Use in an editor** — open the folder in VS Code, Cursor, or any editor; internal links work with standard Markdown preview.

No Ruby, Rails, or database setup is needed to use this documentation.

---

## Contributing

We welcome improvements: typo fixes, clearer explanations, new Q&A, or updated examples. Please read **[CONTRIBUTING.md](CONTRIBUTING.md)** for:

- How to suggest changes (issues, pull requests)
- Style and structure for new or edited docs
- How we review and merge contributions

---

## Conventions in this repo

- **Headings:** Each question is a `###` heading; sub-answers may use `####`.
- **Code:** Ruby/ERB examples are in fenced blocks with `ruby` or `erb`.
- **Cross-links:** Related topics are mentioned by name; use this README or the [list of all questions](docs/list%20of%20all%20questions.md) to jump to the right file.
- **Rails version:** Content targets **Rails 6+ / 7+** (Zeitwerk, Active Job, Hotwire, modern security defaults).

---

## Stats

- **Topics:** 25+ markdown docs in [docs/](docs/)
- **Questions:** 418+ (see [list of all questions](docs/list%20of%20all%20questions.md))
- **Scope:** Core → DB → Jobs → Security → Performance → APIs → Architecture → Config & Ruby

---

## License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details. You may use, copy, and adapt the content with attribution.

---

*Use the table of contents above to open any topic. For a flat list of every question with links to answers, use [list of all questions](docs/list%20of%20all%20questions.md).*
