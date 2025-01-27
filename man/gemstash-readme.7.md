---
title: gemstash-readme
date: November 30, 2015
section: 7
insert_images: true
markdown_github_link_path: .
markdown_github_link_name: README.md
html_link_name: index.html
...

# Gemstash

## What is Gemstash?

Gemstash is both a cache for remote servers such as https://rubygems.org, and a
private gem source.

If you are using [bundler][BUNDLER] across many machines that have
access to a server within your control, you might want to use Gemstash.

If you produce gems that you don't want everyone in the world to have access to,
you might want to use Gemstash.

If you frequently bundle the same set of gems across multiple projects, you
might want to use Gemstash.

Are you only using gems from https://rubygems.org, and don't bundle the same
gems frequently? Well, maybe you don't need Gemstash... yet.

<a href="https://rubycentral.org/"><img src="https://global.discourse-cdn.com/business7/uploads/rubycentral/original/1X/43afd1ed967a1b6e3040965db20af65b665744ec.png" width=200></a><br/>Gemstash
is maintained by [Ruby Central][RUBY_CENTRAL], a non-profit
committed to supporting the critical Ruby infrastructure you rely on. Contribute
today [as an individual, or even better, as a company][RUBY_CENTRAL_SIGNUP], and ensure that Bundler, RubyGems, Gemstash,
and other shared tooling is around for years to come.

## Quickstart Guide

### Setup

Gemstash is designed to be quick and painless to get set up. By the end of this
Quickstart Guide, you will be able to bundle stashed gems from public sources
against a Gemstash server running on your machine.

Install Gemstash to get started:

```
$ gem install gemstash
```

After it is installed, starting Gemstash requires no additional steps. Simply
start the Gemstash server with the `gemstash` command:

```
$ gemstash start
```

You may have noticed that the command finished quickly. This is because Gemstash
will run the server in the background by default. The server runs on port 9292.

### Bundling

With the server running, you can bundle against it. Tell Bundler that you want
to use Gemstash to find gems from RubyGems.org:

```
$ bundle config mirror.https://rubygems.org http://localhost:9292
```

Now you can create a Gemfile and install gems through Gemstash:

```ruby
# ./Gemfile
source "https://rubygems.org"
gem "rubywarrior"
```

The gems you include should be gems you don't yet have installed,
otherwise Gemstash will have nothing to stash. Now bundle:

```
$ bundle install --path .bundle
```

Your Gemstash server has fetched the gems from https://rubygems.org and cached
them for you! To prove this, you can disable your Internet connection and try
again. Gem files (*.gem) are cached indefinitely. Gem dependencies metadata are
cached for 30 minutes, so if you bundle again before that, you can successfully
bundle without an Internet connection:

```
$ # Disable your Internet first!
$ rm -rf Gemfile.lock .bundle
$ bundle
```

### Falling back to rubygems.org

If you want to make sure that your bundling from https://rubygems.org still
works as expected when the Gemstash server is not running, you can easily
configure Bundler to fallback to https://rubygems.org.

```
$ bundle config mirror.https://rubygems.org.fallback_timeout true
```

You can also configure this fallback as a number of seconds in case the Gemstash
server is simply unresponsive. This example uses a 3 second timeout:

```
$ bundle config mirror.https://rubygems.org.fallback_timeout 3
```

### Stopping the Server

Once you've finish using your Gemstash server, you can stop it just as easily as
you started it:

```
$ gemstash stop
```

You'll also want to tell Bundler that it can go back to getting gems from
RubyGems.org directly, instead of going through Gemstash:

```
$ bundle config --delete mirror.https://rubygems.org
```

### Under the Hood

You might wonder where the gems are stored. After running the commands above,
you will find a new directory at `~/.gemstash`. This directory holds all the
cached and private gems. It also has a server log, the database, and
configuration for Gemstash. If you prefer, you can [point to a different
directory][CUSTOMIZE_FILES].

Gemstash uses [SQLite][SQLITE] to store details about private
gems. The database will be located in `~/.gemstash`, however you won't see the
database appear until you start using private gems. If you prefer, you can [use
a different database][CUSTOMIZE_DATABASE].

Gemstash temporarily caches things like gem dependencies in memory. Anything
cached in memory will last for 30 minutes before being retrieved again. You can
[use memcached][CUSTOMIZE_CACHE] instead of caching in memory. Gem files
are always cached permanently, so bundling with a `Gemfile.lock` with all gems
cached will never call out to https://rubygems.org.

The server you ran is provided via [Puma][PUMA] and [Rack][RACK], however they
are not customizable at this point.

## Deep Dive

Deep dive into more subjects:

* [Private gems][PRIVATE_GEMS]
* [Multiple gem sources][MULTIPLE_SOURCES]
* [Using Gemstash as a mirror][MIRROR]
* [Customizing the server (database, storage, caching, and more)][CUSTOMIZE]
* [Deploying Gemstash][DEPLOY]
* [Debugging Gemstash][DEBUGGING]

## Reference

An anatomy of various configuration and commands:

* [Configuration][CONFIGURATION]
* [Authorize][AUTHORIZE]
* [Start][START]
* [Stop][STOP]
* [Status][STATUS]
* [Setup][SETUP]
* [Version][VERSION]

To see what has changed in recent versions of Gemstash, see the
[CHANGELOG][CHANGELOG].

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run
`rake` to run RuboCop and the tests. While developing, you can run
`bin/gemstash` to run Gemstash. You can also run `bin/console` for an
interactive prompt that will allow you to experiment.

## Contributing

Bug reports and pull requests are welcome on GitHub at
https://github.com/rubygems/gemstash. This project is intended to be a safe,
welcoming space for collaboration, and contributors are expected to adhere to
the [Contributor Covenant][CODE_OF_CONDUCT] code of conduct.

## License

The gem is available as open source under the terms of the
[MIT License][LICENSE].

[BUNDLER]: http://bundler.io/
[RUBY_CENTRAL]: https://rubycentral.org/
[RUBY_CENTRAL_SIGNUP]: https://rubycentral.org/#/portal/signup
[CUSTOMIZE_FILES]: ./gemstash-customize.7.md#files
[SQLITE]: https://www.sqlite.org/
[CUSTOMIZE_DATABASE]: ./gemstash-customize.7.md#database
[CUSTOMIZE_CACHE]: ./gemstash-customize.7.md#cache
[PUMA]: http://puma.io/
[RACK]: http://rack.github.io/
[PRIVATE_GEMS]: ./gemstash-private-gems.7.md
[MULTIPLE_SOURCES]: ./gemstash-multiple-sources.7.md
[MIRROR]: ./gemstash-mirror.7.md
[CUSTOMIZE]: ./gemstash-customize.7.md
[DEPLOY]: ./gemstash-deploy.7.md
[DEBUGGING]: ./gemstash-debugging.7.md
[CONFIGURATION]: ./gemstash-configuration.5.md
[AUTHORIZE]: ./gemstash-authorize.1.md
[START]: ./gemstash-start.1.md
[STOP]: ./gemstash-stop.1.md
[STATUS]: ./gemstash-status.1.md
[SETUP]: ./gemstash-setup.1.md
[VERSION]: ./gemstash-version.1.md
[CHANGELOG]: https://github.com/rubygems/gemstash/blob/master/CHANGELOG.md
[CODE_OF_CONDUCT]: https://github.com/rubygems/gemstash/blob/master/CODE_OF_CONDUCT.md
[LICENSE]: http://opensource.org/licenses/MIT
