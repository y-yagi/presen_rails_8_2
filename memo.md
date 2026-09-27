# この資料について(記載時点の記録)


## Active Support

* Make flaky parallel tests easier to diagnose by deterministically assigning tests to workers.
* Add `group` method to `ActiveSupport::ContinuousIntegration` for parallel step execution.
* Add `prepend: true` option to `ActiveSupport::Notifications.subscribe`.
* Add `delete: true` option to `Rails.cache.read` for atomic read-and-delete (only supported by Redis cache store).
* Introduce `ActiveSupport::TimeFormats` and `ActiveSupport::DateFormats` for registering custom date formats.
* `ActiveSupport::Cache::RedisCacheStore` entirely reimplemented.
* Add `ActiveSupport::Notifications::NullInstrumenter`, a stateless no-op instrumenter that executes blocks without publishing any notifications.

## Action Pack

* Define `ActionController::Parameters#deconstruct_keys` to support pattern matching
* Modern header-based CSRF protection.
* Add a configuration for `ActionDispatch::ExceptionWrapper.wrapper_exceptions` at `config.action_dispatch.wrapper_exceptions`.
* Add `config.action_dispatch.strict_accept_header` to stop forcing an HTML response when the `Accept` header contains the `*/*` wildcard.
* Allow HTTP token authentication to require specific authentication schemes.
* Add support for the HTTP QUERY method defined in RFC 10008.

## Action View

* Fix tag parameter content being overwritten instead of combined with tag block content.
* Add `datalist_tag` to create `datalist` form elements.
* Add `f.datalist` to `FormBuilder`
* Allow setting `config.action_view.erb_implementation` to `:herb` to compile HTML+ERB templates through Herb.
* Add a `herb:check` rake task to verify that the application's HTML+ERB templates compile through Herb.

## Active Model

* Add built-in Argon2 support for `has_secure_password`.
* Add `has_json` and `has_delegated_json` to provide schema-enforced access to JSON attributes.
* Support proc and symbol for `NumericalityValidator`s `:in` option

## Active Record

* Include record ID in error when uniqueness validation fails
* Revert alphabetical sorting of table columns inside `schema.rb`.
* Add `#default_order` query method and association option which can be used to order records when no other order is specified.
* Allow query log tags to be configured per connection pool.
* Support dumping `schema_migrations` in `db/schema.rb`.
* Add query predicate expressions for Active Record types.
* Add `config.active_record.schema_ignored_tables` to exclude tables from both the schema cache and the schema file.
* Add `config.active_record.shuffle_unordered_selects`.
* Active Record schema caches can now be dumped in JSON format.


## Action Mailer

なし

## Active Job

* Jobs are now enqueued after transaction commit.
* Allow `retry_on` `wait` procs to accept the error as a second argument.
* Add `ActiveJob::Attributes` for declaring typed attributes that persist across job serialization and deserialization. It is included by `ActiveJob::Continuable` but can also be used standalone.
* Add `ActiveJob::DeserializationError::RecordNotFound`, raised when argument deserialization fails because a referenced record could not be found, and not for any other reason.

## Action Cable

なし

## Active Storage

* Introduce immediate variants that are generated immediately on attachment
* Introduce `ActiveStorage::Attachment` upload callbacks
* Configurable maximum streaming chunk size
* Offload ActiveStorage::Blob#metadata sync to background
* Allow `config.active_storage.variant_processor` to be set to a transformer class.
* Allow ffmpeg and ffprobe input arguments to be configured.
* Marcel 2 for content type detection
* Introduce `config.active_storage.draw_direct_upload_route` to disable the direct upload route without affecting the other Active Storage routes.

## Action Mailbox

なし

## Action Text

* Add `to_markdown` to Action Text, mirroring `to_plain_text`.

## railties

* Add `Rails.app` as alias for `Rails.application`. Particularly helpful when accessing nested accessors inside application code, like when using `Rails.app.credentials`.
* Add `Rails.app.envs` to provide access to ENV variables through symbol-based lookup with explicit methods for required and optional values. This is the same interface offered by #credentials and can be accessed in a combined manner via #creds.
* Add `Rails.app.dotenvs` to provide access to .env variables through symbol-based lookup with explicit methods for required and optional values. This is the same interface offered by #credentials and can be accessed in a combined manner via #creds.
* Add `Rails.app.creds` to provide combined access to credentials stored in either ENV or the encrypted credentials file, and in development also .env credentials. Provides a new require/option API for accessing these values. Examples:
* Add `Rails.app.revision` to provide a version identifier for error reporting, monitoring, cache keys, etc.
* Add `bin/rails query` command for running read-only database queries.
* Add offline fallback page to the PWA scaffold.
* Enable Ruby `frozen_string_literal` by default.
* Show a Rails-flavored startup banner (a small logo, Rails/Ruby version, a rotating tip about console helpers like `app`/`reload!`, and `Rails.root`) when starting `bin/rails console`.
