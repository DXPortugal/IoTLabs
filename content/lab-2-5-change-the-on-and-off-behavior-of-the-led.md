# Lab 2.5: Change the on and off behavior of the LED

### What you will do
Customize the messages to change the LED’s on and off behavior. 

### What you will learn
Use additional Node.js functions to change the LED’s on and off behavior.

### What you need
You must have successfully completed [Send cloud-to-device messages](/content/lab-2-4-send-cloud-to-device-messages.md).

### Add Node.js functions
1. Open the sample application in Visual Studio code by running the following commands:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Open the `app.js` file, and then add the following functions at the end:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![App.js file with added functions](/images/lab2_updated_app_js.png)
3. Add the following conditions before the default one in the switch-case block of the `receiveMessageCallback` function:
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   If you want to guarantee the LED is turned off when the execution is over you must change the case 'stop' statement to:
      ```javascript
    case 'stop':
      stopReceivingMessage = true;
      turnOffLED();
      break;
   ```
   
   Now you’ve configured the sample application to respond to more instructions through messages. The "on" instruction turns on the LED, and the "off" instruction turns off the LED.
4. Open the gulpfile.js file, and then add a new function before the function `sendMessage`:
   
   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```
   
   ![Gulpfile.js file with added function](/images/lab2_updated_gulpfile.png)
5. In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Save all the changes.

#### Deploy and run the sample application
Deploy and run the sample application on Pi by running the following command:

```bash
gulp deploy && gulp run
```

You should see the LED turn on for two seconds, and then turn off for another two seconds. The last "stop" message stops the sample application from running.

![Sample application with on and off messages](/images/lab2_gulp_on_and_off.png)

**Congratulations!** You’ve successfully customized the messages that are sent to Pi from your IoT hub.

### Summary
This lab demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.


---

Back to [Lab 2 - IoT with Linux and Node.js](/content/lab-2-linux-node-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

