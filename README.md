# Arduino Workshop

Workshop Arduino Introduction for 2 Devine, Fall 2019.

In this workshop we'll cover the basics of Arduino, and move to Johnny-Five with Electron to build interactive applications combining computer and Arduino interaction.

## What's Arduino?

Arduino is a collection of 3 tools, forming the Arduino Toolkit. First of all, there's the Arduino controller (hardware), which is available in many formats. The schematics are open source, and everybody can assemble them on their own if wanted. The second part of the Arduino Toolkit is the language and compiler. These enable you to write programs to execute by the controller. Lastly, we've got the Arduino IDE: the coding environment where you can write an Arduino program and upload to a controller.

The goal of Arduino is to enable people to easily build interactive installations, linking hardware and software together. You can read input from different kinds of sensors (push buttons, light sensors, temperature sensors, gyroscopes, distance sensors, ...) and control other electronics (leds, motors, ...)

## Installation and setup

You'll need an Arduino compatible board. You can check a list of supported boards at https://www.arduino.cc/en/Main/Products. We will be using the Arduino UNO board during this course. Next to a board, you'll need some LEDs, resistors, sensors, ...

Next to a board, you'll need the Arduino IDE. Download the Arduino IDE at https://www.arduino.cc/en/Main/Software

### Hello Arduino

We'll do a first quick test of your Arduino board and the IDE. Open op the Arduino IDE. You'll be presented with a screen like the image below:

![Image of Arduino IDE](images/arduino-ide.png)

It consists of a large text area where you'll write your code, a button bar on top, a logging area below and a status bar.

We will try to run the "Blink" example on the Arduino board.

1. Open up `File > Examples > 01.Basics > Blink`. The code opens in the IDE.
2. Connect your Arduino Board on a free USB port.
3. Make sure that `Tools > Board > Arduino/Genuino Uno` is selected in the menu.
4. Make sure that a port with an Arduino is selected in `Tools > Port`. The usb port with the arduino should mention something like (Arduino/Genuino Uno) at the end.
5. Click on the right-pointing arrow button to upload the Sketch to the board.

If everything works as it should, you should see the onboard LED blink on the board!

![Image of blinking LED on Arduino](images/arduino-blink.gif)

Take a look at the code from this example. The language you write Arduino code in is the C programming language. We can identify a couple of parts in this Sketch:

- `void setup()`: The code in this function only runs once at startup of the program
- `void draw()`: The code in this function runs continuously. As soon as this function exits, it runs again. You can interpret this as some sort of endless loop.
- `pinMode`: By using this function you can configure a certain pin on the arduino as either OUTPUT or INPUT. An output pin is used to drive an external component, whereas an INPUT pin is used to read a value from a pin (eg to read a sensor value).
- `digitalWrite`: We use this function to write a binary value (HIGH or LOW) to a given pin number.
- `delay`: This function pauses the execution of the program for a given amount of time in milliseconds.

## A first electrical circuit

Let's spice things up a little bit. Instead of blinking the on board LED, we'll connect a real LED to the Arduino.

To make an LED light up, it'll need electricity running through it. That electric current needs to flow from somewhere to a destination. Just like water in a rivier, it will flow from a high potential to a low potential. You'll need to be careful about the amount of current flowing through the LED at any given time. Just like with the river analogy, too much current / pressure might destroy a component (our LED). We will add a resistor to our circuit, to limit the current.

We've used a couple of terms in the paragraph above, which are expressed in different units:

- Voltage (V): the difference between the high and low potential in a circuit, expressed in Volts.
- Current (I): the amount of current flowing in a circuit, expressed in Ampere.
- Resistance (R): a resistance in a circuit, expressed in Ohms.

There is a close connection between these 3, expressed in Ohm's law. As you can read on the [Wikipedia page on Ohm's law](https://en.wikipedia.org/wiki/Ohm's_law): "Ohm's law states that the current through a conductor between two points is directly proportional to the voltage across the two points."

![I = V / R](http://www.sciweavers.org/tex2img.php?eq=I%20%3D%20%5Cfrac%7BV%7D%7BR%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0)

Where I stands for current, V for voltage and R for resistance.

### An LED in a circuit

When we want to connect an LED to an Arduino, the schematic would look something like this:

![schematic view of led connected to an Arduino](images/led-basic-wires-schematic.png)

The same schematic looks like this in an illustrated preview:

![preview of led connected to an Arduino](images/led-basic-wires-preview.png)

We could get our hands dirty with a soldering iron, and melt wires and components together, but this would be a pretty slow prototyping / testing proces. Instead of that, we'll use a breadboard.

Breadboards make it easier to prototype electronic circuits. Make sure to [read the chapter "Anatomy of a breadboard" on the Sparkfun website](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard/#anatomy-of-a-breadboard) before continuing.

Build the circuit below using a breadboard and test the Blink example again. The LED should turn on and off.

![preview of the led with breadboard wiring](images/led-basic-breadboard-preview.png)

If it doesn't, check the following potential issues:

- The long connector from the LED should be connected to pin 13.
- The resistor should have a value below 1000 Ohms (1 KOhm). Resistance values can be read using the colored stripes on them (see [resistor-calculator.com](http://www.resistor-calculator.com/)). The one in the picture is a 220 Ohm resistor.

## Arduino and Javascript

Until now, we've been writing C code to program our Arduino. Wouldn't it be fun to combine our Arduino sensors and outputs with our javascript frontends?

Using [Electron](https://electronjs.org) and [Johnny-Five](http://johnny-five.io) we can do just that! We'll upload a generic sketch (Firmata) to the Arduino and give it instructions over USB from within our Javascript code.

### Electron

First of all, we'll run our javascript inside an Electron App. Basically, Electron is a mashup of nodejs and the Chromium rendering engine. You'll run you webpages (including javascript) locally, as a desktop application. This enables you to do everything you could do in a nodejs application but from within your frontend javascript code: access the filesystem, interact with hardware, talk to C++ extensions, ...

You're probably already using a couple of Desktop apps built using Electron: Visual Studio Code, Atom, Slack, Hyper, Github Desktop... are all built on top of Electron.

An easy way to get started is by using the [electron-webpack-quick-start project](https://webpack.electron.build/) as a starter project. 

```bash
$ git clone https://github.com/electron-userland/electron-webpack-quick-start p01-blink
$ cd p01-blink
$ npm install
```

Test launching the project, by running the dev script:

```bash
$ npm run dev
```

An electron window should open.

### Johnny Five

Johnny Five is a library which enables you to talk to electronics from Javascript. It uses a standard protocol, called Firmata, to send instructions to the Arduino over the USB connection. An instruction could be turn on an LED, read a sensor value, move a servo, ...

First of all, you'll need an Arduino with the StandardFirmata sketch. Upload this Sketch (`File > Examples > Firmata > StandardFirmata`) to your Arduino UNO.

Next to that, make sure you've got node-gyp installed globally: `npm install -g node-gyp`

#### Hello Johnny Five

We'll expand the electron-quick-start project to control an Arduino using Johnny five.

Add Johnny-five to the project:

```bash
npm install johnny-five
```

Add the necessary code to `src/renderer/index.js` to talk to the board from within the renderer javascript:

```javascript
const five = require('johnny-five');
const board = new five.Board({
  repl: false
});

board.on("ready", () => {
  const led = new five.Led(13);
  led.blink(500);
});
```

Test the application using `npm start`. You won't see anything happen. If you open the devtools, there's probably an error message there:

> Uncaught Error: The module 'projects/p01-blink/node_modules/@serialport/bindings/build/Release/bindings.node' was compiled against a different Node.js version using NODE_MODULE_VERSION 67. This version of Node.js requires NODE_MODULE_VERSION 73. Please try  re-compiling or re-installing the module (for instance, using `npm rebuild` or `npm install`).

Johnny five uses a module called serialport, which uses some C++ code to send messages over USB. This C++ code is compiled for nodejs by default, but we're using an electron flavor of node! We'll need to make sure this C++ code is compatible with Electron.

We'll downgrade our electron version to version 4. Change the version in your package.json file:

```
"electron": "4.2.12"
```

Add an extra script to the scripts section of your package.json:

```
"rebuild-deps": "electron-builder install-app-deps"
```

Remove the node_modules, run npm install again _and_ run the rebuild-deps script:

```bash
$ rm -rf node_modules
$ npm install
$ npm run rebuild-deps
```

Launch the app again. You should see the onboard LED blink! Once this works, try connecting a real LED again.

### Components to test

You've got a lot of different components in your kit, which you can use with johnny five. Build and test the following examples:

#### Inputs

- Button: http://johnny-five.io/examples/button/
- Potentiometer: http://johnny-five.io/examples/potentiometer/ - try this out with the joystick from your kit as well! The joystick is a combination of 2 potentionmeters and one push button.
- Photoresistor: http://johnny-five.io/examples/photoresistor/. You can use this to create a laser-tripwire (http://johnny-five.io/examples/laser-trip-wire/), ask the professor for a laser diode to play with.
- Ultrasonic Sensor: http://johnny-five.io/examples/proximity-hcsr04/ (note: you'll need to upload a different firmata sketch first)

#### Outputs

- Piezzo sound: http://johnny-five.io/examples/piezo/
- DC motor: http://johnny-five.io/examples/motor/
- Servo motor(ask the professor for a servo to play with): http://johnny-five.io/examples/servo/

### Modifying the html

You'll read the following on https://webpack.electron.build/development.html#use-of-html-webpack-plugin

> You might notice that you don’t need an index.html to get started on your application. That’s because it is created for you, as it adds in a few extra configurations needed for the electron environment. If you are creating an electron application with webpack, you are most likely creating a Single Page Application anyways. So because of that, there is already a <div id="app"></div> provided in the markup that you can mount your application onto.

If you do want to have a custom html, we'll need to extend the  webpack config of our project. Add a config object for electron webpack to our package.json (see docs at https://webpack.electron.build/configuration for more info), containing the path to your html file:

```
"electronWebpack": {
  "renderer": {
    "template": "src/renderer/index.html"
  }
}
```

Put an html file in that location and test the application. You should see your custom html.

### Combinations

Use your imagination to combine inputs with outputs. Sound a piezzo alarm when the laser tripwire gets triggered. Control a servo angle based on the distance of your ultrasonic sensor. Use the joystick as a speed controller for your DC motor.

Because you are in javascript land, you can combine the hardware with browser logic. Make sure to try the following projects:

1. Fade an LED using an `<input type="range">`
2. Change a servo angle based on the x position of your mouse pointer on the screen
3. Control the background color of your `<body>` tag using a potentiometer or a light sensor
4. Build a basic arcade game (pong, breakout) and control it using an Arduino sensor

## Other things to check

- [View Arduino, The documentary](https://vimeo.com/18539129) (28 min)
- [Star Wars Imperial March with Floppy and Arduino](https://www.youtube.com/watch?v=B_Q6jMUdfYc)
- [The breakfast machine](https://www.youtube.com/watch?v=E2evC2xTNWg)
- [Lunar Trails](https://vimeo.com/54043239)
- [NES Zapper Gun hack](https://vimeo.com/181220188)
- [NIghtwriter Nyx](http://www.gijsvanbon.nl/nyx1.html)
- [Whiteboard Clock](https://www.youtube.com/watch?v=4QgeQAiSmM8)
- [Neil Mendoza](http://www.neilmendoza.com/)
- [Anouk Wipprecht](http://www.anoukwipprecht.nl/gallery)