<h1>For testing with api</h1>
<p>
  We need to save the call and the reponse of api, like that we don't call the api
  for each test. <br>
  For that we use the `gem vcr` : https://github.com/vcr/vcr <br>
  Wich also need the `gem 'webmock'` <br>
  Wee need to add this two gem in the group `test` of your gemfile
  Then run `bundle`
</p>

> In your test folder add a `support` folder <br>
> Inside, create a `vcr.rb` file


```ruby
  # vcr.rb
  VCR.configure do |config|
    config.before_record do |i|
      i.response.body.force_encoding('UTF-8')
    end
    config.cassette_library_dir = 'fixtures/vcr_cassettes'

    config.hook_into :webmock
    config.configure_rspec_metadata!
    config.ignore_localhost = true
  end
```

Then add this line in `rails_helper.rb` : `Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }`

Then when you need a call to the api you have to do something like : <br>

`let(:user) { VCR.use_cassette('user/create_user') { create(:user) } }`


And that's ok :-)
