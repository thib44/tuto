How to use Rspec ?


Gem required :

> `gem 'rspec-rails'` : https://github.com/rspec/rspec-rails  
> `gem 'factory_girl_rails'` : https://github.com/thoughtbot/factory_girl  


Gem optional :
> `gem 'faker'` : https://github.com/stympy/faker <br>
> Pour tester les vues : https://github.com/teamcapybara/capybara <br>

If your test call some API you need this two gem :
> `gem 'vcr', '~> 3.0', '>= 3.0.3'` <br>
> `gem 'webmock'` <br>



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


Get the log of the test : <br>
`tail -f log/test.log`
