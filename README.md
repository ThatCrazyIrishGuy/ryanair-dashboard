# Ryanair Dashboard
Dashboard to monitor and receive alerts for changes in Ryanair fare prices.

![image](https://cloud.githubusercontent.com/assets/6979737/17744714/99f15da2-646e-11e6-8f13-60c716f1e865.png)

# This is a fork
All credit goes to [ezekg](https://github.com/ezekg) for creating this, i mearly removed the crawler abd adapted it for use with Ryanair.

## Why?
I'm a lazy programmer who was tired of checking flight prices … and I really wanted
to try out Twilio and [blessed](https://github.com/chjj/blessed/). ¯\\\_(ツ)\_/¯

## Installation
Since I would rather not get in trouble for publishing this tool to npm, you can
clone the repo locally and use `npm link` to use the executable.
```
cd wherever-you-cloned-it-to
npm link
```

If you recieve a ``SyntaxError: Unexpected token ...`` upon running the `ryanair` command, make sure you are running a version of node that supports ES6 syntax (5.11.0 and up). 

Under some circumstances, libxmljs may throw an error that looks like this:
```
Error: Could not locate the bindings file. Tried:
 → /root/ryanair-dashboard/node_modules/libxmljs/build/xmljs.node
 ```
You can fix it and run `ryanair` successfully by rebuilding libxmljs manually:
```
sudo npm install -g node-gyp
cd node_modules/libxmljs
node-gyp rebuild
```

## Usage
It will make an api request for Ryanair's prices every `n` minutes (`n` = whatever interval you
define via the `--interval` flag) and compare the results, letting you know the
difference in price since the last interval. The default interval is 30 mins.

You may optionally set the `--individual-deal-price` flag, which will alert you
if either fare price falls below the threshold you define. There is also the
optional `--total-deal-price` flag, which will alert you if the combined total
of both fares falls below the threshold. Other than `--interval` and the
Twilio-related options, all other flags are required.

```bash
ryanair \
  --from 'DUB' \
  --to 'BVA' \
  --leave-date 2017-02-02 \
  --return-date 2017-02-09 \
  --passengers 2 \
  --individual-deal-price 20 \ # In Euro (optional)
  --total-deal-price 40 \ # In Euro (optional)
  --interval 1 # In minutes (optional)
```

### Twilio integration
If you have a Twilio account (I'm using a free trial account) and you've set up
a deal price threshold, you can set the following environment vars to set up SMS
deal alerts. _Just be warned: as long as the deal threshold is met, you're going
to receive SMS messages at the rate of the interval you defined. Better wake up
and book those tickets!_

```bash
export TWILIO_ACCOUNT_SID=""
export TWILIO_AUTH_TOKEN=""
export TWILIO_PHONE_FROM=""
export TWILIO_PHONE_TO=""
```
