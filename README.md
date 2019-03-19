Create Rails and Heroku apps
```
$ gem install rails --version=6.0.0.beta3
$ rails new rails-6-beta-3 -d postgresql
$ heroku create rails-6-beta-3
```

Follow [Heroku's Active Storage](https://devcenter.heroku.com/articles/active-storage-on-heroku) instructions.

Add `amazon` definition to `config/storage.yml`

Add Bucketeer add-on:
```
heroku addons:add bucketeer
```

Set `config.active_storage.service = :amazon` in `config/environments/production.rb`

Add `gem "aws-sdk-s3", require: false` to `Gemfile`
Uncomment `gem 'image_processing', '~> 1.2'` line in `Gemfile`
```
$ bundle install
```

Add buildpack:
```
heroku buildpacks:add -i 1 https://github.com/heroku/heroku-buildpack-activestorage-preview
```

Add action text fields in migration:
```
rails action_text:install
```

Create ActiveRecord scaffold:
```
rails g scaffold message title:string content:text
```

Edit model:
```
# app/models/message.rb
class Message < ApplicationRecord
  has_rich_text :content
end
```

Edit View
```
<%# app/views/messages/_form.html.erb %>
<%= form_with(model: message) do |form| %>
  <div class="field">
    <%= form.label :content %>
    <%= form.rich_text_area :content %>
  </div>
<% end %>
```

Run migrations locally with `rails db:migrate`

Test locally in browser with `rails s`



Commit and push to Heroku
```
$ git commit -a -m "first commit"
$ git push heroku master
```

Run migrations on Heroku with `heroku run rails db:migrate`

Update CORS settings of Bucketeer S3 Bucket to allow uploads:
https://devcenter.heroku.com/articles/bucketeer#updating-cors-settings

View app `heroku open`
