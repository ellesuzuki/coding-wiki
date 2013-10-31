# [Bundler][1]

*A way to manage Ruby dependencies*

**Install** at command line:
`gem install bundler`

Creating a new Rails app at this point will auto run `bundle install` so it has all gems it depends on installed (from Gemfile).

Bundler will use latest version by default, if gem line doesn't include version number.

`bundle` same as `bundle install`.

Running `bundle` means use version that was installed initially, thanks to `Gemfile.lock`

`Gemfile.lock` specifies versions used for project.

*No matter what system you're on, use same version of a gem for project.*

To **update** specific gem:
`bundle update <gem_name>`

Update all gems to latest versions:
`bundle update`

For more control wrt gem version numbers (`Gemfile`):
`gem '<gem_name>', '1.0'`

`bundle update` will stick to version explicitly stated above.

`gem '<gem_name>', '~> 1.0'` will only update the last number, so here up to 1.1.

Can **specify git repo to use** instead of version number:

`gem '<gem_name>', :git => 'git://github.com/rails/rails.git'`

`gem '<gem_name>', github: 'rails/rails'`

Handy if 

- bug fix not yet included in latest gem release
- want to fork a repo, make your project use your version of the code

Alternatively, can use `:path => '~/local_version_of_gem'`

### Groups

Helps control when a gem should be installed/loaded.

Rails has default behavior for certain groups.

`gem 'capybara', :group => :test`

`group :assets do .. end` only gets loaded in test and development (not production).

Control groups loaded by Bundler in `config/application.rb`

Get help with `bundle help`

Get list of gems have newer version than what's used in your project
`bundle outdated`

### bundle exec

Some gems have executable available at command line
e.g., `rake`

Prefixing commands with `bundle exec` forces version to the version application depends on (installed for project).

Without prefix, could be running completely incorrect version of a command, causing problems.

*Helps to keep environment consistent and expected, by specifying gem versions to be what was installed for app.*

Compare `rake --version` with `bundle exec rake --version`

##### Ways to Simplify Use

1. Could shorten with `be` shell alias
2. Make convenient alternatively by enabling bundler plugin (oh-my-zsh, #308) to alias commonly used bundle exec commands
3. Use binstubs to run gem commands through bundler.
        
    `bundle install --binstubs`

    Generates local bin dir for executables for all gems.

##### Configure How Gem Compiled

Gem doesn't compile properly, configure how it is compiled.

`bundle config build.<gem_name>`

Gets stored locally under own home, `~/.bundle/config`

### Summary

There are two (2) parts to this: Bundler and `bundle exec`

Bundler is a tool that helps manage the gems and their versions for your application.

It works closely with the Rails application's `Gemfile` to install gems and determine version numbers for those gems.

Configuration for this is available at `config/application.rb`

There are many commands, such as `bundle install`, `bundle update`, etc.

`bundle exec` is important because when running gem executables, prefixing those calls with this will ensure that the gem versions are the versions the application relies on.  The environment is consistent.


[1]: http://railscasts.com/episodes/201-bundler-revised
