# Lab 3.2: Creating your first flow

### What you will do
Learn basics around Node-RED and the visual editor. This will allow you to start designing your flows and take advantage of Node-RED. You will run Node-RED in your local machine and create your firs flow. 

### What you will learn
In this article, you will learn:
* Node-Red Basics
* Using nodes to design a complete node-RED flow.

### What you need
You must have successfully completed the following operations:

* [Getting Started With Node-Red](/content/lab-3-1-getting-started-with-node-red)

### Node-Red Basics

Node-RED is based on Node.js. The Node-RED application runs as a web server, and you customize and manipulate functional *'flows'* from any computer’s browser, local or remote. Every Node-RED app consists of nodes which are linked together to form the logical flow of your application. The nodes typically fall under input, operation or output.
Below is a very simple example of how these different nodes would interact with each other.

![Node-RED Basics](/images/lab3_node-red-basics.png)

In reality, you wouldn't have nodes with such generic names as "input" or "output". For example, both Microsoft and the community have already created Azure IoT related nodes (you installed it in last Lab step) which are specifically designed to communicate with Azure.

![Node-Red node for Azure IoT Hub](/images/lab3_node-red-basics-azure.png)

This node can be used in any Node-RED application once you install the nodes into your Node environment. Now, enough talk! Let's try it!

### Creating the first flow
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

  ```javascript
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

If you got lost, no problem. Every flow in Node-RED is no more than a JSON. So you can take a look at the flow created in this lab step. It can be imported straight into the editor by pasting the json into the Import dialog (Ctrl-I or via the dropdown menu).

 ```json
 [{"id":"58ffae9d.a7005","type":"debug","name":"","active":true,"complete":false,"x":640,"y":200,"wires":[]},{"id":"17626462.e89d9c","type":"inject","name":"","topic":"","payload":"","repeat":"","once":false,"x":240,"y":200,"wires":[["2921667d.d6de9a"]]},{"id":"2921667d.d6de9a","type":"function","name":"Format timestamp","func":"// Create a Date object from the payload\nvar date = new Date(msg.payload);\n// Change the payload to be a formatted Date string\nmsg.payload = date.toString();\n// Return the message so it can be sent on\nreturn msg;","outputs":1,"x":440,"y":200,"wires":[["58ffae9d.a7005"]]}]
  ```

### Summary
You've learned about the basics of Node-Red flows, and created your first flow.

In the **[next lab][nextlab]** you will create a more advanced node red flow.

---

Back to [Lab 3 - IoT with Linux and Node.js](/content/lab-3-linux-iot-node-red.md)

Back to [IoT Labs homepage](/readme.md)

[nextlab]: /content/lab-3-3-creating-creating-a-more-advanced-node-red-flow.md
