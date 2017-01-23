#Lab - Setting up your Raspberry Pi 3

If you haven't already done so, please follow the instructions in the ['Getting Started'](../getting-started/) section.

In this lab you will install Windows 10 IoT Core on your Raspberry Pi 2. 

# Install Windows 10 IoT Core on Raspberry Pi 2
Windows 10 IoT Core is a version of Windows 10 designed to run on small devices, like the Raspberry Pi 2. You can download and install the Windows IoT Core image onto the micro SD card. 

1. In a browser, navigate to the [Microsoft Windows IoT Downloads &amp; Tools page](http://ms-iot.github.io/content/en-US/Downloads.htm){:target="_blank"}. 
2. Click on the __Get IoT Core Dashboard__ button to download the dashboard utility.
3. Once the __setup.exe__ has downloaded, open it to install the dashboard utility.

Once the Windows IoT Core Dashboard is installed, use it to flash the microSD card with the latest Windows IoT Core OS image.

1. Insert the micro SD card into your SD card writer.
2. In the IoT Dashboard, select _Set up a new device_.

![Set up a new device](/images/rpi2/dashboard-setup01.png)

3. Use the dropdown list to make _Device Type_ = __Raspberry Pi 2__.
4. Select __Windows 10 IoT Core for Raspberry Pi 2__.
4. Select the microSD card in the _Drive_ dropdown list.
5. Check the box for _I accept the software license terms_ (of course you have already read them).
6. Click the __Download and install__ button.

![Download and install](/images/rpi2/dashboard-setup02.png)

7. When prompted, select __Continue__ indicating that you have backed up any files from the microSD card before it gets erased.

![Accept License](/images/rpi2/dashboard-setup03.png)

The download and install could take some time depending on your bandwidth and the size of the microSD card. Whe it is complete you will see the _You SD card is ready_ screen.

![Your SD card is ready](/images/rpi2/dashboard-setup04.png)

# Connect the Raspberry Pi 2
You are now ready to connect and power on your Raspberry Pi 2.

1. Insert the micro SD card with the Windows 10 IoT Core image on it into your Raspberry Pi 2. (The slot is on the underside, on the opposite edge from the side with the USB ports.)
2. Connect a network cable from your local network to the Ethernet port on the Raspberry Pi 2. Your development device must be on the same network.
3. Connect an HDMI monitor to the HDMI port on the board.
4. Connect the power supply to the micro USB port on the board. You must power this from the 5V 2A adapter - USB power from your computer is insufficient.

Windows 10 IoT Core will boot on power-up. The first boot may take a few minutes.

![Default app](/images/rpi2/rpi2_defaultapp.png)

# Set the Machine Name and Password
Once the Raspberry Pi 2 has booted up, it will appear in the _My devices_ tab of the IoT Dashboard - by default it will have the name __minwinpc__ and an Administrator password of __p@ssw0rd__. It is recommend that you change both of these to avoid confusion with other devices on your network.

1. Click on the stylus icon to edit the Raspberry Pi 2.
2. Change the password if you would like.
3. Change the name to something unique (remember, you are naming a Windows device - all normal naming rules and practices apply).
4. Click __Accept__ to accept the changes and reboot the RPi2

Your Raspberry Pi 2 will reboot and when it is back up and running it will have the new name you gave it, and the Administrator account will use the password you created. 

# Conclusion &amp; Next Steps
In this section you prepared your Raspberry Pi 2 for the subsequent labs. Next you will do the IoT equivalent of a 'Hello, World!' program: you will make an LED blink.

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato01.png)