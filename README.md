#Kit's Rails Development Environment
###Configuration, settings and other useful bit and pices.


## RVM

## Sublime Text 2





## Bundler
###Removing the need to use "bundle exec" before commands.<br>
First: confirm the most reasont version of rvm with 
<pre><code>rvm get head && rvm reload</code></pre>
then (to change the mode of a particular hook) run:
<pre>chmod +x $rvm_path/hooks/after_cd_bundler</pre>
next run:
<pre>bundle --binstubs=./bundler_stubs</pre>
Finaly add <code>bundler_stubs/</code> to the <code>.gitignore</code> file.
<pre># Ignore bundler config
/.bundle

# Ignore the default SQLite database.
/db/*.sqlite3

# Ignore all logfiles and tempfiles.
/log/*.log
/tmp

# Ignore other unneeded files.
doc/
*.swp
*~
.project
.DS_Store
<code>bundler_stubs/</code></pre>

If you install any new executable re-run <code>bundle install</code> command:
<pre><code>bundle install --binstubs=./bundler_stubs</code></pre>



## Guard
###To install gaurd
First add <code>guard-rspec</code> to the <code>Gemfile</code> in the <code>group :development</code>
<pre><code>source 'https://rubygems.org'

gem 'rails', '3.2.13'

group :development, :test do
  gem 'sqlite3', '1.3.5'
  gem 'rspec-rails', '2.11.0'
  gem 'guard-rspec', '1.2.1'
end

# Gems used only for assets and not required
# in production environments by default.
group :assets do
  gem 'sass-rails',   '3.2.5'
  gem 'coffee-rails', '3.2.2'
  gem 'uglifier', '1.2.3'
end

gem 'jquery-rails', '2.0.2'

group :test do
  gem 'capybara', '1.1.2'
  # System-dependent gems
end

group :production do
  gem 'pg', '0.12.2'
end</code></pre>
Then we have to replace the comment at the end of the <code>test group</code> with some system-dependent gems<br>
OS X
<pre><code># Test gems on Macintosh OS X
group :test do
  gem 'capybara', '1.1.2'
  gem 'rb-fsevent', '0.9.1', :require => false
  gem 'growl', '1.0.3'
end </code></pre>
Next install the gems by running <code>bundle install</code>:
<pre><code>$ bundle install</code></pre>
Then initialize Guard so that it works with RSpec:
<pre>$ bundle exec guard init rspec</pre>
Now edit the resulting <code>Guardfile</code> so that Guard will run the right tests when the integration tests and views are updated<br>
Additions to the default Guardfile.
<pre><code>require 'active_support/core_ext'

guard 'rspec', :version => 2, :all_after_pass => false do
  .
  .
  .
  watch(%r{^app/controllers/(.+)_(controller)\.rb$})  do |m|
    ["spec/routing/#{m[1]}_routing_spec.rb",
     "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb",
     "spec/acceptance/#{m[1]}_spec.rb",
     (m[1][/_pages/] ? "spec/requests/#{m[1]}_spec.rb" : 
                       "spec/requests/#{m[1].singularize}_pages_spec.rb")]
  end
  watch(%r{^app/views/(.+)/}) do |m|
    (m[1][/_pages/] ? "spec/requests/#{m[1]}_spec.rb" : 
                       "spec/requests/#{m[1].singularize}_pages_spec.rb")
  end
  .
  .
  .
end</code></pre>
To look like:

<pre><code>require 'active_support/core_ext'

guard 'rspec', :version => 2, :all_after_pass => false do
	watch(%r{^spec/.+_spec\.rb$})
	watch(%r{^lib/(.+)\.rb$})     { |m| "spec/lib/#{m[1]}_spec.rb" }
	watch('spec/spec_helper.rb')  { "spec" }

	# Rails example
	watch(%r{^app/(.+)\.rb$})                           { |m| "spec/#{m[1]}_spec.rb" }
	watch(%r{^app/(.*)(\.erb|\.haml)$})                 { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
	watch(%r{^app/controllers/(.+)_(controller)\.rb$})  { |m| ["spec/routing/#{m[1]}_routing_spec.rb", "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb", "spec/acceptance/#{m[1]}_spec.rb"] }
	watch(%r{^spec/support/(.+)\.rb$})                  { "spec" }
	watch('config/routes.rb')                           { "spec/routing" }
	watch('app/controllers/application_controller.rb')  { "spec/controllers" }

	# Capybara request specs
	watch(%r{^app/views/(.+)/.*\.(erb|haml)$})          { |m| "spec/requests/#{m[1]}_spec.rb" }

	# Turnip features and steps
	watch(%r{^spec/acceptance/(.+)\.feature$})
	watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$})   { |m| Dir[File.join("**/#{m[1]}.feature")][0] || 'spec/acceptance' }

	watch(%r{^app/controllers/(.+)_(controller)\.rb$})  do |m|
		["spec/routing/#{m[1]}_routing_spec.rb",
		 "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb",
		 "spec/acceptance/#{m[1]}_spec.rb",
		 (m[1][/_pages/] ? "spec/requests/#{m[1]}_spec.rb" :
											 "spec/requests/#{m[1].singularize}_pages_spec.rb")]
	end
	watch(%r{^app/views/(.+)/}) do |m|
		(m[1][/_pages/] ? "spec/requests/#{m[1]}_spec.rb" :
											 "spec/requests/#{m[1].singularize}_pages_spec.rb")
	end
end
</code></pre>

Now start guard as follows:
<pre><code>$ bundle exec guard</code></pre>
To eliminate the need to prefix the command with bundle exec, re-follow the steps in <code>Bundle - Remove bundle exec</code>
By the way, if you get a Guard error complaining about the absence of a spec/routing directory, you can fix it by creating an empty one:
<pre><code>$ mkdir spec/routing</code></pre>


## Spork
