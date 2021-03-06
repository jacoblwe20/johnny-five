# Servo Diagnostic

Run with:
```bash
node eg/servo-diagnostic.js
```


```javascript
var five = require("johnny-five"),
  args, pins, ranges;

/**
 * This program is useful for manual servo administration.
 *
 * ex. node eg/servo-diagnostic.js [ pin list ]
 *
 *     To setup servos on pins 10 and 11:
 *
 *     node eg/servo-diagnostic.js 10 11
 *
 *     To setup servos on pins 10 and 11 with custom ranges:
 *
 *     node eg/servo-diagnostic.js 10:10:170 11
 *
 *     Note: Ranges default to 0-180
 *
 */

args = process.argv.slice(2);

pins = [];
ranges = [];

args.forEach(function(val) {
  var vals = val.split(":").map(function(v) {
    return +v;
  });

  pins.push(vals[0]);

  ranges.push(
    vals.length === 3 ?
    vals.slice(1) : [0, 180]
  );
});

(new five.Board()).on("ready", function() {
  var servos;

  // With each provided pin number, create a servo instance
  pins.forEach(function(pin, k) {
    new five.Servo({
      pin: pin,
      range: ranges[k]
    });
  }, this);

  servos = new five.Servos();

  servos.center();

  // Inject a Servo Array into the REPL as "s"
  this.repl.inject({
    s: servos
  });
});

```













## Contributing
All contributions must adhere to the [Idiomatic.js Style Guide](https://github.com/rwldrn/idiomatic.js),
by maintaining the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt](https://github.com/cowboy/grunt).

## License
Copyright (c) 2012 Rick Waldron <waldron.rick@gmail.com>
Licensed under the MIT license.
