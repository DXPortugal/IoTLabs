# Lab 1.1 - Hello Windows IoT

In this lab you will create a simple *Thing* using the Universal Windows Platform on a Windows 10 IoT device. 

## Wire Up the Device
The Raspberry Pi 3 connects to the physical world through the GPIO pins. GPIO stands for General Purpose Input/Output and refers to the two rows of pins on the Raspberry Pi 3. The GPIO pins are a physical interface between the Raspberry Pi 3 and the physical world. Through your app you can designate pins to either receive input or send output. The inputs can be from switches, sensors or other devices. The outputs can be LEDs, servos, motors or countless other devices. Twenty-six of the 40 pins are GPIO pins; the others are power, ground, or reserved pins.

![Raspberry Pi 3 pin Map](/images/labs-rp2-pinout.png)

Wire the Raspberry Pi 3 to  the solderless breadboard as  described below.

![Blinky/Hello, World Wiring](/images/lab1_rpi2-hello-windows-bb.png)

1. GPIO 12 is connected to the positive (longer) lead on the LED. In the app you will build in this workshop, you will control whether or not GPIO pin 12 sends voltage over the circuit.
2. The negative (shorter) lead on the LED is connected to a resistor to reduce the amount of voltage pulled through the circuit.
3. The other end of the resistor is connected to one of the ground GPIO pins, completing the circuit.

The LED will light up when current is passed through the circuit by the app you will build. Because that app isn't created yet, the LED doesn't do anything right now.

# Create an Application using the Universal Windows Platform
A Universal Windows app is a Windows experience that is built upon the Universal Windows Platform (UWP), which was first introduced in Windows 8 as the Windows Runtime. The UWP enables you to write an app that targets a device family, such as IoT devices. In fact, the Universal app that you write may be able to run on multiple devices families, depending on the device characteristics that it takes advantage of. In this lab you will create a Universal app targeting IoT devices running Windows 10. In theory this could be nearly any device, such as a phone, a tablet or a Raspberry Pi 3. The Universal app you will write, however, will access the General Purpose Input/Output (GPIO) of the device, so the app won't be compatible with devices that don't have a GPIO.   

## Create a Blank Universal App

1. Launch Visual Studio and start a new **Blank App (Universal Windows)** (found in the *C# -> Windows -> Universal* node).
2. Name the application *HelloWindowsIoT*.

![Create a blank Universal Windows Application](/images/lab1_rpi2-new-universal.png)

## Add the Windows IoT Extensions for the UWP
The *Windows IoT Extenstions for the UWP* are not included in a new Blank Application by default. The IoT extensions enable namespaces, such as <code>Windows.Devices.Gpio</code> to be referenced and uses in the application. You must add a reference to the *Windows IoT Extensions for the UWP*.

 1. Click on the *Project* menu and select *Add Reference*.
 2. In the Reference Manager dialog, expand the *Universal Windows* node and select *Extensions*.
 3. In the list of extensions,check the box next to *Windows IoT Extensions for the UWP* and click **OK**. (Make sure to select the same version number as the OS running on the Raspberry Pi 3.) It is easy to accidently select the *Windows Mobile Extensions for the UWP*, (which is just below the IoT extensions) so take extra care to make sure you have added the correct reference.
 
![Add the Windows IoT Extensions for the UWP](/images/lab1_rpi2-install-iotextensions.png)

## Design the App UI
This application has a UI that duplicates what is happening with the hardware. (It has a blinking virtual LED on-screen.)

1. Open the *MainPage.xaml* file  (This is the layout definition for the initial page that loads when the app is run.) 
1. Replace the <code>&lt;Grid&gt;...&lt;/Grid&gt;</code> code with the following:

```csharp
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
        <Ellipse x:Name="LedGraphic" Fill="LightGray" Stroke="White" Width="100" Height="100" Margin="10"/>
        <TextBlock x:Name="DelayText" Text="500ms" Margin="10" TextAlignment="Center" FontSize="26"/>
        <TextBlock x:Name="GpioStatus" Text="Waiting to initialize GPIO..." Margin="10,50,10,10" TextAlignment="Center" FontSize="26"/>
    </StackPanel>
</Grid>
```

## Code the App Logic
Throughout this lab you will use a feature in Visual Studio called *light bulbs*. Light bulbs are a new productivity feature in Visual Studio 2015. They are icons that appear in the Visual Studio editor and that you can click to perform quick actions including refactoring fixing errors. Light bulbs bring error-fixing and refactoring assistance into a single focal point, often right on the line where you are typing. As you write the code in this lab you will add calls to methods that don't yet exist. The editor will indicate this to you by putting a red squiggle underline beneath the method call. When you hover over the offending code a light bulb will appear and you can expand it to see options for generating the missing method. 

### Add 'using' Statements
1. Open the *MainPage.xaml.cs* file. This is the code behind the layout for the MainPage.xaml. 
2. Add the following to the *using* statements to the end of the list of similar *using* statements at the top of the file. 

```csharp
// Enable asynchronous tasks
using System.Threading.Tasks;
// Enable access to the GPIO bus on the Raspberry Pi 3
using Windows.Devices.Gpio;
```

### Define Class-level Constants and Variables
There are a number of constants and variables that you will be using throughout this application. Define these in the <code>MainPage</code> class definition.
 
1. Locate the <code>public sealed partial class MainPage : Page</code> class definition. This defines a class called *MainPage* and inherits all of the capabilities of the *Page* class).
2. 2. Add the following constant and variable definitions:

```csharp
public sealed partial class MainPage : Page
{
        // Define the physical pin connected to the LED.
        private const int LED_PIN = 12;
        // Deifne a variable to represent the pin as an object.
        private GpioPin pin;
        // Define a variable to hold the value of the pin (HIGH or LOW).
        private GpioPinValue pinValue;
        // Define a time used to control the frequency of events.
        private DispatcherTimer timer;
        // Define a color brushes for the on screen representation of the LED.
        private SolidColorBrush redBrush = new SolidColorBrush(Windows.UI.Colors.Red);
        private SolidColorBrush grayBrush = new SolidColorBrush(Windows.UI.Colors.LightGray);

        public MainPage()
        {
            this.InitializeComponent();
            
            // TODO: Create an instance of a Timer that will raise an event every 500ms
        }
}
```

### Create a Timer to Control the LED
The LED connected to GPIO pin 12 will turn on and off at an interval defined by the <code>timer</code> object. The <code>TODO</code> comment is the placeholder for configuring the <code>timer</code>. Following the call to <code>InitializeComponent</code>, configure the <code>timer</code> to raise an event every 500ms. (Soon you will create an event handler that will do the real work.) 

1. Locate the <code>// TODO: Create an instance of a Timer that will raise an event every 500ms</code> comment in the <code>public MainPage()</code> constructor.
2. Replace the comment with the following code. 

```csharp
// Create an instance of a Timer that will raise an event every 500ms
timer = new DispatcherTimer();
timer.Interval = TimeSpan.FromMilliseconds(500);
timer.Tick += Timer_Tick;

// TODO: Initialize the GPIO bus
```

The <code>Timer_Tick</code> reference should have a red squiggle under it. This indicates that that event handler doesn't exist yet. 

### Handle the Timer.Tick Event
The <code>timer</code> will raise an event at the interval it was configured with, which is 500ms in this case. When the *Timer.Tick* event is fired the application will query the current state of the LED and if it is set to *Low* (off) then it will be changed to *High* (on) and the virtual LED on the screen will be filled with the color red. If the state is *High* then it will be switched to *Low* and the virtual LED will be filled with the color light gray. You will use <code>pin.Write(pinValue)</code> to execute the change on the GPIO bus.

You can create an event handler that will fire every time the *Timer.Tick* event is raised. You can do this using the Visual Studio *light bulb* refactoring tool. 

1. Hover the mouse over the *Timer\Tick* reference until a light bulb appears. 
2. Click the down arrow and select *Generate method 'MainPage.Timer_Tick'*. 

![Using Visual Studio light bulb refactoring](/images/lab1_rpi2-lab01-Timer-Tick.png)

3. Add the following code for the *Timer_Tick* event handler.

```csharp
private void Timer_Tick(object sender, object e)
{
    // This Timer event will be raised on each timer interval (defined above)
            
    if (pinValue == GpioPinValue.Low)
    {
        // If the current state of the pin is LOW (off), then set it to HIGH (on)
        // and update the on screen UI to represent the LED in the on state
        pinValue = GpioPinValue.High;
        LedGraphic.Fill = redBrush;
    }
    else
    {
        // If the current state of the pin is HIGH (on), then set it to LOW (off)
        // and update the on screen UI to represent the LED in the off state
        pinValue = GpioPinValue.Low;
        LedGraphic.Fill = grayBrush;
    }
    // Write the state to to pin
    pin.Write(pinValue);
}
```

Of course, <code>pin</code> is *null* - you haven't done anything more than simply define it thus far. (You defined it in the class-level variables as <code>private GpioPin pin;</code>.) You need to initialize the GPIO controller to get access to the <code>pin</code>.

### Initialize the GPIO Controller
The GPIO controller is the object that provides access to the GPIO bus. It exposes the GPIO pins that you are connecting physical things to. 

1. Replace the <code>// TODO: Initialize the GPIO bus</code> comment in the <code>public MainPage()</code> constructor with the following: 

```csharp
// Initialize the GPIO bus
InitGpioAsync();
```

2. Just like you did with the *Timer_Tick* code earlier, use the refactoring *light bulb* tool to generate the <code>InitGpioAsync()</code> method. In the *InitGpioAsync()* method you will get the instance of the default GPIO controller. If the GPIO controller instance is *null* then the device the app is running on doesn't support GPIO, and you will display a message on the screen indicating this, and that will be the end of the app functionality. If there is a GPIO controller instance, then you will use it to open the GPIO pin that you have connected to the LED and prepare it for use. Add a *null* check on the *pin* instance. If it is not *null*, go ahead and start the timer. The timer will begin invoking the *Timer_Tick* event every 500ms. 

<blockquote>
The GpioController.GetDefaultAsync() asynchronous method was added to the Windows IoT Extensions for the UWP in the 10.0.10586 version. If you are running Windows 10 (version 10.0.10240) or earlier, the GpioController.GetDefaultAsync() method will not work.
</blockquote>

Add the following code for the <code>InitGpioAsync()</code> method.

```csharp
private async Task InitGpioAsync()
{
    // Get the default GPIO controller
    var gpio = await GpioController.GetDefaultAsync();
    
    // If the default GPIO controller is not present, then the device 
    // running this app isn't capable of GPIO operations.
    if (gpio == null)
    {
        pin = null;
        GpioStatus.Text = "There is no GPIO controller on this device.";
        return;
    }
    
    // Open the GPIO channel
    pin = gpio.OpenPin(LED_PIN);

    // As long as the pin object is not null, proceed
    if (pin != null)
    {
        // Define the pin as an output pin
        pin.SetDriveMode(GpioPinDriveMode.Output);
        // Define the initial status as LOW (off)
        pinValue = GpioPinValue.Low;
        // Write the tate to the pin
        pin.Write(pinValue);
        // Update the on screen text to indicate that the GPIO is ready
        GpioStatus.Text = "GPIO pin is initialized correctly.";
        // Start the timer.
        timer.Start();
    }
}
```

Note that we added the <code>async</code> modifier to the method signature and changed the return type from <code>void</code> to <code>Task</code>. 

# Run the App on the Raspberry Pi 3
The application is ready to be deployed and run on the Raspberry Pi 3. You must set the target in Visual Studio to *ARM* and point the debugger at a *Remote Machine* (your Raspberry Pi 3).

1. Select **ARM** from the *Solution Platforms* list in the toolbar.
2. Select **REMOTE MACHINE** from the *Device* dropdown list in the toolbar.

![Targeting ARM on a remote machine](/images/lab1_rpi2-lab01-arm.png)

You will be prompted with the *Remote Connections* dialog. 

3. Select your device from the list of *Auto Detected* devices. (Note: The Raspberry Pi 3 you are working with is named <strong>ThingLabsXX</strong> where the XX is replaced with the number on your Raspberry Pi 3.) If your device is not listed, type your device's name into the *Manual Configuration* textbox.
4. Set the *Authentication Mode* to **Universal (Unencrypted Protocol)**.
5. Click *Select*.

![Choose the remote machine to deploy to](/images/lab1_rpi2-lab01-remote.png)

**NOTE:** You can verify or modify these values by navigating to the project properties (select Properties in the Solution Explorer) and choosing the Debug tab on the left.

6. Press **F5** to run the application and you should see it deploy on the Raspberry Pi 3. You will see the red LED blink in unison with the red circle on the screen. If the red LED is not blinking, but the display on the screen is, recheck your wiring. 

# Conclusion &amp; Next Steps

Congratulations! You have built a Universal Windows Platform application that controlled one of the GPIO pins and deployed the app to a Raspberry Pi 3. The core concepts you've learned are:

1. Building a Universal Windows Platform application that can run on any Windows 10 device. 
2. Testing for the existence of the GPIO controller to inform the application of what capabilities are accessible.
3. Controlling the state of a device via the GPIO pins. 

In the **[next lab][nextlab]** you will set up a Microsoft Azure IoT Hub that will act as the cloud backend for your IoT devices.

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato03.png)


[nextlab]: /content/lab-1-2-setting-up-an-azure-iot-hub.md
