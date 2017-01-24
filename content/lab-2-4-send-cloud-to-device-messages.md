# Lab 2.4: Send cloud-to-device messages

In this lab, you deploy a sample application on Raspberry Pi 3. The sample application monitors incoming messages from your IoT hub. You also run a gulp task on your computer to send messages to Pi from your IoT hub. When the sample application receives the messages, it blinks the LED.

### What you will do
* Connect the sample application to your IoT hub.
* Deploy and run the sample application.
* Send messages from your IoT hub to Pi to blink the LED.

### What you will learn
In this article, you will learn:
* How to monitor incoming messages from your IoT hub
* How to send cloud-to-device messages from your IoT hub to Pi.

### What you need
* Raspberry Pi 3, set up for use. To learn how to set up Pi, see [Configure your device and get the tools](/content/lab-2-configure-your-device-and-get-the-tools.md).
* An IoT hub that is created in your Azure subscription. To learn how to create your IoT hub, see [Create your IoT hub and register device](/content/lab-2-2-create-your-iot-hub-and-register-device.md).

### Connect the sample application to your IoT hub
1. Make sure that you're in the repo folder `iot-hub-node-raspberrypi-getting-started`. Open the sample application in Visual Studio Code by running the following commands:
   
   ```bash
   cd Lesson4
   code .
   ```
   
   Notice the `app.js` file in the `app` subfolder. The `app.js` file is the key source file that contains the code to monitor incoming messages from the IoT hub. The `blinkLED` function blinks the LED.
   
   ![Repo structure in the sample application](/images/lab2_new_repo_structure.png)
2. Initialize the configuration file by using the following commands:
   
   ```bash
   npm install
   gulp init
   ```
   
   If you completed the steps in [Create an Azure function app and storage account](/content/lab-2-3-send-device-to-cloud-messages.md#create-an-azure-function-app-and-azure-storage-account) 
 on this computer, all the configurations are inherited, so you can skip to the task of deploying and running the sample application. If you completed the steps in [Create an Azure function app and storage account](/content/lab-2-3-send-device-to-cloud-messages.md#create-an-azure-function-app-and-azure-storage-account) 
 on a different computer, you need to replace the placeholders in the `config-raspberrypi.json` file. The `config-raspberrypi.json` file is in the subfolder of your home folder.
   
   ![Contents of the config-raspberrypi.json file](/images/lab2_config_raspberrypi.png)

* Replace **[device hostname or IP address]** with the IP address of Pi or the host name that you get by running the `devdisco list --eth` command.
* Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.
* Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.

> [!NOTE]
> Run **gulp install-tools** as well, if you haven't done it in Lesson 1.

## Deploy and run the application
Deploy and run the sample application on Pi by running the following command:

```bash
gulp deploy && gulp run
```

The command deploys the sample application to Pi. Then, it runs the application on Pi and a separate task on your host computer to send 20 blink messages to Pi from your IoT hub.

After the application runs, it starts listening to messages from your IoT hub. Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Pi. For each blink message that Pi receives, the sample application calls the `blinkLED` function to blink the LED.

You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Pi. The last one is a "stop" message that tells the application to stop running.

![application with gulp command and blink messages](/images/lab2_gulp_blink.png)

## Summary
You’ve successfully sent messages from your IoT hub to Pi to blink the LED. 

In the **[next lab][nextlab]** you will change the on/off behavior of the LED.

---

Back to [Lab 2 - IoT with Linux and Node.js](/content/lab-2-linux-node-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)


---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato10.jpg)

[nextlab]: /content/lab-2-5-change-the-on-and-off-behavior-of-the-led.md