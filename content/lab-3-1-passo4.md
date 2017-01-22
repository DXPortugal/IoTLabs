# Lab 3.3: Create your first flows

Node-RED is a visual wiring tool for the Internet of Things designed by IBM, a great tool to who want to develop fast and simple applications.


Open your browser and go to https://nodered.org/
You will find the steps to install Node-RED in linux devices, so, just open a terminal window in DragonBoard and type:
$ sudo npm install -g node-red
$ node-red
After the installation, lets download node-red-contrib-azure-iot-hub, a Node-RED node that allows you to send messages and register devices with Azure IoT Hub.
Go to https://flows.nodered.org/node/node-red-contrib-azure-iot-hub
Install:
$ npm install node-red-contrib-azure-iot-hub
After the installation, lets run Node-RED:
node-red
Go to your browser and type 127.0.0.1:1880
In the flow tab, located at the left side of the screen, verifiy if Azure cloud blocks is in there (see in images).
Well, click on the menu button located at the upper right side of the screen and click on Import, Clipboard (see in images). Copy the attached code in there and click on Import button.
Ok, now Double-click the Register Payload node, enter your desired deviceId into the Payload field and click Done. For example: {"deviceId": "DragonBoard"}
Double-click the Azure IoT Hub Registry node, enter your IoT Hub connection string and click Done. (see in images how to get your connection string).
Click on Register Payload node to insert the Device String to Azure IoT Hub Registry node, a message confirming the Device creation notification will be appear in debug log. Save the primary and secondary keys!
Now, double-click on the Send Payload block and put the deviceid that you created in previous step, and the primary key.
Double-Click on Azure IoT Hub node and insert the hostname of your Azure IoT Hub (see in images).
Ok, now your are sending data to your device connected to Azure!!

 node-red.txt 
Step 3: Verify the incomming data

To verify the data comming from your node-red application, follow the microsoft guide to run a javascript code to receive device-to-cloud messages
https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-getstarted
Go to Receive device-to-cloud messages session and run this code, all message sent to the Hub is displayed in your terminal!



[{"id":"e092747f.d2fb08","type":"azureiothubregistry","z":"a625ca19.b34ce8","name":"Azure IoT Hub Registry","x":370,"y":120,"wires":[["4c1b62c2.1681bc"]]},{"id":"8877c5f0.e15828","type":"inject","z":"a625ca19.b34ce8","name":"Register Payload","topic":"","payload":"{\"deviceId\": \"device146\"}","payloadType":"json","repeat":"","crontab":"","once":false,"x":140,"y":120,"wires":[["e092747f.d2fb08"]]},{"id":"4c1b62c2.1681bc","type":"debug","z":"a625ca19.b34ce8","name":"Log","active":true,"console":"false","complete":"true","x":650,"y":120,"wires":[]},{"id":"a3b3f737.a7a428","type":"debug","z":"a625ca19.b34ce8","name":"Log","active":true,"console":"false","complete":"true","x":650,"y":60,"wires":[]},{"id":"2be1c658.c9bbea","type":"azureiothub","z":"a625ca19.b34ce8","name":"Azure IoT Hub","protocol":"amqp","x":340,"y":60,"wires":[["a3b3f737.a7a428"]]},{"id":"600a3eb0.2b238","type":"inject","z":"a625ca19.b34ce8","name":"Send Payload","topic":"","payload":"{ \"deviceId\": \"device145\", \"key\": \"Qmq2SSe4CzuB5N0v2FvfV3LAE8OKf2rWj6IQPx+AU3w=\", \"protocol\": \"amqp\", \"data\": \"{tem: 25, wind: 20}\" }","payloadType":"json","repeat":"","crontab":"","once":false,"x":130,"y":60,"wires":[["2be1c658.c9bbea"]]}]