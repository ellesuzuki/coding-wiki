# [Request Specs and Capybara][1]

High-level testing

## Request Specs

RSpec's alternative to the built-in Rails integration tests.

1. Install to Gemfile:
	
		group :development, :test do
			gem 'rspec-rails'
		end

2. `bundle install`

3. Get Rails set up

    `rails g rspec:install`

4. Generate request spec
		
		rails g integration_test <name> => spec/requests/<name>s_spec.rb

TDD ensures tests are truly working.

If non-TDD, break tests to make sure they and code work as expected.

So far, only simple request specs available to us.

## [Capybara][2]

Want to test full user experience, mimic user's action.

Alternative to Webrat.

1. Set up in Gemfile:

		group :development, :test do
			gem 'rspec-rails'
			gem 'capybara'
		end

2. `bundle install`

This causes language used in integration tests to change.

Before:
	
	get recipes_path
	response.body.should include('Blah')

After:
	
	visit recipes_path
	page.should have_content('Blah')

Run tests:

`rake spec:requests`

### Launchy

Use this tool to see what's happening in the browser while the tests run.

Call `save_and_open_page` within test steps.

Browser will open up at that point in the code.

### Testing JavaScript

Capybara, by default, doesn't support JavaScript.

Must tell Capybara explicitly to test it *through Selenium*.

In `spec_helper.rb`, already has line

`require 'rspec/rails'`

Add

`require 'capybara/rspec'`

Now can tell Capybara to use JavaScript driver with option

`:js => true`

e.g.,

	it 'supports js', :js => true do
		..
	end

Tests will launch firefox and use *Selenium* to test in the background.

Problem with database records: unavailable through Selenium tests.

Specs using database transactions (`spec_helper.rb`) -- not compatible with Selenium.

	config.use_transactional_fixtures = true

Change `true` above to `false`.

Will carry over database records between specs, which is undesirable.

Use `database_cleaner` to clear out db between specs.

### database_cleaner

1. Add to Gemfile in :development, :test block

    `gem 'database_cleaner'`

2. `bundle install`

3. Copy code from online docs to `spec_helper.rb` for configuration

Use 'truncation' instead of 'transactions'.

Can use Capybara for acceptance testing, with feature-scenario DSL (eliminates competitor/predecessor Steak).

Capybara nice alternative to Cucumber.

## Misc

Can tell gem in Gemfile to check in with git repo for latest version:

	gem 'capybara', :git => 'git://github.com/jnicklas/capybara.git'

## Questions

According to a comment for the screencast, these gems have to be included not only in the :test environment, but also in the :development environment.

Apparently, these gems have their own command line tools/generators, that need to be accessed.

1. But why?

2. What's the deal with *transactions* versus *truncation*?

3. What inserted the below line into `spec_helper.rb`?

    `require 'rspec/rails'`

## Notes
* Webkit is alternative/predecessor to **[Capybara][2]**.
* **RSpec request specs** alternative to Rails default integration tests.
* **Capybara** offers richer testing experience, enables JavaScript testing.

##### Transaction versus Truncation

Transactional fixtures involve a setup and teardown for each test when using relational databases.

The way Selenium works, it doesn't like this arrangement.

So we disable the transactional fixtures.

This means there is no clean up between tests.

Suboptimal.

There is a different tool called `database_cleaner` that helps with this problem: It clears out the database between specs.

**Truncation** removes all data from database.  **Transaction** rolls back all changes made by the current spec.


[1]: http://railscasts.com/episodes/257-request-specs-and-capybara
[2]: https://github.com/jnicklas/capybara



