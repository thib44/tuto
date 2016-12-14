<h3>Test the integration of the website</h3>

Gem needed :
capybara
https://github.com/teamcapybara/capybara
<br>

Add to gemfile :
`gem 'capybara'` <br>
And run bundle install

Add `require 'capybara/rspec'` to `spec_helper.rb` file <br>

>  put your Capybara specs in spec/features (only works if you have it configured in RSpec)

Then create file in features, folders, with the name of the process.
For example : `sign_in_process_spec.rb`. Wich will look like :
```ruby
describe "the signin process", :type => :feature do
  before :each do
    User.make(email: 'user@example.com', password: 'password')
  end

  it "signs me in" do
    visit '/sessions/new'
    within("#session") do
      fill_in 'Email', with: 'user@example.com'
      fill_in 'Password', with: 'password'
    end
    click_button 'Sign in'
    expect(page).to have_content 'Success'
  end
end
```
