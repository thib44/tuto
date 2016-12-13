<h3>How to log as ?</h3>
 At the top of `config/active_admin.rb` (before `ActiveAdmin.setup do |config|`) add :
 ```ruby
 def authenticate_admin!
  redirect_to new_user_registration_path unless current_user && current_user.admin
end
 ```

And in your user file of admin folder :

```ruby
member_action :sign_in_as, method: :put do
  sign_in resource
  redirect_to root_path
end

action_item :view, only: :show do
  link_to 'Connexion', sign_in_as_admin_user_path(resource), method: :put
end
```
