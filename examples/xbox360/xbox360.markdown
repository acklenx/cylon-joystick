# Joystick - Xbox 360 Controller

For this Cylon example, we'll demonstrate how to get input from a wired Xbox 360
controller.

Before we get started, make sure you've got `cylon-joystick` installed via NPM
so we can connect to the controller.

First, let's load up Cylon:

    var Cylon = require('cylon');

With that done, we can start defining our new robot.

    Cylon.robot({

We'll be setting up a connection to our 360 controller, and also setting the
controller up as a device the robot can talk to:

      connection: { name: 'joystick', adaptor: 'joystick', controller: 'xbox360' },
      device: { name: 'controller', driver: 'xbox360' },

With our connection to the controller established, we'll tell it what to do:

      work: function(my) {

Let's ask our robot to tell us when the face buttons on the controller (A, B, X,
Y) are pressed and released:

        ["a", "b", "x", "y"].forEach(function(button) {
          my.controller.on(button + ":press", function() {
            console.log("Button " + button + " pressed.");
          });

          my.controller.on(button + ":release", function() {
            console.log("Button " + button + " released.");
          });
        });

Next up, when someone moves the left and right analog sticks, lets' print their
current positions.

Since the Xbox 360 controller's adaptor emits the `left:move`
and `right:move` events even when the sticks aren't moved, we'll prevent it from
emitting duplicate events, too:

        my.controller.on("left:move", function(pos) {
          var last = lastPosition.left;
          if (!(pos.x === last.x && pos.y === last.y)) {
            console.log("Left Stick:", pos);
          }
        });

        my.controller.on("right:move", function(pos) {
          var last = lastPosition.right;
          if (!(pos.x === last.x && pos.y === last.y)) {
            console.log("Right Stick:", pos);
          }
        });
      }
    });

And with our work defined, we can tell Cylon to start up our Robot:

    Cylon.start();
