# Juwelier: Craft the perfect RubyGem for Ruby 2.3.x and Beyond

 Provides the noble ruby developer with two primary features:

 * a library or managing and releasing RubyGem projects
 * a scaffold generator for starting new RubyGem projects

PLEASE NOTE that I have recently taken over the original Jeweler and will be
maintaining both repos for a while, and eventually converge them to one. In
the mean time, all new features shall be added to Juwelier, while keeping
the origial Jeweler up-to-date with the latest Ruby releases.

ALSO NOTE that I am transistioning the README to the orgmode format. As such,
the Markdown version may be out of date, especially with the release notes
and changelogs. Please refer to it instead.

[![Build Status](https://travis-ci.org/flajann2/juwelier.png)](https://travis-ci.org/flajann2/juwelier)
[![Coverage Status](https://coveralls.io/repos/flajann2/juwelier/badge.png)](https://coveralls.io/r/flajann2/juwelier)
[![Dependency Status](https://www.versioneye.com/ruby/juwelier/2.0.0/badge.png)](https://www.versioneye.com/ruby/juwelier/2.0.0)

 "Juwelier" is pronounced "you-ve-LEER" (with German inflection! :))

 Note that his has been forked from the old Jeweler by Josh Nichols
 due to lack of maintenance. I need this to work alread with the
 latest Ruby, so I've taken it over. All is cool because
 sometimes we move on and loose interest. I wish to thank
 Josh and others who were behind the original Jeweler for
 creating this awesome tool.

 Note that if you have a preexisting project created with
 Jeweler, you may have some issues. Eventally I will provide
 a migration option, but in the meantime, you may wish to
 run this bash script from the root directory of your project:

```bash
for f in $(grep -irl jeweler *)
do
  sed -i 's/jeweler/juwelier/g' $f
  sed -i 's/Jeweler/Juwelier/g' $f
done
bundle update
```

 As you know, "Juwelier" is "Jeweler" in German. Since I
 have made Germany my new home, it only seemed approporiate.

## Hello, world

Use RubyGems to install the heck out of juwelier to get started:

`$ gem install juwelier`

With juwelier installed, you can use the `juwelier` command to generate a new project. For the most basic use, just give it a name:

`$ juwelier hello-gem`

This requires some Git configuration (like name, email, GitHub account, etc), but `juwelier` will prompt along the way.

Your new `hello-gem` gem is ready in the `hello-gem` directory. Take a peek, and you'll see several files and directories

 * `Rakefile` setup for juwelier, running tests, generating documentation, and releasing to [rubygems.org](http://rubygems.org/)
 * `README.rdoc` with contribution guidelines and copyright info crediting you
 * `LICENSE` with the MIT licensed crediting you
 * `Gemfile` with development dependencies filled in
 * `lib/hello-gem.rb` waiting for you to code
 * `test/` containing a (failing) shoulda test suite [shoulda](http://github.com/thoughtbot/shoulda)


### More `juwelier` options

The `juwelier` command supports a lot of options. Mostly, they are for generating baked in support for this test framework, or that.

Check out `juwelier --help` for the most up to date options.

## Hello, rake tasks

Beyond just editing source code, you'll be interacting with your gem using `rake` a lot. To see all the tasks available with a brief description, you can run:

`$ rake -T`

You'll need a version before you can start installing your gem locally. The easiest way is with the `version:write` Rake task. Let's imagine you start with 0.1.0

`$ rake version:write MAJOR=0 MINOR=1 PATCH=0`

You can now go forth and develop, now that there's an initial version defined. Eventually, you should install and test the gem:

`$ rake install`

The `install` rake task builds the gem and `gem install`s it. You're all set if you're using [RVM](http://rvm.beginrescueend.com/), but you may need to run it with sudo if you have a system-installed ruby:

`$ sudo rake install`

### Releasing

At last, it's time to [ship it](http://shipitsquirrel.github.com/)! Make sure you have everything committed and pushed, then go wild:

`$ rake release`

This will automatically:

 * Juwelier Generate `hello-gem.gemspec` and commit it
 * Use `git` to tag `v0.1.0` and push it
 * Build `hello-gem-0.1.0.gem` and push it to [rubygems.org](http://rubygems.org/gems/)

`rake release` accepts REMOTE(default: `origin`), LOCAL_BRANCH(default: `master`), REMOTE_BRANCH(default: `master`) and BRANCH(default: master)as options.

`$ rake release REMOTE=upstream LOCAL_BRANCH=critical-security-fix REMOTE_BRANCH=v3`

This will tag and push the commits on your local branch named `critical-security-fix` to branch named `v3` in remote named `upstream` (if you have commit rights
on `upstream`) and release the gem.

`$ rake release BRANCH=v3`

If both remote and local branches are the same, use `BRANCH` option to simplify.
This will tag and push the commits on your local branch named `v3` to branch named `v3` in remote named `origin` (if you have commit rights
on `origin`) and release the gem.

### Version bumping

It feels good to release code. Do it, do it often. But before that, bump the version. Then release it. There's a few ways to update the version:

```bash
# version:write like before
$ rake version:write MAJOR=0 MINOR=3 PATCH=0

# bump just major, ie 0.1.0 -> 1.0.0
$ rake version:bump:major

# bump just minor, ie 0.1.0 -> 0.2.0
$ rake version:bump:minor

# bump just patch, ie 0.1.0 -> 0.1.1
$ rake version:bump:patch
```

Then it's the same `release` we used before:

`$ rake release`

## Customizing your gem

If you've been following along so far, your gem is just a blank slate. You're going to need to make it colorful and full of metadata.

You can customize your gem by updating your `Rakefile`. With a newly generated project, it will look something like this:

```ruby
require 'juwelier'
Juwelier::Tasks.new do |gem|
  # gem is a Gem::Specification... see http://guides.rubygems.org/specification-reference/ for more options
  gem.name = "whatwhatwhat"
  gem.summary = %Q{TODO: one-line summary of your gem}
  gem.description = %Q{TODO: longer description of your gem}
  gem.email = "fred.mitchell@gmx.com"
  gem.homepage = "http://github.com/flajann2/whatwhatwhat"
  gem.authors = ["Joshua Nichols"]
end
JuwelierJuwelier::RubygemsDotOrgTasks.new
```

It's crucial to understand the `gem` object is just a Gem::Specification. You can read up about it at [guides.rubygems.org/specification-reference](http://guides.rubygems.org/specification-reference/). This is the most basic way of specifying a gem, -managed or not.  just exposes this to you, in addition to providing some reasonable defaults, which we'll explore now.

### Project information

A short description about the configuration in the previous item.

- **gem.name**: Every gem has a name. Among other things, the gem name is how you are able to `gem install` it. [Reference](http://guides.rubygems.org/specification-reference/#name)

- **gem.summary**: This is a one line summary of your gem. This is displayed, for example, when you use `gem list --details` or view it on [rubygems.org](http://rubygems.org/gems/).

- **gem.description**: Description is a longer description. Scholars ascertain that knowledge of where the description is used was lost centuries ago.

- **gem.email**: This should be a way to get a hold of you regarding the gem.

- **gem.homepage**: The homepage should have more information about your gem. The juwelier generator guesses this based on the assumption your code lives on [GitHub](http://github.com/), using your Git configuration to find your GitHub username. This is displayed by `gem list --details` and on rubygems.org.

- **gem.authors**: Hey, this is you, the author (or me in this case). The `juwelier` generator also guesses this from your Git configuration. This is displayed by `gem list --details` and on rubygems.org.

### Files

The quickest way to add more files is to `git add` them. Juwelier uses your git repository to populate your gem's files by including added and committed and excluding `.gitignore`d. In most cases, this is reasonable enough.

If you need to tweak the files, that's cool. Juwelier populates `gem.files` as a `Rake::FileList`. It's like a normal array, except you can `include` and `exclude` file globs:

```ruby
gem.files.exclude 'tmp' # exclude temporary directory
gem.files.include 'lib/foo/bar.rb' # explicitly include lib/foo/bar.rb
```

If that's not enough, you can just set `gem.files` outright

`gem.files = Dir.glob('lib/**/*.rb')`

### Dependencies

Dependencies let you define other gems that your gem needs to function. `gem install your-gem` will install your-gem's dependencies along with it, and when you use your-gem in an application, the dependencies will be made available. Use `gem.add_dependency` to register them. [Reference](http://guides.rubygems.org/specification-reference/#add_development_dependency)

`gem.add_dependency 'nokogiri'`

This will ensure a version of `nokogiri` is installed, but it doesn't require anything more than that. You can provide extra args to be more specific:

```ruby
gem.add_dependency 'nokogiri', '= 1.2.1' # exactly version 1.2.1
gem.add_dependency 'nokogiri', '>= 1.2.1' # greater than or equal to 1.2.1, ie, 1.2.1, 1.2.2, 1.3.0, 2.0.0, etc
gem.add_dependency 'nokogiri', '>= 1.2.1', '< 1.3.0' # greater than or equal to 1.2.1, but less than 1.3.0
gem.add_dependency 'nokogiri', '~> 1.2.1' # same thing, but more concise
```

When specifying which version is required, there's a bit of the condunrum. You want to allow the most versions possible, but you want to be sure they are compatible. Using `>= 1.2.1` is fine most of the time, except until the point that 2.0.0 comes out and totally breaks backwards the API. That's when it's good to use `~> 1.2.1`, which requires any version in the `1.2` family, starting with `1.2.1`.

### Executables

Executables let your gem install shell commands. Just put any executable scripts in the `bin/` directory, make sure they are added using `git`, and  will take care of the rest.

When you need more finely grained control over it, you can set it yourself:

`gem.executables = ['foo'] # note, it's the file name relative to bin/, not the project root`

### Versioning

We discussed earlier how to bump the version. The rake tasks are really just convience methods for manipulating the `VERSION` file. It just contains a version string, like `1.2.3`.

`VERSION` is a convention used by , and is used to populate `gem.version`. You can actually set this yourself, and  won't try to override it:

`gem.version = '1.2.3'`

A common pattern is to have this in a version constant in your library. This is convenient, because users of the library can query the version they are using at runtime.

```ruby
# in lib/foo/version.rb
class Foo
  module Version
    MAJOR = 1
    MINOR = 2
    PATCH = 3
    BUILD = 'pre3'

    STRING = [MAJOR, MINOR, PATCH, BUILD].compact.join('.')
  end
end
```

```ruby
# in Rakefile
require 'juwelier'
require './lib/foo/version.rb'
Juwelier::Tasks.new do |gem|
  # snip
  gem.version = Foo::Version::STRING
end
```

### Rake tasks

Juwelier tasks lives inside of Rake. As a result, they are dear friends. But, that friendship doesn't interfere with typical Rake operations.

This means you can define your own namespaces, tasks, or use third party Rake libraries without cause for concern.

## Contributing to

* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet
* Ask on the [mailing list](http://groups.google.com/group/juwelier-rb) for feedback on your proposal, to see if somebody else has done it.
* Check out the [issue tracker](http://github.com/flajann2/juwelier/issues) to make sure someone already hasn't requested it and/or contributed it
* Fork the project
* Start a feature/bugfix branch
* Commit and push until you are happy with your contribution
* Make sure to add tests for the feature/bugfix. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate it to its own commit so I can cherry-pick around it.

## Copyright

Copyright (c) 2016 Fred Mitchell. See LICENSE for details.
