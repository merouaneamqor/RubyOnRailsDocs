# Contributing to Ruby on Rails Documentation

Thank you for considering contributing. This repo is a **documentation-only** project: we welcome typo fixes, clearer explanations, new Q&A, and updated examples.

---

## How to contribute

### Reporting issues

- **Typos, broken links, or unclear wording:** Open an [issue](../../issues) with the file name and section (or question number).
- **Suggesting new topics or questions:** Open an issue describing what you’d like to see and where it could live (e.g. under [docs/](docs/)).

### Submitting changes

1. **Fork** the repository and clone your fork.
2. **Create a branch** (e.g. `fix-typo-routing`, `add-api-versioning-qa`).
3. **Edit** the Markdown files in [docs/](docs/) (or [guides/](guides/), [examples/](examples/) as needed).
4. **Commit** with a clear message (e.g. “Fix typo in 02 Routing.md”, “Add Q&A on API versioning”).
5. **Push** to your fork and open a **pull request** against the default branch.

We’ll review the PR and may ask for small edits. Once approved, your changes will be merged.

---

## Style and structure

### Documentation (docs/)

- **One main idea per Q&A:** Each `###` heading is one question; the content below is the answer.
- **Code blocks:** Use fenced code blocks with a language tag:
  ```ruby
  # Ruby
  ```
  ```erb
  <%# ERB %>
  ```
- **Consistency:** Match the tone and level of detail of existing topics (concise, practical).
- **Rails version:** Aim for **Rails 6+ / 7+** (Zeitwerk, Active Job, Hotwire, etc.).

### Adding new questions

- Place new Q&A in the most relevant existing file under [docs/](docs/) (e.g. new routing question in `02 Routing.md`).
- If you add a new topic file, use the same numbering/style as existing docs and add it to the root [README.md](README.md) table of contents and, if applicable, to [docs/list of all questions.md](docs/list%20of%20all%20questions.md) with a link to the new heading.

### Guides and examples

- **guides/:** Short, step-by-step guides. Use clear headings and optional code snippets.
- **examples/:** Minimal, runnable or copy-paste code. Add a short comment or README if needed.

---

## What we’re looking for

- **Accuracy:** Correct Rails behavior and up-to-date APIs.
- **Clarity:** Short sentences and concrete examples.
- **Relevance:** Content that helps with learning, interviews, or day-to-day Rails work.

We may reject or request changes to contributions that are off-topic, promotional, or inconsistent with the above.

---

## Questions

If you’re unsure where to put something or how to phrase it, open an issue and we’ll help.

Thanks for contributing.
