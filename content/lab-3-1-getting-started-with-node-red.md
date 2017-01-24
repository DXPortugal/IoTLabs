# Lab 3.1: Getting Started With Node-Red

> [Node-RED](https://nodered.org/) is a tool for wiring together hardware devices, APIs and online services in new and interesting ways.

### What you will do
Understand [Node-RED](https://nodered.org/) and it's concepts. Because the best way to learn is getting hands dirty, you will install Node-RED and the Node-Red node for Azure IoT Hub. 

### What you will learn
In this article, you will learn:
* What is Node-RED and Node-RED nodes.
* How to install node-RED.
* How to install Node-Red node for Azure IoT Hub.

### What you need
You must have successfully completed the following operations:

* [Configure your device](/content/lab-2-configure-your-device-and-get-the-tools.md)
* [Get the tools](/content/lab-2-configure-your-device-and-get-the-tools.md#install-git-note)
<!--* This Tutorial uses the Device Explorer sample executable from the Azure SDK to prevent you from having to program to create a Device in your IoT Hub.  It can be downloaded and installed from this [link](https://github.com/Azure/azure-iot-sdks/releases).  Scroll down to the Downloads section and download and install **SetupDeviceExplorer.msi** on Windows.
 ![](images/lab3-github-deviceexplorer.png)
-->

### What is Node-RED and Node-Red nodes

Node-RED is an open source software tool developed by IBM for wiring together hardware devices, APIs and online services as part of the Internet of Things (IoT). Node-RED provides a browser-based flow editor, which can be used to create JavaScript functions. Elements of applications can be saved or shared for re-use. The runtime is built on Node.js. The flows created in Node-RED are stored using JSON.
Originally developed as an open source project at IBM in late 2013, to meet their need to quickly connect hardware and devices to web services and other software – as a sort of glue for the IoT – it has quickly evolved to be a general purpose IoT programming tool.
Importantly, Node-RED has rapidly developed a significant and growing user base and an active developer community who are contributing new nodes that allow programmers to reuse Node-RED code for a wide variety of tasks.

It uses a visual programming approach that allows developers to connect predefined code blocks, known as '*nodes*', together to perform a task. The connected nodes, usually a combination of input nodes, processing nodes and output nodes, when wired together, make up a '*flows*'.
This '*nodes*' which look simply to be icons that you drag and drop on to the canvas and wire together. Each *node* offers different functionality which can range from a simple debug node to be able to see what's going on in your flow, through to a Raspberry Pi node which allows you to read and write to the GPIO pins of your Pi.

![Node-RED flow](/images/lab3_node-red.png)

Here's an example of a *flow* where location is captured, the tweet is created, Forecast.io is queried with the location, so we can have weather forecast. Finally, data is formatted accordgingly to twitter API, and is sent. This flow took literally 15 minutes to set up.

### Node-Red node for Azure IoT Hub

There is already a node that adds Azure IoT connectivity to any Node-RED flow. To use the Azure IoT Hub Node-RED node, you need to install it on top of Node-RED.

It's available as an open source project at [azure-iot-sdk-node](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/node-red) github repo.

## Install and run Node-RED

The easiest way to install Node-RED is to use node’s package manager, npm. Installing it as a global module adds the command node-red to your system path:

   ```bash
   npm install -g node-red
   ```

Once installed, you are ready to run Node-RED.
Because it was installed as a global npm package, you can use the **node-red** command:

   ```bash
   22 Jan 22:04:36 - [info]
   
   Welcome to Node-RED
   ===================
   
   22 Jan 22:04:36 - [info] Node-RED version: v0.16.1
   22 Jan 22:04:36 - [info] Node.js  version: v6.4.0
   22 Jan 22:04:36 - [info] Windows_NT 10.0.14393 ia32 LE
   22 Jan 22:04:37 - [info] Loading palette nodes
   22 Jan 22:04:39 - [info] Removing modules from config
   22 Jan 22:04:39 - [info] Settings file  : \Users\xpto\.node-red\settings.js
   22 Jan 22:04:39 - [info] User directory : \Users\xpto\.node-red
   22 Jan 22:04:39 - [info] Flows file     : \Users\xpto\.node-red\flows_MACHINE.json
   22 Jan 22:04:39 - [info] Creating new flow file
   22 Jan 22:04:39 - [info] Starting flows
   22 Jan 22:04:39 - [info] Started flows
   22 Jan 22:04:39 - [info] Server now running at http://127.0.0.1:1880/
   ```

You can then access the Node-RED editor at [http://localhost:1880](http://localhost:1880).

## Install Node-Red node for Azure IoT Hub


1. Clone or download the [Node.js SDK for Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdk-node). 
   You can download the zip or clone the repo by executing <code>git clone https://github.com/Azure/azure-iot-sdk-node.git</code>

![Node.js SDK for Microsoft Azure IoT Clone/Download](lab3_nodejs-iot-sdk-download-clone.png)

2. Navigate to repo root folder and then the node-red node folder: **device\node-red**. Check there is an existant package.json file. This json file corresponds to the node package description.

3. To install the node, run the following command: 

  ```bash
  npm install -g
  ```

4. To validate this installation, access the Node-Red editor, one more time at [http://localhost:1880](http://localhost:1880). You may have to restart node-red (Press Ctrl-C to stop the server).


### Summary
You've now installed Node-RED and made the Node-Red node for Azure IoT Hub, available to be used in your flows. From now on, everytime you start Node-RED, this node will be available to connect your flow to Microsoft IoT Hub, so you can ingest data.  

In the **[next lab][nextlab]** you will start using Node-RED, learning the basics of using the editor. You will create your first flow.

---

Back to [Lab 3 - IoT with Linux and Node.js](/content/lab-3-linux-iot-node-red.md)

Back to [IoT Labs homepage](/readme.md)

[nextlab]: /content/lab-3-2-creating-your-first-flow.md
