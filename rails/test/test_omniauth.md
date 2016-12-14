Source :
https://github.com/omniauth/omniauth/wiki/Integration-Testing
https://www.cookieshq.co.uk/posts/how-to-test-facebook-login-using-devise-omniauth-rspec-and-capybara
https://gist.github.com/kinopyo/1338738

I need capybara gem <br>

Create a `spec/support/omniauth.rb` file.
Wich will look like that :
```ruby
module Omniauth
  def set_mock_auth
    OmniAuth.config.mock_auth[:facebook] = OmniAuth::AuthHash.new({
      provider:   'facebook',
      uid:        '123545',
      email:      Faker::Internet.email,
      first_name: Faker::Name.first_name,
      last_name:  Faker::Name.last_name
    })
  end
end
```

> In `spec_helper.rb` add `OmniAuth.config.test_mode = true` <br>
> Add `require 'capybara/rspec'` to `rails_helper` <br>
> And include omniauth file in `rails_helper`  `config.include(Omniauth)` <br>


Finaly a test can look like that :
```ruby
require 'rails_helper'

describe "the sign up process", type: :feature do
  it 'step2 complete sign_up user' do
    visit '/users/sign_up'
    set_mock_auth
    click_link "Se connecter avec Facebook"
    page.should have_content "Finaliser votre inscription"
    within("#new_user") do
      fill_in :user_birth_date, with: Faker::Date.between(100.years.ago, 18.years.ago)
      fill_in :user_user_name, with: Faker::Internet.user_name
      find(:select, 'user_country').first(:option, 'France').select_option
    end
    VCR.use_cassette('user/create_user_form_omniauth') do
      click_button "Create Utilisateur"
    end
    expect(page).to have_content("Bienvenue ! Vous vous êtes enregistré(e) avec succès.")
  end
end
```
