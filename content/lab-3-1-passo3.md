# Lab 3.3: Create your first flows

> [Node-RED](https://nodered.org/) is a tool for wiring together hardware devices, APIs and online services in new and interesting ways.

### What you will do
Install and understand [Node-RED](https://nodered.org/) 

### What you will learn
In this article, you will learn:
* How to install node-RED.

### What you need
You must have successfully completed the following operations:

* [Configure your device](/content/lab-2-configure-your-device-and-get-the-tools.md)
* [Get the tools](/content/lab-2-configure-your-device-and-get-the-tools.md#install-git-note)





Once Node-RED is running, point a local browser at http://localhost:1880. You can always use a browser from another machine if you know the ip address or name of the Node-RED instance - http://{Node-RED-machine-ip-address}:1880


1. Add an Inject node
  The Inject node allows you to inject messages into a flow, either by clicking the button on the node, or setting a time interval between injects.
  Drag one onto the workspace from the palette.
  Open the sidebar (Ctrl-Space, or via the dropdown menu) and select the Info tab.
  Select the newly added Inject node to see information about its properties and a description of what it does.

2. Add a Debug node
  The Debug node causes any message to be displayed in the Debug sidebar. By default, it just displays the payload of the message, but it is possible to display the entire message object.

3. Wire the two together
  Connect the Inject and Debug nodes together by dragging between the output port of one to the input port of the other.

4. Deploy
  At this point, the nodes only exist in the editor and must be deployed to the server.
  Click the Deploy button. Simple as that.
  With the Debug sidebar tab selected, click the Inject button. You should see numbers appear in the sidebar. By default, the Inject node uses the number of milliseconds since January 1st, 1970 as its payload. Let’s do something more useful with that.

5. Add a Function node
  The Function node allows you to pass each message though a JavaScript function.
  Wire the Function node in between the Inject and Debug nodes. You may need to delete the existing wire (select it and hit delete on the keyboard).
  Double-click on the Function node to bring up the edit dialog. Copy the follow code into the function field:
  ```Javascript
  // Create a Date object from the payload
  var date = new Date(msg.payload);
  // Change the payload to be a formatted Date string
  msg.payload = date.toString();
  // Return the message so it can be sent on
  return msg;
  ```
  
  Click Ok to close the edit dialog and then click the deploy button.
  Now when you click the Inject button, the messages in the sidebar will be more readable time stamps.

####Backup-Plan
The flow created in this example is represented by the following json. It can be imported straight into the editor by pasting the json into the Import dialog (Ctrl-I or via the dropdown menu).
```JSON
[{"id":"58ffae9d.a7005","type":"debug","name":"","active":true,"complete":false,"x":640,"y":200,"wires":[]},{"id":"17626462.e89d9c","type":"inject","name":"","topic":"","payload":"","repeat":"","once":false,"x":240,"y":200,"wires":[["2921667d.d6de9a"]]},{"id":"2921667d.d6de9a","type":"function","name":"Format timestamp","func":"// Create a Date object from the payload\nvar date = new Date(msg.payload);\n// Change the payload to be a formatted Date string\nmsg.payload = date.toString();\n// Return the message so it can be sent on\nreturn msg;","outputs":1,"x":440,"y":200,"wires":[["58ffae9d.a7005"]]}]
```









This example is slightly more complex and starts to bring in data from external sources to do something useful locally.
* It will go out to an external web site
* grab some information
* read and convert that into a useful form
* output that in two formats, one as a JSON object for further use, and one as a boolean to switch things on and off

1. Add an Inject node
  In the previous example, the Inject node was used to trigger the flow when its button was clicked. For this example, the Inject node will be configured to trigger the flow at a regular interval.
  Drag an Inject node onto the workspace from the palette.
  Double click the node to bring up the edit dialog. Set the repeat interval to every 5 minutes on every day.
  Click Ok to close the dialog.

2. Add an HttpRequest node
  The HttpRequest node can be used to retrieve a web-page when triggered.
  After adding one to the workspace, edit it to set the URL property to:
  `http://realtimeweb-prod.nationalgrid.com/SystemData.aspx`
  You can optionally add a friendly name.

3. Add a function node
  Add a Function node with the following code:
  
  ```Javascript
  // does a simple text extract parse of the http output to provide an
  // object containing the uk power demand, frequency and time
  
  if (~msg.payload.indexOf('<span')) {
      var dem = msg.payload.split('Demand:')[1].split("MW")[0];
      var fre = msg.payload.split('Frequency:')[1].split("Hz")[0];
  
      msg.payload = {};
      msg.payload.demand = parseInt(dem.split(">")[1].split("<")[0]);
      msg.payload.frequency = parseFloat(fre.split(">")[1].split("<")[0]);
  
      msg2 = {};
      msg2.payload = (msg.payload.frequency >= 50) ? true : false;
  
      return [msg,msg2];
  }
  return null;
  ```

  Set the number of outputs for the function node to 2.

4. Add a Debug node
  Add two Debug nodes.

5. Wire them all together
  * Wire the Inject node output to the HttpRequest node input.
  * Wire the HttpRequest node output to the Function node input.
  * Wire each of the Function node outputs to a different Debug node input.

6. Deploy
  At this point, the nodes only exist in the editor and must be deployed to the server.
  Click the Deploy button.
  With the Debug sidebar tab selected (Ctrl-Space, or via the dropdown menu, then click the Debug tab), click the Inject button. You should see an entry with some contents that looks like:
  
  `(Object) { "demand": 34819, "frequency": 50.04 }`
  
  and another with something like:
  
  `(boolean) true`
  
7. Summary

  You now have a flow that goes to the Internet - gets the live UK total electricity consumption - and converts it into a JavaScript object with demand in MW, and frequency in Hertz.
  The object is emitted out of the first output of the Function node.
  The frequency is an indication of overall stress - so when the frequency is under 50 HZ there may be excess load on the overall National Grid. This is indicated in the message emitted out of the second output of the Function node; if the payload is true, there is capacity in the grid.

####Backup-Plan
The flow created in this example is represented by the following json. It can be imported straight into the editor by pasting the json into the Import dialog (Ctrl-I or via the dropdown menu).
 ```Javascript
 [{"id":"11b032a3.ee4fcd","type":"inject","name":"Tick","topic":"","payload":"","repeat":"","crontab":"*/5 * * * *","once":false,"x":161,"y":828,"z":"6480e14.f9b7f2","wires":[["a2b3542e.5d4ca8"]]},{"id":"a2b3542e.5d4ca8","type":"http request","name":"UK Power","method":"GET","url":"http://realtimeweb-prod.nationalgrid.com/SystemData.aspx","x":301,"y":828,"z":"6480e14.f9b7f2","wires":[["2631e2da.d9ce1e"]]},{"id":"2631e2da.d9ce1e","type":"function","name":"UK Power Demand","func":"// does a simple text extract parse of the http output to provide an\n// object containing the uk power demand, frequency and time\n\nif (~msg.payload.indexOf('<span')) {\n    var dem = msg.payload.split('Demand:')[1].split(\"MW\")[0];\n    var fre = msg.payload.split('Frequency:')[1].split(\"Hz\")[0];\n\n    msg.payload = {};\n    msg.payload.demand = parseInt(dem.split(\">\")[1].split(\"<\")[0]);\n    msg.payload.frequency = parseFloat(fre.split(\">\")[1].split(\"<\")[0]);\n    \n    msg2 = {};\n    msg2.payload = (msg.payload.frequency >= 50) ? true : false;\n\n    return [msg,msg2];\n}\n\nreturn null;","outputs":"2","valid":true,"x":478,"y":828,"z":"6480e14.f9b7f2","wires":[["8e56f4d3.71a908"],["cd84371b.327bc8"]]},{"id":"8e56f4d3.71a908","type":"debug","name":"","active":true,"complete":false,"x":678,"y":798,"z":"6480e14.f9b7f2","wires":[]},{"id":"cd84371b.327bc8","type":"debug","name":"","active":true,"complete":false,"x":679,"y":869,"z":"6480e14.f9b7f2","wires":[]}]
  ```
















### Summary
You've created an IoT hub and registered Pi with a device identity in your IoT hub. You're ready to learn how to send messages from Pi to your IoT hub.


In the **[next lab][nextlab]** you will start sending messages, from your device to IoT Hub.

---

Back to [Lab 3 - IoT with Linux and Node.js](/content/lab-3-linux-iot-node-red.md)

Back to [IoT Labs homepage](/readme.md)

[nextlab]: /content/lab-2-3-send-device-to-cloud-messages.md
