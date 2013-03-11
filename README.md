# Watchpage heroku

Has a website changed? The provided shell script deploys on heroku and will poll for changes and notify you when they occur. Currently notifications via SendGrid (mail) and Twilio (SMS) are supported, but you could easily add custom ones by modifying the script.

## Installing on heroku

1. [Download](https://github.com/mobmad/watchpage-heroku/archive/master.zip) and unzip archive
2. `git init && git add . && git commit -m "Created watcher"`
3. Run `heroku create --buildpack http://github.com/ryandotsmith/null-buildpack.git`
4. `git push heroku master`
5. Configure the site you want to watch and the polling interval:

		heroku config:set \
		URL=http://inserturltoyoursitehere.com \
		SLEEP_MINS=12

6. (Optional) Configure SMS notifications. Register for a [Twilio](https://www.twilio.com) account to receive SMS/Call notifications. The `TWILIO_FROM` variable used below must be a number you can use for sending SMS and must be configured in your account. `SMS_RECIPIENTS` should be a space separated string with numbers your application has access to send to.

		heroku config:set \
		TWILIO_ACCOUNT_SID=<insert> \
		TWILIO_TOKEN=<insert> \
		TWILIO_FROM=<insert> \
		SMS_RECIPIENTS="+4712345678 +47987654321"

7. (Optional) Configure Call notifications. In addition to registering on Twilio as described above, you must request access to call the desired countries. Test via Twilio's pages first, then configure notifications with: 

		heroku config:set \
		TWILIO_ACCOUNT_SID=<insert> \
		TWILIO_TOKEN=<insert> \
		TWILIO_FROM=<insert> \
		TWILIO_CALL_RESPONDER=<http://yourdomain.com/someurl/answer.xml> \
		CALLER_RECIPIENTS="+4712345678 +47987654321"

	Where answer is a static/dynamically generated file with the following contents (example):

		<?xml version="1.0" encoding="UTF-8"?>
		<Response>
		    <Say voice="woman">Site has changed</Say>
		</Response>

8. (Optional) Configure email notifications ([more info](https://devcenter.heroku.com/articles/sendgrid) and [implementation details](http://sendgrid.com/docs/API_Reference/Web_API/mail.html))

		heroku addons:add sendgrid:starter
		heroku config:set MAIL_RECIPIENTS="to[]=person1@mail.com&to[]=person2@mail.com"

9. `heroku ps:scale worker=1` (can be stopped by setting worker=0. Check the current status with `heroku ps`)
10. Run `heroku config:set TEST_SEND_ON_START=true`. You should receive a notification if the configuration is working. If it's working, run `heroku config:unset TEST_SEND_ON_START` to avoid receiving notifications after config changes and reboot/deploys.

# Tips
Download [nezumi](http://nezumiapp.com/) from App Store and remote control your app - change settings, view logs, stop/start.

# Credit
Inspired from [http://github.com/ryandotsmith/null-buildpack](http://github.com/ryandotsmith/null-buildpack)
