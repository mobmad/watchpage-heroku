# Watchpage heroku

Has a website changed? This small shell script will poll for changes and notify you when they occur.

## Installing on heroku

1. [Download](https://github.com/mobmad/watchpage-heroku/archive/master.zip) and unzip archive
2. Modify `Procfile` to send the URL of the site you wish to monitor
3. `git init && git add . && git commit -m "Created watcher"`
4. Run `heroku create --buildpack http://github.com/ryandotsmith/null-buildpack.git`
5. `git push heroku master`
6. Configure URL and polling interval:

		heroku config:set \
		URL=http://inserturltoyoursitehere.com \
		SLEEP_MINS=12

7. Configure SMS notifications. Register for a [Twilio](https://www.twilio.com) account to receive SMS/Call notifications. The `TWILIO_FROM` variable used below must be a number you can use for sending SMSes and must be configured in your account. `SMS_RECIPIENTS` should be a space separated string with numbers your application has access to send to.

		TWILIO_ACCOUNT_SID=<insert> \
		TWILIO_TOKEN=<insert> \
		TWILIO_TO=<+4712345678> \
		SMS_RECIPIENTS="+4712345678 +47987654321"

7. Configure email notifications ([details](http://sendgrid.com/docs/API_Reference/Web_API/mail.html))

		heroku addons:add sendgrid:starter
		heroku config:set MAIL_RECIPIENTS="to[]=person1@mail.com&to[]=person2@mail.com"

8. `heroku ps:scale worker=1` (can be stopped by setting worker to 0, check status with `heroku ps`)
9. Run `heroku config:set TEST_SEND_ON_START=true`. You should receive a notifications if the configuration is working. If it's working, run `heroku config:unset TEST_SEND_ON_START` to avoid receiving notifications after config changes and reboot/deploys.

# Credit
Inspired from [http://github.com/ryandotsmith/null-buildpack](http://github.com/ryandotsmith/null-buildpack)
