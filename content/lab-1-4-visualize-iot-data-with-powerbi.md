# Lab 1.4 - Visualize IoT Data with Power BI

In this lab you will use Azure Stream Analytics to process the data that you are sending into Azure IoT Hub so that you can visualize it using Power BI.

In an [earlier lab](../setup-azure-iot-hub/) you provisioned an Azure IoT Hub and in the [previous lab](../sending-telemetry/) you built a physical device...a _Thing_. You coded an application to collect
data from the device and send it to your IoT Hub. At the end of the previous lab you had data going into your IoT Hub but you weren't utilizing that data. This lab will change that.

# Using Stream Analytics to Process and Route IoT Data
Azure Stream Analytics is a service that does real-time data processing in the cloud. You will create a new Stream Analytics job and define the 
input data stream as the data coming from your IoT Hub. Next you will define an output data stream that sends data to Power BI. Finally, you 
will write a SQL-like query that collects data coming in on the input stream and routes it to the output stream. 

## Create the Stream Analytics Job
Open a new browser tab and navigate to [https://portal.azure.com](https://portal.azure.com). Login if necessary. Click on the __NEW__ icon in the lower-left corner.

![Windows Azure Portal v1](/images/photon_lab07_5.png)

1. Select __DATA SERVICES__ > __STREAM ANALYTICS__ > __QUICK CREATE__ and enter the following:

 - JOB NAME: You can use any name you like. (e.g. __iotlab__)
 - REGION: If you created your IoT Hub in _East US_, select __East US 2__. For other regions, select the same region you created your IoT Hub in.
 - REGIONAL MONITORING STORAGE ACCOUNT: Select or create a storage account.

![Defining a new Stream Analytics job](/images/newasa.png)

2. Click _CREATE STREAM ANALYTICS JOB_. It will take a few minutes for the Steam Analytics job to get created and become available. 

![Creating a new Stream Analytics job](/images/asajobcreated.png)

When the job indicates that it is created, click into it to create the data streams and query. Once you are in the Stream Analytics job you will need to define the job input, query and output. 

## Define the Input Data Stream
The data will come in as a data stream from the Event Hub that was automatically created when you created the Azure IoT Hub. 

1. Click on the __INPUTS__ header.

![Create the input](/images/asainputs.png)

2. Click on _ADD AN INPUT_.
3. Select __Data stream__ and click on the forward arrow in the lower-right.
4. Select __IoT Hub__ and click on the forward arrow in the lower-right.
5. Complete the form as follows:

 - INPUT ALIAS - __DeviceInputStream__
 - SUBSCRIPTION - Choose your subscription.
 - CHOOSE AN IOT HUB - Choose the IoT Hub you created earlier.
 - IOT HUB SHARED ACCESS POLICY NAME - Leave this as the default, which should be _iothubowner_.
 - IOT HUB CONSUMER GROUP - Select _Create a new consumer group_ and name it __AnalyticsConsumerGroup__.

6. Click on the forward arrow in the lower-right.
7. On the _Serialization settings_ form, leave the defaults (Event Serialization Format:JSON and Encoding:UTF8) click on the checkmark in the lower-right. 

![Stream Analytics input definition](/images/asainputform.png)

After a few seconds, a new input will be listed.

## Define the Output Data Stream
Before defining the query that will select data from the input and send it to the output you need to define the output. You will output the results of the query to a Power BI dataset for reporting.

1. Click on the _OUTPUTS_ header.

![Create the output](/images/asaoutputs.png)

2. Click on __ADD AN OUTPUT__.
3. Select __Power BI__ and click on the forward arrow in the lower-right.
4. Follow the instructions for either _Existing Microsoft Power BI User_ or _New User_ using your __@microsoft.com__ email account.

<blockquote>
Power BI is a data visualization toolkit for organizations. To create a new user account, you will have to use an account that belongs to an 
organization, such as your place of employment. You will not be able to create a new user account using an email address that ends in 
Outlook.com, Hotmail.com, GMail.com or other general email provider accounts.
</blockquote>

5. After you have authorized the connection to Power BI, complete the form as follows:

 - OUTPUT ALIAS - __DeviceBI__
 - DATASET NAME - __MyIoTDataSet__
 - TABLE NAME - __MyIoTDataTable__
 - GROUP NAME - __My Workplace__

6. Click on the checkmark in the lower-right.

![Stream Analytics output definition](/images/asaoutputform.png)

### Write the Query
In the query, you want to select data from the input stream and put it into the output stream. With data like _darkness_ you can do interesting things like apply operations on the data as you query it. For this example, you will write a query that selects from the input stream and sends the output stream the minimum, maximum, and average darkness values across all devices. You will be able to group the data by either location or device ID. Using a <code>TumblingWindow</code> you will send data to the output stream in rolling increments of 5-seconds.

1. Click on the _QUERY_ header.

![Create the query](/images/asaquery.png)

2. Write the following query:

{% highlight sql %}
SELECT
    MAX(messurementValue) MaxDark,
    MIN(messurementValue) MinDark,
    AVG(messurementValue) AvgDark,
    location,
    deviceId,
    System.Timestamp AS Timestamp
INTO
    [DeviceBI]
FROM
    [DeviceInputStream]
WHERE
    [messurementType] = 'darkness'
GROUP BY
    TumblingWindow (second, 5), deviceId, location 
{% endhighlight %}

3. Click _SAVE_ in the lower middle of the screen. 
4. Once the query is saved, click _START_ to start the Stream Analytics job. 

If your app from the [previous lab](../sending-telemetry/) isn't still running, go ahead and start it up. It will take a few minutes for the Stream Analytics job to get started and to start sending data to Power BI, but you should see _MyIoTDataSet_ show up in Power BI within a few minutes. Remember, the _TumblingWindow_ is set to 5-seconds, so PowerBI will only update every 5-seconds.

# Build Reports in Power BI
Go back to the browser tab where you have Power BI open. Look in the _Datasets_ node in the left-hand navigation. The _MyIoTDataSet_ should appear there within a few minutes of IoT Hub data streaming into the Stream Analytics job. 

1. Click on the _MyIoTDataSet_ dataset to open the report designer.
2. Select the __Line__ chart from the _Visualizations_ toolbox on the right side.
3. Select __maxdark__ to set it as the _Value_.
4. Click on the dropdown arrow for _maxdark_ in the _Values_ box and select __Maximum__.
5. Select __timestamp__ to set it as the _Axis_.

Repeat steps 2-4 for __avgdark__ and __mindark__, changing their field type to __Average__ and __Minimum__ respectively.

![Create the Power BI report](/images/RPi3/powerbi01.png)

As you are setting the values you should see the line chart updating with the changes. Click _File_ > _Save_ and give the report the name __Darkness Report__. Hover over the upper-right corner of the chart and click on the pin icon. Create a new dashboard to pin the gauges to. Once you have pinned all three gauges, click on the dashboard in the left sidebar. On the dashboard you can watch the data update in near real-time. While you are watching the dashboard, pinch or blow on the sensors on the weather shield and you will see the data change in the gauges.

# Conclusion
In this lab you learned how to create an Azure Stream Analytics job to query data coming in to Azure IoT Hub, process it and send it to Power BI. In Power BI you learned how to create reports and pin the data visualizations to a dashboard for near real-time updates.

Congratulations! In this hands-on workshop you experienced an IoT solution end-to-end. You built a _Thing_ that both sent output (blinked an LED) and collected input (ambient light) and used it to send data to Azure IoT Hubs. You then passed that data stream into Azure Stream Analytics, queried it, and sent the output to Power BI where you created a visualization of the IoT data.

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato06.jpg)

