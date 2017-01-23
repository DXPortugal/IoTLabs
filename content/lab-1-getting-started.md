#Lab - Getting Started

## Preparing for the Windows 10 IoT - Connected Nightlight Workshop
The labs in the Connected Nightlight Workshop build on each other to enable you to prototype your own Internet of Things (IoT) devices. You will use the Microsoft .NET Framework to build an application for the Universal Windows Platform (UWP) that can run on any Windows 10 device, including a Raspberry Pi 3, MinnowBoard MAX, or a DragonBoard 410c running Windows 10 IoT Core.

## Bill of Materials
In the Connected Nightlight Workshop you will need the following:

1. [Raspberry Pi 3](https://www.adafruit.com/products/3055){:target="_blank"} with a [5V 2A Switching Power Supply w/ 20AWG 6' MicroUSB Cable](https://www.adafruit.com/product/1995)
2. [Solderless Breadboard](https://www.adafruit.com/products/64){:target="_blank"}
3. [Jumper wires (Male to Male)](https://www.adafruit.com/product/1957){:target="_blank"}
4. [Jumper wires (Male to Female)](https://www.adafruit.com/product/1954){:target="_blank"}
5. [Photoresistor](https://www.adafruit.com/products/161){:target="_blank"}
6. [Red LED](http://www.adafruit.com/products/297){:target="_blank"}
7. [330 Ohm resistors](http://www.amazon.com/E-Projects-Resistors-Watt-330R-Pieces/dp/B00BVOR6IS/){:target="_blank"}
8. [10k Ohm resistor](http://www.amazon.com/E-Projects-10k-Resistors-Watt-Pieces/dp/B00BWYS9BA/){:target="_blank"}
9. One of the following analog-digital-converters:
    * [MCP3002 - 2-Channel 10-Bit ADC with SPI Interface](https://www.sparkfun.com/products/8636){:target="_blank"}	
	* [MCP3008 - 8-Channel 10-Bit ADC with SPI Interface](https://www.adafruit.com/products/856){:target="_blank"}
    * [MCP3208 - 8-Channel 12-Bit ADC with SPI Interface](http://www.digikey.com/product-detail/en/MCP3208-CI%2FSL/MCP3208-CI%2FSL-ND/305929){:target="_blank"}
10. 8GB micro SD card - class 10 or better. Microsoft suggests one of the following:
	* [Samsung 32GB EVO Class 10 Micro SDHC up to 48MB/s with Adapter (MB-MP32DA/AM)](http://www.amazon.com/gp/product/B00IVPU786){:target="_blank"}
	* [SanDisk Ultra Micro SDHC, 16GB Card](http://www.amazon.com/SanDisk-Ultra-Micro-SDHC-16GB/dp/9966573445).{:target="_blank"}

**Note:** All of these items, with the exception of the Raspberry Pi 3, are included in the [Microsoft IoT Pack for Raspberry Pi 3](http://www.adafruit.com/windows10iotpi2){:target="_blank"} from AdaFruit.

## Install Visual Studio 2015
If you don't already have it installed, install [Visual Studio 2015](https://www.visualstudio.com/){:target="_blank"}. You can use the free [Community Edition](https://www.visualstudio.com/post-download-vs/?sku=community&clcid=0x409&telem=ga), or any other higher edition. When you are installing Visual Studio, you must do a __Custom__ install and select to install the __Universal Windows App Development Tools -> Tools and Windows SDK__. 

![Install UWP](/images/RPi3/RPi3_install_uwp.png)

After the installation is complete, install the Windows IoT Core Project Templates from [here](https://visualstudiogallery.msdn.microsoft.com/55b357e1-a533-43ad-82a5-a88ac4b01dec){:target="_blank"}.

## Enable Developer Mode on your Windows 10 Development Device
When you are developing on Windows 10, you choose what tasks you want to enable on the device. This includes any devices - Windows 10 desktops, tablets and phones. You can enable a device for development, or just app side loading. To enable _Developer mode_ on your Windows 10 device:

1. Click the Windows icon (typically in the lower-left of the screen, on the left-most side of the toolbar). 
2. Type __Update__ and select _Windows Update settings_ from the _Best match_ list. This will open the __UPDATE & SECURITY__ settings page. 
3. Click on __For developers__ in the left sidebar.
4. Ensure the __Developer mode__ radio button is selected.
5. Save your changes and close the _Settings_ window.  

## Create a Microsoft Azure Trial Account
In this workshop you will use Microsoft Azure as the cloud backend for your IoT solution. If you don't already have an Azure account, go to [https://azure.microsoft.com/en-us/pricing/free-trial/](https://azure.microsoft.com/en-us/pricing/free-trial/){:target="_blank"} to start a free trial of Microsoft Azure. You may need a credit card for identity verification, but the trial is completely free. If you have an MSDN Subscription you may be eligible for free credits to Microsoft Azure every month. Check your [MSDN account](https://msdn.microsoft.com/subscriptions/manage/){:target="_blank"} page for details.

# Conclusion &amp; Next Steps
In this Getting Started section you prepared your development machine for the subsequent labs in the workshop. The next step is to configure your prototyping board to become a connected Thing. 

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md)


---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato02.png)