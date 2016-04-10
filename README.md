# bunto-heroku
> Makes a Bunto site deployable on Heroku.

[![Deploy on Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## Installation
> Here are the step by step instructions!

Add a `Gemfile` in the Bunto project root containing:

    source 'https://rubygems.org'
    ruby '2.1.2'
    gem 'bunto', '>= 1.0'
    gem 'kramdown'
    gem 'rack-bunto'
    gem 'rake'
    gem 'puma'

Run: `bundler install`.
Create a Procfile telling Heroku how to serve the web site with Puma:

    web: bundle exec puma -t 8:32 -w 3 -p $PORT

Create a Rakefile which tells Heroku’s slug compiler to build the Bunto site as part of the `assets:precompile` Rake task:

    namespace :assets do
      task :precompile do
        puts `bundle exec bunto build`
      end
    end

Add the following lines to the `_config.yml` file:

    gems: ['kramdown']
    exclude: ['config.ru', 'Gemfile', 'Gemfile.lock', 'vendor', 'Procfile', 'Rakefile']

Add a `config.ru` file containing:

    require 'rack/bunto'
    require 'yaml'
    run Rack::Bunto.new

That is it! When you do the usual `git push heroku master` deployment, the standard Ruby Buildpack will kick off the Bunto compiler and when your app runs, Puma will serve the static assets.

To run Bunto locally using the dependencies in the project, run:

    bundle exec bunto serve --watch

Let me know how it goes.
