source 'https://rubygems.org/'

gem 'rails', '4.1.4'

### OpenShift Online changes:

# Fix the conflict with the system 'rake':
gem 'rake', '~> 0.9.6'

# Support for databases and environment.
# Use 'sqlite3' for testing and development and mysql and postgresql
# for production.
#
# To speed up the 'git push' process you can exclude gems from bundle install:
# For example, if you use rails + mysql, you can:
#
# $ rhc env set BUNDLE_WITHOUT="development test postgresql"
#
group :development, :test do
  gem 'sqlite3'
  gem 'minitest'
  gem 'thor'
  gem 'thin'
end

# Add support for the MySQL
group :production, :mysql do
  gem 'mysql2'
end

#group :production, :postgresql do
#  gem 'pg'
#end

### / OpenShift changes

# Use SCSS for stylesheets
gem 'sass-rails', '~> 4.0.3'
# Use Uglifier as compressor for JavaScript assets
gem 'uglifier', '>= 1.3.0'
# Use CoffeeScript for .js.coffee assets and views
gem 'coffee-rails', '~> 4.0.0'
# See https://github.com/sstephenson/execjs#readme for more supported runtimes
gem 'therubyracer',  platforms: :ruby

# Use jquery as the JavaScript library
gem 'jquery-rails'
# Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
gem 'turbolinks'
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem 'jbuilder', '~> 2.0'
# bundle exec rake doc:rails generates the API under doc/api.
gem 'sdoc', '~> 0.4.0',          group: :doc

# Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
gem 'spring',        group: :development

# Use ActiveModel has_secure_password
# gem 'bcrypt', '~> 3.1.7'

# Use unicorn as the app server
# gem 'unicorn'

# Use Capistrano for deployment
# gem 'capistrano-rails', group: :development

# Use debugger
# gem 'debugger', group: [:development, :test]

#Use kaminari page
gem 'kaminari'

#Use settingslogic
gem 'settingslogic'

# Use haml template
gem 'haml-rails'

# Use bootstrap css
gem 'bootstrap-sass'
gem 'font-awesome-sass-rails', '~> 3.0.2.2'
gem 'twitter-bootstrap-rails', '~> 3.2.0'
#gem 'twitter-bootstrap-rails-confirm', '~> 3.2.0'
gem 'twitter-bootstrap-rails-confirm', github: 'fxhover/twitter-bootstrap-rails-confirm', branch: 'bootstrap3'
# Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
gem 'turbolinks'
# Use ActiveModel has_secure_password
gem 'bcrypt', '~> 3.1.7'
# Use unicorn as the app server
gem 'unicorn'

#Avatar
gem 'gravatar_image_tag'

#markdown
gem 'markdown-toolbar', github: 'fxhover/markdown-toolbar'
gem 'redcarpet'

#file upload
gem 'simple_fileupload'
gem 'remotipart'