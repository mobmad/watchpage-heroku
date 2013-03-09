# Watchpage heroku

Has a website changed? This small shell script will poll for changes and notify you when they occur.

## Configurting SMS notifications

Register for a [Twilio](https://www.twilio.com) account to receive SMS/Call notifications. The `TWILIO_FROM` variable used 
below must be a number you can use for sending SMSes and must be configured in your account. `TWILIO_TO` may be any number
your application has access to send to.

## Installing on heroku

1. `curl -OL https://github.com/mobmad/watchpage-heroku/archive/master.zip > watchpage-heroku.zip && unzip watchpage-heroku.zip`
2. Modify `Procfile` to send the URL of the site you wish to monitor
3. `git commit -am "Changed URL"`
4. Run `heroku create --buildpack http://github.com/ryandotsmith/null-buildpack.git`
5. `git push heroku master`
6. `heroku config:set SLEEP_MINS=12 TWILIO_ACCOUNT_SID=<insert> TWILIO_TOKEN=<insert> TWILIO_FROM=<+1234567890> TWILIO_TO=<+4712345678>` this will poll every 12 minute and configure SMS notifications
7. `heroku ps:scale worker=1` (can be stopped by setting worker to 0, check status with `heroku ps`)
8. `heroku logs` (to verify it is running. It should say Fetched inital version, then Fetch current version)

# Credit
Inspired from [http://github.com/ryandotsmith/null-buildpack](http://github.com/ryandotsmith/null-buildpack)