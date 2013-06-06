# capistrano-npm

capistrano-npm is a [Capistrano](https://github.com/capistrano/capistrano) extension that will let you run [npm](https://npmjs.org/) during your deploy process.

## Installation

1. Install the Gem

```bash
gem install capistrano-npm
```

Or if you're using Bundler, add it to your `Gemfile`:

```ruby
gem 'capistrano-npm', github: 'swalkinshaw/npm'
```

2. Add to `Capfile` or `config/deploy.rb`:

```ruby
require 'capistrano/npm'
```

## Usage

Add the task to your `deploy.rb`:

```ruby
after 'deploy:finalize_update', 'npm:install'
```

To speed up your deployments, you should also add `node_modules` to Capistrano's `shared_children` variable:

```ruby
set :shared_children, shared_children + %w{node_modules}
```

This will symlink `node_modules` from the `shared` directory so it isn't re-created from scratch each deploy.

Ideally when using npm, you should add `node_modules` to your `.gitignore` file to keep them out of your repository.

Since `npm install` does not guarantee that you will get the same package versions when deploying, you should use `npm shrinkwrap` (https://npmjs.org/doc/shrinkwrap.html) first.

Running `npm shrinkwrap` will created a `npm-shrinkwrap.json` file that locks down the dependency versions. Check this file into your repository.

Now when deploying, `npm install` will detect the `npm-shrinkwrap.json` file and use that to install the packages.

### Tasks

* `npm:install`: Runs `npm install`.

### Configuration

* `npm_path`: Path to npm bin on the remote server. Defaults to just `npm` as its assumed to be in your `$PATH`.
* `npm_options`: Options for `npm` command. Defaults to an empty string.