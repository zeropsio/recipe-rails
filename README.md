# Zerops x Ruby on Rails 

A basic [Ruby on Rails](https://rubyonrails.org/) 8 application running on Zerops, utilizing PostgreSQL for database. Perfect starting point for your Rails projects with a simple posts feature.

![rails](https://github.com/zeropsio/recipe-shared-assets/blob/main/covers/svg/cover-ror.svg)

<br/>

## Deploy on Zerops
You can either click the deploy button to deploy directly on Zerops, or manually copy the import yaml to the import dialog in the Zerops app.

[![Deploy on Zerops](https://github.com/zeropsio/recipe-shared-assets/blob/main/deploy-button/green/deploy-button.svg)](https://app.zerops.io/recipe/rails)

<br/>

## Recipe features

- Rails running on a load balanced **Zerops Ruby** service
- Zerops **PostgreSQL 16** service as database
- Configuration for Rails **database migrations** and **seed data**
- Pre-built assets in build phase
- Environment-specific configurations for development, testing, and production
- Logs accessible through Zerops GUI
- Utilization of Zerops built-in **environment variables** system for database configuration

<br/>

## Application features

- Simple Post model with title and content fields
- Ability to create new posts through a form
- Listing all posts on the index page
- Basic validations for post data
- Seed data for initial posts

<br/>

## Production vs. development

Base of the recipe is ready for production, the difference comes down to:

- Use highly available version of the PostgreSQL database (change `mode` from `NON_HA` to `HA` in recipe YAML, `db` service section)
- Use at least two containers for Rails service to achieve high reliability and resilience (add `minContainers: 2` in recipe YAML, `app` service section)

<br/>

## Changes made over the default installation

If you want to modify your existing Rails app to efficiently run on Zerops, these are the general steps we took:

- [ ] Add `zerops.yml` to your repository, our example includes database migrations and seed data
- [ ] Configure the database to use environment variables in `config/database.yml`:
  ```yaml
  default: &default
    adapter: postgresql
    encoding: unicode
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
    host: <%= ENV.fetch("DATABASE_HOST") { "localhost" } %>
    username: <%= ENV.fetch("DATABASE_USERNAME", nil) %>
    password: <%= ENV.fetch("DATABASE_PASSWORD", nil) %>
    port: <%= ENV.fetch("DATABASE_PORT") { 5432 } %>
  ```
- [ ] Configure appropriate headers for reverse proxy load balancer

<br/>

## Local development

1. Clone this repository
2. Install Ruby 3.4.x and Rails 8.x
3. Install PostgreSQL
4. Set up environment variables for database connection
5. Run:
   ```bash
   bundle install
   bin/rails db:migrate db:seed
   bin/rails server
   ```
6. Visit http://localhost:3000 to see your posts application

<br/>
<br/>

Need help setting your project up? Join [Zerops Discord community](https://discord.com/invite/WDvCZ54).