To use cron with rvm :
First install RVM and add a ruby version.
then run `rvm cron setup` it will setup your crontab file, with for example :
```
PATH="/home/pi/.rvm/gems/ruby-2.5.0/bin:/home/pi/.rvm/gems/ruby-2.5.0@global/bin:/home/pi/.rvm/rubies/ruby-2.5.0/bin:/home/pi/.rvm/gem$
GEM_HOME='/home/pi/.rvm/gems/ruby-2.5.0'
GEM_PATH='/home/pi/.rvm/gems/ruby-2.5.0:/home/pi/.rvm/gems/ruby-2.5.0@global'
MY_RUBY_HOME='/home/pi/.rvm/rubies/ruby-2.5.0'
IRBRC='/home/pi/.rvm/rubies/ruby-2.5.0/.irbrc'
RUBY_VERSION='ruby-2.5.0'
```

Then add your crontab tap : `crontab -e`.
And add a crontab for example :
`*/1 * * * * rvmsudo ruby /home/pi/code/meteo-ruby/meteo.rb` will play the crontab every minutes
