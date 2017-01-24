#Lab - Getting Started

### Preparing for the IoT Labs
The **#ptiotlabs** build on each other to enable you to prototype your own Internet of Things (IoT) devices. You will use several technologies like Windows, .Net Framwork, Linux, Node.JS or Node-RED. Let's start preparing things for Windows IoT Core. You will build an application for the Universal Windows Platform (UWP) that can run on any Windows 10 device, in this case a Raspberry Pi 3. But remember there are other options like Intel Joule, MinnowBoard MAX, or a DragonBoard 410c running Windows 10 IoT Core.

### Bill of Materials
For the IoT Labs you will need the following:

1. [Raspberry Pi 3](https://www.adafruit.com/products/3055) with a [5V 2A Switching Power Supply w/ 20AWG 6' MicroUSB Cable](https://www.adafruit.com/product/1995)
2. [Solderless Breadboard](https://www.adafruit.com/products/64)
3. [Jumper wires (Male to Male)](https://www.adafruit.com/product/1957)
4. [Jumper wires (Male to Female)](https://www.adafruit.com/product/1954)
5. [Photoresistor](https://www.adafruit.com/products/161)
6. [Red LED](http://www.adafruit.com/products/297)
7. [330 Ohm resistors](http://www.amazon.com/E-Projects-Resistors-Watt-330R-Pieces/dp/B00BVOR6IS/)
8. [10k Ohm resistor](http://www.amazon.com/E-Projects-10k-Resistors-Watt-Pieces/dp/B00BWYS9BA/)
9. One of the following analog-digital-converters:
    * [MCP3002 - 2-Channel 10-Bit ADC with SPI Interface](https://www.sparkfun.com/products/8636)	
	* [MCP3008 - 8-Channel 10-Bit ADC with SPI Interface](https://www.adafruit.com/products/856)
    * [MCP3208 - 8-Channel 12-Bit ADC with SPI Interface](http://www.digikey.com/product-detail/en/MCP3208-CI%2FSL/MCP3208-CI%2FSL-ND/305929)
10. 8GB micro SD card - class 10 or better. Microsoft suggests one of the following:
	* [Samsung 32GB EVO Class 10 Micro SDHC up to 48MB/s with Adapter (MB-MP32DA/AM)](http://www.amazon.com/gp/product/B00IVPU786)
	* [SanDisk Ultra Micro SDHC, 16GB Card](http://www.amazon.com/SanDisk-Ultra-Micro-SDHC-16GB/dp/9966573445).

**Note:** All of these items, with the exception of the Raspberry Pi 3, are included in the [Microsoft IoT Pack for Raspberry Pi 3](http://www.adafruit.com/windows10iotpi2) from AdaFruit.

### Install Visual Studio 2015
If you don't already have it installed, install **[Visual Studio 2015](https://www.visualstudio.com/)**. You can use the free [Community Edition](https://www.visualstudio.com/post-download-vs/?sku=community&clcid=0x409&telem=ga), or any other higher edition. When you are installing Visual Studio, you must do a **Custom** install and select to install the **Universal Windows App Development Tools -> Tools and Windows SDK**. 

![Install Developemnt Tools](/images/labs-install-dev-tools.png)

After the installation is complete, install the Windows IoT Core Project Templates from [here](https://visualstudiogallery.msdn.microsoft.com/55b357e1-a533-43ad-82a5-a88ac4b01dec).

### Enable Developer Mode on your Windows 10 Development Device
When you are developing on Windows 10, you choose what tasks you want to enable on the device. This includes any devices - Windows 10 desktops, tablets and phones. You can enable a device for development, or just app side loading. To enable *Developer mode* on your Windows 10 device:

1. Click the Windows icon (typically in the lower-left of the screen, on the left-most side of the toolbar). 
2. Type **Update** and select *Windows Update settings* from the *Best match* list. This will open the **UPDATE & SECURITY** settings page. 
3. Click on **For developers** in the left sidebar.
4. Ensure the **Developer mode** radio button is selected.
5. Save your changes and close the *Settings* window.  

### Conclusion &amp; Next Steps
In this Getting Started section you prepared your development machine for the subsequent labs. The next step is to configure your prototyping board to become a connected Thing. 

---

Advance to **[Setting your Raspberry Pi][nextlab]**.

Back to [IoT Labs homepage](/readme.md)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato02.png)

[nextlab]: /content/labs-setting-up-your-rpi.md