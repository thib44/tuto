How to use Rspec ?


Gem required :

> `gem 'rspec-rails'` : https://github.com/rspec/rspec-rails  
> `gem 'factory_bot_rails'` : https://github.com/thoughtbot/factory_bot_rails  


Gem optional :
> `gem 'faker'` : https://github.com/stympy/faker <br>
> To test views : https://github.com/teamcapybara/capybara <br>

If your test call some API you need this two gem :
> `gem 'vcr', '~> 3.0', '>= 3.0.3'` <br>
> `gem 'webmock'` <br>

Installation :
> Run `rails generate rspec:install` <br>
> In `config/application.rb` add : <br>

```ruby
config.generators do |g|
  g.test_framework :rspec,
    fixtures: true,
    view_specs: false,
    helper_specs: false,
    routing_specs: false,
    controller_specs: true,
    request_specs: false
end
```

Add this line to rails_helper.rb : `config.include FactoryBot::Syntax::Methods`
<br>
To avoid problem with email, add this two lines in `config/environments/test.rb` : <br>
```ruby
  config.action_mailer.delivery_method = :test  # Normaly is already in the file
  config.action_mailer.default_url_options = { host: ENV['HOST'] || "localhost:3000" }
```

Get the log of the test : <br>
`tail -f log/test.log` <br>

Generate a test from console : <br>
> `rails g rspec:model user` <br>
> `rails g rspec:controller users/registrations`

If you are creating without console, take care, the file need to have `_rspec.rb` ad the end



Refactor :

describe : Will describe the action of your group of test <br>
it       : Tell what result is wanted <br>
subject  : Inside a describe is the action make for all the test (it) <br>

Example for a subscribe controller :
```ruby
require 'rails_helper'
require 'faker'

RSpec.describe RegistrationsController, type: :controller do
  describe 'Sign_up User' do
    before(:each) do
      @request.env["devise.mapping"] = Devise.mappings[:user]
    end


    subject do
      post :create, user: attributes_for(:user,
        first_name: first_name,
        last_name:  last_name,
      )
    end

    let(:first_name) { Faker::Name::first_name }
    let(:last_name) { Faker::Name::last_name}

    describe 'with valid params' do
      it 'render root_url with valid params' do
        subject
        expect(response).to redirect_to root_url
      end
    end

  end
end
```

### To easy use factory girl :
Add this line in `rails_helper.rb` :
```
# Include factory bot
config.include FactoryBot::Syntax::Methods

config.before(:suite) do
  FactoryBot.find_definitions
end
```

<h3> How to deal with devise ? </h3> <br>

Add this two lines to `spec/rails_helper.rb` <br>
```ruby
config.include Devise::TestHelpers, type: :controller
config.include Devise::TestHelpers, type: :view
```

Then to sign_in a user you just have to do : `sign_in user` <br>
And to sign_out : `sign_out user`
