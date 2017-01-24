# Lab 2.2: Create your IoT hub and register device

### What you will do
* Create a resource group.
* Create your Azure IoT hub in the resource group.
* Add Raspberry Pi 3 to the Azure IoT hub by using the Azure command-line interface (Azure CLI).

When you use Azure CLI to add Pi to your IoT hub, the service generates a key for Pi to authenticate with the service.

### What you will learn
In this article, you will learn:
* How to use Azure CLI to create an IoT hub
* How to create a device identity for Pi in your IoT hub

### What you need
* An Azure account
* A Mac or a Windows computer with the Azure CLI installed

### Create your IoT hub
Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets. To create your IoT hub, follow these steps:

1. Sign in to your Azure account by running the following command:

   ```bash
   az login
   ```

   All your available subscriptions are listed after a successful sign-in.

2. Set the default subscription that you want to use by running the following command:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name` can be found in the output of the `az login` or the `az account list` command.

3. Register the provider by running the following command. Resource providers are services that provide resources for your application. You must register the provider before you can deploy the Azure resource that the provider offers.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Create a resource group named iot-sample in the West US region by running the following command:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus` is the location you create your resource group. If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.
 
5. Create an IoT hub in the iot-sample resource group by running the following command:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   By default, the tool creates an IoT Hub in the Free pricing tier. For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE] 
> The name of your IoT hub must be globally unique. You can create only one F1 edition of Azure IoT Hub under your Azure subscription.

### Register Pi in your IoT hub
Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID. You will use Azure CLI to register your Pi and create a self-signed X.509 certificate for device authentication.

Run the following command:

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

### Summary
You've created an IoT hub and registered Pi with a device identity in your IoT hub. You're ready to learn how to send messages from Pi to your IoT hub.


In the **[next lab][nextlab]** you will start sending messages, from your device to IoT Hub.

---

Back to [Lab 2 - IoT with Linux and Node.js](/content/lab-2-linux-node-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato08.jpg)

[nextlab]: /content/lab-2-3-send-device-to-cloud-messages.md
