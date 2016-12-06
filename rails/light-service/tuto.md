Add gem 'light-service', '~> 0.6.1'
To Gemfile <br>
Run `bundle` <br>
https://github.com/adomokos/light-service <br>

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
