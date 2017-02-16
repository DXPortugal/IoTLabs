# Lab 3.5: Node-RED on your Raspberry Pi

### What you will do
Learn basics around Node-RED and the visual editor. This will allow you to start designing your flows and take advantage of Node-RED. You will run Node-RED in your local machine and create your firs flow. 

### What you will learn
In this article, you will learn:
* Node-Red Basics
* Using nodes to design a complete node-RED flow.

### What you need
You must have successfully completed the following operations:

* [Getting Started With Node-Red](/content/lab-3-1-getting-started-with-node-red)

### Install Node-RED on Raspberry Pi
### Install Node-RED nodes
### Trying node-red
Run sudo node-red-start the open a web browser to http://<YOUR PI's IP ADDRESS>:1880

Adding npm
The raspbian image doesn't include npm, however it is easy to install:
- sudo apt-get update
- sudo apt-get install npm

Unfortunately this only gets us npm version 1.4.21 which wont work for us, but that's easily updatable:
- sudo npm i npm -g

- sudo apt-get update && sudo apt-get upgrade
- sudo apt-get install nodered
- sudo npm install node-red-contrib-opi-gpio 

Autostart Node-RED on boot
- sudo systemctl enable nodered.service
- sudo reboot

## Adding johnny-five
We'll need to add some packages to node-red for johnny-five in the root's .node-red directory:
- sudo su -
- cd .node-red
- npm i node-red-contrib-gpio

That takes a couple of minutes to finish on a raspberry pi 2, and gets us the GPIO and johnny-five nodes. At this point we could plug in an arduino uno to a USB port and get started, however let's add raspi-io so that we can use the pi's onboard pins.
- npm i raspi-io
At this point we'll need to reboot.
- sudo reboot
Then start node red up again.
- sudo node-red-start


## Blinking an LED
Once again, point your browser to http://<YOUR PI's IP ADDRESS>:1880
Drop a johnny-five node into the node-red workspace and double click it. Click the edit button to configure a new nodebot, and select Raspberry Pi as the nodebot type and click Add.

Then in the onReady code block you can do something like:
- var led = new five.Led('GPIO4');

- led.blink(500);
Click the deploy button on the upper-right of node-red.
If you have an LED connected to GPIO4 it should start blinking!



## Make a twitter button.
We can connect the output of our johnny-five node to any other type of node.

Then just wire up a physical button to your board, handle button press events by passing a new message along:
var button = new five.Button("GPIO4");

button.on("press", function() {
    node.send({payload: "johnny-five is awesome!"});
});
















### Summary
You've created a flow where you explored several ways to send data to Azure IoT Hub, using the **Azure IoT node**. You've tried sending basic data payloads - from strings to JSON payloads with multiple data inside. In real IoT projects a different set of data is usual sent to the cloud, identifying device, timestamps and data collected from device sensors or appliances (ex: telemetry)
You have also learned how to use functions and add some logic to your flow.

---

Back to [Lab 3 - IoT with Linux and Node.js](/content/lab-3-linux-iot-node-red.md)

Back to [IoT Labs homepage](/readme.md#labs)

