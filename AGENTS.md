# Repository Guidelines

## Project Structure & Module Organization

`lib/resque/` holds the core queueing, worker, failure backend, logging, and
Sinatra web UI code. `lib/active_job/` contains the Rails adapter. CLI
entrypoints live in `bin/`, shared Rake tasks in `lib/tasks/`, server views in
`lib/resque/server/views/`, and static assets in
`lib/resque/server/public/`.

Tests live in `test/**/*_test.rb`. Active Job coverage is split between
`test/active_job/cases/` and `test/active_job/integration/`, with Rails matrix
Gemfiles in `test/gemfiles/`. Use `examples/demo/` for a runnable sample and
`docs/` for supplemental notes such as hooks and plugins.

## Build, Test, and Development Commands

Run `bundle install` to install development dependencies.

- `bundle exec rake` runs the default test suite.
- `bundle exec rake test:activejob` runs Active Job adapter tests.
- `BUNDLE_GEMFILE=test/gemfiles/Gemfile-rails-8.1 bundle exec rake test:activejob:integration` runs one Rails integration target from the CI matrix.
- `bundle exec rubocop` runs the repository lint rules.
- `QUEUE=* bundle exec rake resque:work` starts a local worker.
- `bundle exec ruby bin/resque-web` starts the web UI for manual checks.

Tests start isolated Redis instances via `test/test_helper.rb`, but
`redis-server` must be available on `PATH`.

## Coding Style & Naming Conventions

Follow the existing Ruby style: 2-space indentation, no hard tabs, and roughly
80-column lines per `CONTRIBUTING.md`. Use `snake_case` for methods,
variables, and files; use `CamelCase` for classes and modules, usually under
the `Resque` namespace. Match the surrounding API shape before introducing new
abstractions.

## Testing Guidelines

Add or update tests for every behavioral change. Prefer focused tests in the
matching file, such as `test/worker_test.rb` for worker behavior or
`test/resque-web_test.rb` for UI behavior. Most tests use Minitest spec style
with `describe`/`it`. For Active Job changes, cover both adapter tests and, if
needed, one integration target.

## Commit & Pull Request Guidelines

Recent history uses short, imperative subjects such as `Apply suggestions from
code review` or `Clarify that Rails 7.2 is the minimum supported version`.
Keep commits focused and descriptive.

Before opening a PR, read `CONTRIBUTING.md`, include tests and docs updates,
and update `HISTORY.md` for user-visible changes that are not purely internal.
PRs should summarize behavior changes, link related issues, and note the
commands you ran. Add screenshots only for web UI changes.
