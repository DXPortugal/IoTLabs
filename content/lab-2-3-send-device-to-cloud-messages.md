# Lab 2.3: Send device-to-cloud messages

> [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) is a solution for easily running *functions* (small pieces of code) in the cloud. An Azure function app hosts the execution of your functions in Azure.

#### What you will do
Use an Azure Resource Manager template to create an Azure function app and an Azure storage account. The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage. You will deploy and run a sample application on Raspberry Pi 3 that sends messages to your IoT hub. Finally, you will monitor the device-to-cloud messages that are sent from Raspberry Pi 3 to your IoT hub as the messages are written to your Azure Table storage.

#### What you will learn
In this article, you will learn:

* How to use [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) to deploy Azure resources.
* How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.

You will learn how to use the gulp tool to deploy and run the sample Node.js application on Pi.
Also, you will learn how to use the gulp read-message task to read messages persisted in your Azure Table storage.

#### What you need
You must have successfully completed:
* [Configure your device and get the tools](/content/lab-2-configure-your-device-and-get-the-tools.md)
* [Create your IoT hub and register device](/content/lab-2-2-create-your-iot-hub-and-register-device.md)


### Create an Azure function app and Azure storage account 

#### Open the sample app
Open the sample project in Visual Studio Code by running the following commands:

```bash
cd Lesson3
code .
```

![Repo structure](/images/lab2_repo_structure.png)

* The `app.js` file in the `app` subfolder is the key source file. This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.
* The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.
* The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.
* The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.

#### Configure Azure Resource Manager templates and create resources in Azure
Update the `arm-template-param.json` file in Visual Studio Code.

![Azure Resource Manager template parameters](/images/lab2_arm_para.png)

* Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [Create your IoT hub and register device](/content/lab-2-2-create-your-iot-hub-and-register-device.md).
* Replace **[prefix string for new resources]** with any prefix you want. The prefix ensures that the resource name is globally unique to avoid conflict. Do not use a dash or number initial in the prefix.

After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:

```bash
azure group deployment create -f "arm-template.json" -e "arm-template-param.json" -g **NameOfResourceGroup**
```

It takes about five minutes to create these resources. While the resource creation is in progress, you can move on to the next article.

### Run a sample application to send device-to-cloud messages

#### Get your IoT hub and device connection strings
The device connection string is used by your Pi to connect to your IoT hub. The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub. 

* List all your IoT hubs in your resource group by running the following Azure CLI command:

```bash
az iot hub list -g iot-sample --query [].name
```

Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.

* Get the IoT hub connection string by running the following Azure CLI command:

```bash
azure iothub connectionstring show --name {my hub name} -g iot-sample
```

`{my hub name}` is the name that you specified when you created your IoT hub and registered Pi.

* Get the device connection string by running the following command:

```bash
azure iothub device connectionstring show --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Use `myraspberrypi` as the value of `{device id}` if you didn't change the value.

#### Configure the device connection
1. Initialize the configuration file by running the following commands:
   
   ```bash
   npm install
   gulp init
   ```
2. Open the device configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![config.json](/images/lab2_config.png)
3. Make the following replacements in the `config-raspberrypi.json` file:
   
   * Replace **[device hostname or IP address]** with the device IP address or host name you got from `device-discovery-cli` or with the value inherited when you configured your device.
   * Replace **[IoT device connection string]** with the `device connection string` you obtained.
   * Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.

Update the `config-raspberrypi.json` file so that you can deploy the sample application from your computer.

#### Deploy and run the sample application
Deploy and run the sample application on Pi by running the following command:

```bash
gulp deploy && gulp run
```

#### Verify that the sample application works
You should see the LED that is connected to Pi blinking every two seconds. Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub. In addition, each message received by the IoT hub is printed in the console window. The sample application terminates automatically after sending 20 messages.

![Sample application with sent and received messages](/images/lab2_gulp_run.png)


### Read messages persisted in Azure Storage

#### Read new messages from your storage account
In the previous step, you ran a sample application on Pi. The sample application sent messages to your Azure IoT hub. The messages sent to your IoT hub are stored into your Azure Table storage via the Azure function app. You need the Azure storage connection string to read messages from your Azure Table storage.

To read messages stored in your Azure Table storage, follow these steps:

1. Get the connection string by running the following commands:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   The first command retrieves the `storage name` that is used in the second command to get the connection string. Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.
2. Open the configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Replace `[Azure storage connection string]` with the connection string you got in step 1.
4. Save the `config-raspberrypi.json` file.
5. Send messages again and read them from your Azure Table storage by running the following command:
   
   ```bash
   gulp run --read-storage
   ```
   
   The logic for reading from Azure Table storage is in the `azure-table.js` file.
   
    ![gulp run --read-storage](/images/lab2_gulp_read_message.png)

#### Summary
You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages. 
You've deployed and run the new blink sample application on Pi to send device-to-cloud messages to your IoT hub. 
You've successfully connected Pi to your IoT hub in the cloud and used the blink sample application to send device-to-cloud messages. You also used the Azure function app to store incoming IoT hub messages to your Azure Table storage. You can now send cloud-to-device messages from your IoT hub to Pi.

In the **[next lab][nextlab]** you will send messages from the cloud (Azure IoT Hub) to your device.

---

Back to [Lab 2 - IoT with Linux and Node.js](/content/lab-2-linux-node-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato09.jpg)

[nextlab]: /content/lab-2-4-send-cloud-to-device-messages.md
