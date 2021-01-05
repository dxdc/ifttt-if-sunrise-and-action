# IFTT if-sunrise-and-action
[![mit license](https://badgen.net/badge/license/MIT/red)](https://github.com/dxdc/ifttt-if-sunrise-and-action/blob/master/LICENSE)
[![Donate](https://badgen.net/badge/Donate/PayPal/91BE09)](https://paypal.me/ddcaspi)

IFTTT applet to use sunset/sunrise times as an additional if/then trigger for when to run an action. You can [enable the original IFTTT applet here](https://ifttt.com/applets/KUEbxRCB-turn-on-wemo-light-switch-if-motion-detected-after-sunset-and-before-sunrise-dallas-tx). Note that this is hardcoded for Dallas, TX.

<img src="/images/main.png?raw=true" width="250" alt="Applet Preview">

## Description

IFTTT made **filter code** a premium feature, so for the benefit of the IFTTT community, I am open sourcing the code for this applet.

You can easily adapt this template to limit any typical IFTTT action to happen between any hour *and/or* supported SunCalc time: `['sunrise', 'sunset', 'sunriseEnd', 'sunsetStart', 'dawn', 'dusk', 'nauticalDawn', 'nauticalDusk', 'nightEnd', 'night', 'goldenHourEnd', 'goldenHour']`

After installation, two parts should be customized:
1. GPS coordinates for your location
> An adapted version of [SunCalc](https://github.com/mourner/suncalc) is used to calculate sunset/sunrise times based on the GPS coordinates
```js
// Customize for your geolocation
// Dallas, TX
var myLatitude = 32.8;
var myLongitude = -96.8;
```
2. IFTTT-based code for when to skip the action. For example, in this code, the action is skipped during daylight hours.

```js
var todaySunlight = <any>SunCalc.getTimes(triggerDate, myLatitude, myLongitude);

// Customize with getTimes properties, sunrise, dusk, etc.
if (triggerDate > todaySunlight.sunrise && triggerDate < todaySunlight.sunset) {
  WemoLightSwitch.attributeLsOnDiscrete.skip("Skipped during sunlight hours");
}
```

3. If you want to use specific hours (i.e., not sunset/sunrise), here is an example of how that can be done. The `filter.js` code can be adapted to support any variation/combination you'd like.

```js
var currentHour = Meta.currentUserTime.hour();
if (currentHour >= 20 || currentHour < 5 ) {
 // add action here
}
```

## Notes

- I experienced poor latency (up to 10 minutes in some cases) from IFTTT with this applet and moved on to other services
- There is a small bug in the Suncalc library, affecting the time calculation in some instances having to do with daylight savings time. This may be fixed here: https://github.com/dxdc/ifttt-if-sunrise-and-action/commit/a01d9eda5ebaf05edf325d89f8845c6340d5fc6c.
> See: https://github.com/mourner/suncalc/issues/107

## Installation

1. Create a new private applet on [IFTTT Platform](https://platform.ifttt.com/).
<img src="/images/private.png?raw=true" width="750" alt="Private">

2. Set up a new applet using this guideline.
<img src="/images/applet.png?raw=true" width="750" alt="Customize">

3. Copy the raw [TypeScript code](/filter.js?raw=true) into the `Filter code` box, and make any needed changes (see [Description](#description)).

4. Publish and test!

## :question: Get Help

For bug reports and feature requests, open issues. :bug:

## How to contribute

Have an idea? Found a bug? Contributions and pull requests are welcome.

## Support my projects

I try to reply to everyone needing help using these projects. Obviously, this takes time. However, if you get some profit from this or just want to encourage me to continue creating stuff, there are few ways you can do it:

-   Starring and sharing the projects you like :rocket:
-   [![PayPal][badge_paypal]][paypal-donations] **PayPal**— You can make one-time donations via PayPal.
-   **Venmo**— You can make one-time donations via Venmo.
    ![Venmo QR Code](/images/venmo.png?raw=true 'Venmo QR Code')
-   **Bitcoin**— You can send me Bitcoin at this address: `33sT6xw3tZWAdP2oL4ygbH5TVpVMfk9VW7`

## MIT License

[badge_paypal]: https://img.shields.io/badge/Donate-PayPal-blue.svg
[paypal-donations]: https://paypal.me/ddcaspi
