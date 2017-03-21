# tweet-today

Send an email of a twitter list's tweets every day.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Deployment

This isn't quite a one click deploy. After you deploy to heroku (by clicking the button above), you will need to configure the following environment variables in Heroku by either using the `heroku config:add` command or the web ui.

 - `EMAIL_FROM_ADDRESS='Tweet Today <from@example.com>'` This will need to be configured in the postmark ui. You can see that by either clicking the link in the web ui or running `heroku addons:open postmark`. You can either configure a whole domain or just a single address.
 - `EMAIL_TO_ADDRESS='Your Name <you@example.com>'` This is where you want the email to go.
 - `TWITTER_KEY=blob` You get this from twitter at https://apps.twitter.com/app/new
 - `TWITTER_SECRET=blob` You get this from twitter at https://apps.twitter.com/app/new
 - `TWITTER_LIST=icco/short-list` This should be a public list on Twitter in the form of username/listname.

 Once all of these are set, open the scheduler ui (either on the web or with `heroku addons:open scheduler`) and set a daily script to run at the end of the day UTC with the command of `rake`.

 After that, it should just work. You can run `heroku run rake` to test if everything is configured right.
