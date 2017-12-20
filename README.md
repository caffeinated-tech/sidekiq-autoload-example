# Sidekiq Autoload example

To see the difference between rails autoload and sidekiq autoloading due to different logic in the constantize method:

1. Clone repo & checkout `master`
2. `bundle install`
3. Run `bundle exec sidekiq` in one console
4. Separately run `rails c` and execute:
```
  ::Foo::Baz::Bar::ExampleWorker.perform_async
```
  - The worker is defined in `app/sidekiq/foo/baz/bar/example_worker.rb`
  - But as a top level `::Baz` module is defined in `app/services/baz.rb` and is accessed during intialization by `config/initializers/setup_bar_module.rb`, the Sidekiq `constantize` method tries to load `::Baz::Bar` instead of `::Foo::Baz::Bar` and fails.

This example will work by switching to the `modified_constantize` branch and following steps 2-4 above.