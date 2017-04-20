<h1>How to use bitbucket pipeline</h1>
Follow the bitbucket tutorial. <br>
This config work with ruby 2.4.0 and postgresql
In the file that they give to you put : <br>
```yml
image: sdavies/ruby-postgresql
pipelines:
  default:
    - step:
        script:
          - apt-get update
          - apt-get --assume-yes install nodejs
          - /etc/init.d/postgresql start
          - sudo -u postgres sh -c 'createuser root & createdb ruby'
          - bundle install
          - bundle exec rake db:reset RAILS_ENV=test
          - bundle exec rake db:migrate RAILS_ENV=test
          - bundle exec rspec
```
<br>

The docker image `sdavies/ruby-postgresql` work well, but it need some little configuration in you database.yml file : <br>
```ruby
test:
  <<: *default
  database: account_test
  username: bitbucket
  password: bitbucket
  encoding: SQL_ASCII
```
<br>

And to make it working with your local test, you need to create a `bitbucket` `postgres` user. <br>

> `psql -U postgres`
> `create user username with password 'password';`
> `alter user username superuser;`

<br>
To do next : <br>
Deploy when test ok and in master !
