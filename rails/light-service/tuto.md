<h3> How to create service with light-service ? </h3>

Add gem 'light-service', '~> 0.6.1'
https://github.com/adomokos/light-service <br>
To Gemfile <br>
Run `bundle` <br>

Create a `services` folder in `app/`
In this folder I can create files or subfolder (like in all ruby projects). <br>

First I have to create an organizer, who will call all the action need : <br>
To create an organizer, I have to include `extend LightService::Organizer` <br>

To create an action, I have to include `extend LightService::Action` <br>

Here is a good example : https://github.com/adomokos/light-service/wiki/A-Simple-Example
<br>


In the organiser i have to define a class methode `self.post` for example :

```ruby
def self.to_post(:user, :project, params: {})
  with(user: user, project: project, params: params)
  .reduce [
    Contributions::SaveContributionAction
    # Other action
    # Other action
  ]
end
```
<br>
This method need argument. <br>

To call it in an controller i have to do :
```ruby
  service = Contributions::CreateContribution.to_post(user, project, params)
  # Where Contributions is the name of my module and CreateContribution the name of my class
```

This is calling my organizer, wich is calling all the actions, for example :

```ruby
module Contributions
  class CreateContribution
    extend LightService::Organizer

    def self.to_post(user, project, params = {})
      with(user: user, project: project, params: params)
      .reduce [
        Contributions::SaveContributionAction
      ]
    end
  end
end
```

And my action looks like :
```ruby
module Contributions
  class SaveContributionAction
    extend LightService::Action
    expects :user, :params

    executed do |context|
      user    = context.user
      params  = context.params
      project = Reward::Project.friendly.find(context[:project])
      prize  = project.prizes.find(params[:reward_prize_id])

      # find the amount of contribution (take the amount of prize if present)
      amount =
        if prize.present?
          prize.amount
        else
          params[:amount].to_i
        end
      contribution = Reward::Contribution.new(
                      user: user,
                      project: project,
                      amount: amount,
                      prize: prize
                    )
        if contribution.save
          context[:contribution] = contribution
        end
    end
  end
end
```

<h4>How to test with light service ?</h4>

Create a folder `spec/services` <br>
In this folder create a file for each of your `action` service (at the end of the name add `_spec.rb`)
The header of your file will look like :
```ruby
require 'rails_helper'
RSpec.describe Contributions::SaveContributionAction, type: :service do

end
```

The subject of the test :
```ruby
subject do
  described_class.execute(user: user,
    project: project,
    prize:   prize,
    params:  params
  )
end
```

And after you can create classic test, one example of a complete test file (wich can for sure be improve) <br>
```ruby
require 'rails_helper'

RSpec.describe Contributions::SaveContributionAction, type: :service do
  subject do
    described_class.execute(user: user,
      project: project,
      params:  params
    )
  end
  let(:user) { VCR.use_cassette('reward/user') { FactoryGirl.create(:user) } }
  let(:project) do
    VCR.use_cassette('reward/project_model') do
      FactoryGirl.create(:project,
        project_info_attributes: FactoryGirl.attributes_for(:project_info),
        user: user,
        state: 'collecting'
      )
    end
  end
  let(:prize) do
    FactoryGirl.create(:prize, project: project, amount: 45, quantity: quantity)
  end
  let(:quantity) { rand(5..10) }
  let(:params) { { reward_prize_id: put_a_prize, amount: amount } }

  describe 'test with price' do
    let(:amount) { nil }
    let(:put_a_prize) { prize }

    it 'will success' do
      expect(subject).to be_success
    end

    it 'will take price amount' do
      expect(subject[:contribution].amount).to eq(45)
    end

    it 'will save contribution' do
      expect { subject }.to change(Reward::Contribution, :count).by(1)
    end

    it 'will return contribution' do
      expect(subject[:contribution].class).to eq(Reward::Contribution)
    end
  end
  describe 'test without price' do
    let(:amount) { 100 }
    let(:put_a_prize) { nil }

    it 'will success' do
      expect(subject).to be_success
    end

    it 'will take amount' do
      expect(subject[:contribution].amount).to eq(100)
    end

    it 'will save contribution' do
      expect { subject }.to change(Reward::Contribution, :count).by(1)
    end

    it 'will return contribution' do
      expect(subject[:contribution].class).to eq(Reward::Contribution)
    end
  end
end

```
