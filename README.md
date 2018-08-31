# Ryanair Dashboard
Dashboard to monitor and receive alerts for changes in Ryanair fare prices.

g![image](https://github.com/pedro-c/ryanair-dashboard/blob/master/screenshot.png?raw=true)

### This is a fork
All credit goes to [ezekg](https://github.com/ezekg) for creating this, and [gilby125](https://github.com/gilby125/) for adapting to Ryanair.
My contributio resumes to adding the possibility to add one way trips and fixing the code to comply with recente API changes.

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

```
ryanair --from DUB --to BVA --leave-date 2019-02-02 --return-date 2019-02-09 --passengers 2  --interval 1
```

or

```
node index.js --from DUB --to BVA --leave-date 2019-02-02 --return-date 2019-02-09 --passengers 2  --interval 1
```