# Lab 3.4: Azure IoT hub examples

### What you will do
Learn basics around Node-RED and the visual editor. This will allow you to start designing your flows and take advantage of Node-RED. You will run Node-RED in your local machine and create your firs flow. 

### What you will learn
In this article, you will learn:
* Node-Red Basics
* Using nodes to design a complete node-RED flow.

### What you need
You must have successfully completed the following operations:

* [Getting Started With Node-Red](/content/lab-3-1-getting-started-with-node-red)

### Connecting Node-Red with Azure IoT Hub

1. Drag the **azureiothub** node to your flow. Double-click int this node so we can configure it. Specify a **name** for this node (ex: 'Azure IoT Hub'), select one of the available **protocols** (ex: mqtt) and specify the **connection string**. If you dont know the connection string, just go the the [https://portal.azure.com](https://portal.azure.com) and look in *Shared access policies* of your IoT Hub. 

  ![Configure Azure IoT Hub node](/images/lab3_node-red-configure_azure-iot-node.png)

2. Add a new inject node, and configure it.
  Specify a **name** (ex: "Hello World"), and for the **payload** select *string* (in the drop down menu). Finally just type a custom message you would like to see arriving in Azure (ex: "I Rock! Hello from Node-Red!!").

  ![Configure inject node](/images/lab3_node-red-configure_inject_node.png)

3. Connect the **inject** node with the **Azure IoT** node.

4. Now you will repeat this process to add aditional inputs do Azure IoT node. The ideia is to understand how data can be ingested into Azure IoT, as an entry point for a IoT cloud back end.

 * Create an inject node **named** "Send one value", where the **payload** is JSON based with the following content:
  
  ```json
  {"deviceID": "myDevice", "Protocol": "mqtt", "Data": { "Speed" : "25"}}
  ```

 * Create an inject node **named** "Send Multiple values", where the **payload** is JSON based with the following content:
  
  ```json
  {"deviceID": "myDevice", "Protocol": "mqtt", "Data": { "AvgFlowRate" : 500, "FlowRate" : 700, "AvgStaticPressure" : 525, "StaticPressure" : 518, "AvgCasingPressure" : 776, "CasingPressure" : 805, "AvgTubingPressure" : 609, "TubingPressure" : 588}}
  ```

  > Remember to connect the new inject nodes as input to the existing **Azure IoT** node

5. To test your flow, press the **Deploy** button on your top right. Once deployed, you can press the buttons on the left of the inject nodes.

  ![Node-RED Deploy button](/images/lab3_node-red-deploy-button.png)

  To validate Azure IoT Hub is getting the messages, you can use Azure IoT Device Explorer. In the **Data** tab you can check all the messages ingested into Azure IoT Hub.
  
  ![Azure IoT Device Explorer](/images/lab3_node-red-validate-messages-in-azure.png)
 
### Taking advantage of Node functions

Now let's be smart and take advantage of some of the logic components, Node-RED has to offer.

1. Add aditional inputs do Azure IoT node, wit the following configurations:

 * An inject node **named** "Speed", where the **payload** is of type *number* with a value in it (ex:345). In the **Topic** write "speed".

 * An inject node **named** "Temperature", where the **payload** is of type *number* with a value in it (ex:29). In the **Topic** write "temperature".

* An inject node **named** "Pressure", where the **payload** is of type *number* with a value in it (ex:2342). In the **Topic** write "pressure".
   
2. Add a **function** node and double-click to configure it. Type a **name** (ex: Function) and in the function field you're allowed to insert Javascript code, that will be executed for every input. Add the following Javascript code:

  ```javascript
  msg1 = '{"deviceID": "myDevice", '
  msg1 = msg1 + '"Protocol": "mqtt", '
  msg1 = msg1 + '"Data": { "' + msg.topic + '": "' + msg.payload + '"}}'
  
  newMsg = { payload: msg1 };
  return newMsg;
   ```

2. Now, connect all the recent inject nodes as inputs to this function. By this time, you're injecting data into the function, which is returning a message as an outpout. 

3. Let's send this ooutput to Azure. For that, you just need to connect the **function** node to the already existing **Azure IoT Hub** node.

  Your flow should look like something like this:

  ![Azure IoT flow](/images/lab3_node-red-full-azure-iot-flow.png)

> If you get in trouble using the **function** node, you will want to debug it. Best way to achieve this is by using the **debug** node.
> 
> Add a **debug** node and connect both the **function** and **Azure IoT Hub** node as inputs to PPDebug** node.
> Remember to deploy the flow. Now everytime you execute the flow, you will see information in the debug tab.
>
>  ![Azure IoT flow with Debug node](/images/lab3_node-red-full-azure-iot-flow-width-debug.png)
>

You're done! Congrats! You're sending data to Azure IoT Hub with Node-RED.

####Backup-Plan

If you got lost, no problem. Every flow in Node-RED is no more than a JSON. So you can take a look at the flow created in this lab step. It can be imported straight into the editor by pasting the json into the Import dialog (Ctrl-I or via the dropdown menu).

 ```json
[{"id":"6c6d9f11.af211","type":"azureiothub","z":"fd34366.251aec8","name":"Azure IoT Hub","protocol":"mqtt","x":480,"y":572,"wires":[["74a79d08.2b36a4"]]},{"id":"9a26a4f5.967a88","type":"function","z":"fd34366.251aec8","name":"Function","func":"msg1 = '{\"deviceID\": \"myDevice\", '\nmsg1 = msg1 + '\"Protocol\": \"mqtt\", '\nmsg1 = msg1 + '\"Data\": { \"' + msg.topic + '\": \"' + msg.payload + '\"}}'\n\nnewMsg = { payload: msg1 };\nreturn newMsg;\n","outputs":1,"noerr":0,"x":344,"y":734,"wires":[["6c6d9f11.af211","74a79d08.2b36a4"]]},{"id":"9488a546.b1b7a8","type":"inject","z":"fd34366.251aec8","name":"Speed","topic":"speed","payload":"345","payloadType":"num","repeat":"","crontab":"","once":false,"x":161,"y":699,"wires":[["9a26a4f5.967a88"]]},{"id":"74a79d08.2b36a4","type":"debug","z":"fd34366.251aec8","name":"","active":true,"console":"false","complete":"payload","x":648.3400268554687,"y":662.47998046875,"wires":[]},{"id":"40b35a59.ef3754","type":"inject","z":"fd34366.251aec8","name":"Temperature","topic":"temperature","payload":"27","payloadType":"num","repeat":"","crontab":"","once":false,"x":179,"y":735,"wires":[["9a26a4f5.967a88"]]},{"id":"3fbc067e.18977a","type":"inject","z":"fd34366.251aec8","name":"Pressure","topic":"pressure","payload":"2342","payloadType":"num","repeat":"","crontab":"","once":false,"x":169,"y":771,"wires":[["9a26a4f5.967a88"]]},{"id":"4d7187b2.3dfc08","type":"inject","z":"fd34366.251aec8","name":"Send Multiple Values","topic":"","payload":"{\"deviceID\": \"myDevice\", \"Protocol\": \"mqtt\", \"Data\": { \"AvgFlowRate\" : 500, \"FlowRate\" : 700, \"AvgStaticPressure\" : 525, \"StaticPressure\" : 518, \"AvgCasingPressure\" : 776, \"CasingPressure\" : 805, \"AvgTubingPressure\" : 609, \"TubingPressure\" : 588}}","payloadType":"json","repeat":"","crontab":"","once":false,"x":200,"y":560,"wires":[["6c6d9f11.af211"]]},{"id":"4f6d30c7.a5ed3","type":"inject","z":"fd34366.251aec8","name":"Send one value","topic":"","payload":"{\"deviceID\": \"myDevice\", \"Protocol\": \"mqtt\", \"Data\": { \"Speed\" : \"25\"}}","payloadType":"json","repeat":"","crontab":"","once":false,"x":177.90994262695312,"y":522.0900192260742,"wires":[["6c6d9f11.af211"]]},{"id":"c979fcd4.0c2de","type":"inject","z":"fd34366.251aec8","name":"Hello World","topic":"","payload":"\"Hello from Node-Red\"","payloadType":"str","repeat":"","crontab":"","once":false,"x":173,"y":454,"wires":[["6c6d9f11.af211"]]}]
  ```

### Summary
You've created a flow where you explored several ways to send data to Azure IoT Hub, using the **Azure IoT node**. You've tried sending basic data payloads - from strings to JSON payloads with multiple data inside. In real IoT projects a different set of data is usual sent to the cloud, identifying device, timestamps and data collected from device sensors or appliances (ex: telemetry)
You have also learned how to use functions and add some logic to your flow.

---

Back to [Lab 3 - IoT with Linux and Node.js](/content/lab-3-linux-iot-node-red.md)

Back to [IoT Labs homepage](/readme.md#labs)

