+++
date = '2025-06-24T11:02:44-07:00'
draft = false
title = 'BLE Flutter Explanation'
description = "Learn how to make a Flutter BLE Scanner and read data from a peripheral"
+++

## Introduction

With the recent explosion of IoT popularity Bluetooth, and more specifically Bluetooth low Energy (BLE) is a  mainstream tech addition that everyone adores - however its documentation in the flutter space is **NOT** beginner friendly. I spent a lot of time scouring blog posts, and the main [Flutter blue plus docs](https://pub.dev/packages/flutter_blue_plus) to do the essential functions most people want to do. Those being:
- Get local bluetooth devices
- Connect to a bluetooth device
- Retrieve data from the bluetooth device.

Knowing that other people would come acrosss the same issues I faced - I decided to create this blog post to showcase, explain, and promote my code as a learning resource for creating BLE applications in flutter.

Before we start I want to provide my code incase you want to skip my blog and just see a working example. [My code](https://github.com/Sudo-Victor-Victory/BLE_Demo)
However, I will go more in detail here and provide some important background.
## What even is Bluetooth Low Energy (BLE) and why should I care
<html>
<head>
<style>
.blue-list {
    color:     #0096FF
 ;      /* Change text color */
    font-size: 18px; /* Change text size */
    margin: 0;       /* Remove default margin for nested lists */
    padding-left: 20px; /* Add some indentation for nested lists */
}



</style>
</head>
<body>
<p>
Traditional Bluetooth (also known as Bluetooth classic) is a short range wireless communication protocol for transmitting data from one electronic device to another. It was invented in 1994 and has serveral updates since.
</p>

<p>

BLE is Bluetooth Classic's newer offspring - made in 2010 as a part of the Bluetooth 4.0 update. It was made to address Bluetooth Classic's power consumption and inefficiency. You could power a beacon peripheral, a device that constantly makes its location known, for up to 1-2 years off a single coin cell battery with BLE. That is INSANE.
 [Here's the article & data](https://web.archive.org/web/20200711023145/https://www.aislelabs.com/reports/beacon-guide/)
</p>


You should care because BLE is everywhere around you. 
- Smart watch? BLE.  
- Wireless headphones/earbuds with a decent battery life? BLE.
- Ring Cameras? You guess it - BLE.
- Any "Smart" device that has a mobile app that displays data in real time?  **You already know.**

## Important disclaimers before we continue.
Any bluetooth device (BLE or classic) sends data between two devices. Typically one device is a peripheral/server (a sensor, smart device, the thing you are connecting to) that sends data. There is also the client - the one that receives the data.

This code is the client. That means
<ul class="blue-list">
    <li>While I am posting and discussing the Flutter code - keep in mind that the code is acting as the client. It is receiving not sending data it is retrieving and displaying data.</li>
    <li>Permissions are a pain. If you are running this on android version 12 or under, you may face some compatability issues. Check the github readme for more details.</li>
    <li>This is for android only at the moment, I don't have an Iphone and therefore can't develop a version for IOS. Thanks Apple 💖 </li>
    <li>You PROBABLY cannot run this on an emulator. Most emulators do not simulate BLE connectivity, so you should connect your phone to whatever development environment you have and run it there.</li>
    <li>This is for FlutterBluePlus version 1.35.5 - always be sure to check the patch notes for any differences between this and your version. The general idea should still remain though.</li>
</ul>

Cool lets talk code. FINALLY.

</body>
</html>

## The code discussion and explanation
The code for the widget is in `/ble_demo/lib/views/widgets/ble_widget.dart`

I will be discussing the main components of getting Bluetooth devices, connecting to one, and getting data from it, alongside some **GOTCHAS** I encountered.

The BLE widget is a widget (UI) that scans for, lists, connects to a bluetooth device via BLE, and displays the server uptime from my peripheral. I decided to use an ESP32 since it has Bluetooth & BLE capabilities, and I had already been learning embedded so it works out. 

The widget is stateful - because we want to dynamically displays the BLE devices near us, and update the screen with any data from the peripheral. 

## <span style="color:#D70040">GOTCHA #1 - 2 concurrent scans</span>
```dart
bool _isScanningForBluetoothDevices = false;
```
Is used in the scanning function to prevent 2 scans from happening at the same time. If you did not check and ran 2 bluetooth scans on a device like android you'd either come across an `IllegalStateException` or get scan options ommitted. That and it'd take more power and we want to conserve battery life.




##  <span style="color:#D70040">GOTCHA #2 - Not checking the status of the adapter</span>
```dart
final state = await FlutterBluePlus.adapterState.first;
```
Within  `Future<void> _initBle() async {}` we check the adapter state's first. The reason why is to confirm the bluetooth adapter's state (The adapter communicates with the actual bluetooth chip)'s status within the app. We check the 1st data read from the adapter because we are initializing our app's access of it and want to verify that our access isn't hindered by permissions or anything from our app. Without it, it'd be increadibly difficult to diagnose issues with connectivity later on. 

## <span style="color:#00FFFF">EXPLANATION #1 - Checking and initing BLE </span>
```dart

  Future<void> _initBle() async {
    try {
      await _requestPermissions();
      // Allows us to scan for BLE dev. if the initial state
      // of the Bluetooth adapter is on. Otherwise the adapater has issues.
      final state = await FlutterBluePlus.adapterState.first;
      if (state == BluetoothAdapterState.on) {
        _listenScanResults();
        await _startScan();
      } else {
        print("Bluetooth is not ON");
      }
    } catch (e) {
      print("BLE init error: $e");
    }
  }
```
This is considered the entrypoint for our BLE connectivity. The flow of logic is to 
- Ask for permissions (in my case - Bluetooth Scan, Bluetooth Connect, and location)
- Confirm with the bluetooth adapter that it is on and functioning
- Scan for bluetooth devices

## <span style="color:#00FFFF">EXPLANATION #2 - Streaming (gathering) devices </span>
```dart
  void _listenScanResults() {
    // Updates _scanSubscription based on the stream of scan results
    _scanSubscription = FlutterBluePlus.scanResults.listen((results) {
      if (!listEquals(_bluetoothDevices, results)) {
        // Rebuilds the widget with the new list of devices
        setState(() => _bluetoothDevices = List.from(results));
      }
    });
  }
  ```
An important but small section. We use the the static member `FlutterBluePlus` which we get for free from importing the library in this file. That took me a long time to figure out because apparently they changed it over the past few versions of FlutterBluePlus which meant no blogs or resources used it in that way.
We `listen` to a stream (an asynchronous pipeline of data that gets delivered over time) the results of which are a list of bluetooth devices. That's our first big step!!!!
The if clause is used to determine if there are any new bluetooth devices we had not already listed, to rebuild the widget with the new devices listed. 

## <span style="color:#00FFFF">EXPLANATION #3 - Finding devices </span>
```dart
  Future<void> _startScan() async {
    if (_isScanningForBluetoothDevices) {
      return;
    }
    // A scan started - therefore we must set the bool to prevent dup. scans.
    setState(() => _isScanningForBluetoothDevices = true);
    try {
      await FlutterBluePlus.startScan(timeout: const Duration(seconds: 10));
    } catch (e) {
      print("Scan failed: $e");
    } finally {
      setState(() => _isScanningForBluetoothDevices = false);
    }
  }
  ```
  This could count as a GOTCHA. in `Future<void> _initBle() async {}` we call `_listenScanResults();` right before calling `await _startScan();`. The reason is simple - we want to start listening to the stream before any data is sent to it - so that no data is lost. If we started the scan and listened afterwards, there would be no data to be read within the stream!
  On the topic of GOTCHAS - here we set the `_isScanningForBluetoothDevices` variable to true akin to prevent <span style="color:#D70040">GOTCHA #1</span>

  So far everything has been for the first step - listening bluetooth devices. Thankfully one function does the connecting and retrieving of data.

## <span style="color:#00FFFF">EXPLANATION #4 - Connecting to a device</span>
```dart
    Future<void> _connectToDevice(BluetoothDevice device, String name) async {
    try {
      if (_isScanningForBluetoothDevices) {
        await FlutterBluePlus.stopScan();
        setState(() => _isScanningForBluetoothDevices = false);
      }

      print('Connecting to ${device.remoteId}');
      await device.connect(timeout: const Duration(seconds: 10));
      print('Connected to ${device.remoteId}');
```
Thankfully `FlutterBluePlus` made it very simple to **connect** to a device. Since we already have a device list when you tap on one, you can pass it in as a paramter and simply connect to it. That is awesome!
The IF is used as insurance - Bluetooth doesn't like to scan and connect at the same time. So if there is a scan in progress we stop in order to connect to the device. There's also a timeout incase the bluetooth peripheral was facing issues among other problems.

## <span style="color:#00FFFF">EXPLANATION #5 - Getting data from the Peripheral</span>
The final one!!!!! 
```dart
      List<BluetoothService> services = await device.discoverServices();
      BluetoothCharacteristic? targetCharacteristic;

      for (var service in services) {
        if (service.uuid.str == serviceUuid) {
          for (var c in service.characteristics) {
            if (c.uuid.str == characteristicUuid) {
              targetCharacteristic = c;
              break;
            }
          }
        }
      }
      if (targetCharacteristic != null) {
        List<int> value = await targetCharacteristic.read();
        print("Read value: $value");

        decodedValue = String.fromCharCodes(value);
      }
```
This is the second half of `_connectToDevice` within Explanation 4. Thankfuilly this one is also pretty simple.
I will make another blog post explaining GATT in more detail, but this image will suffice 


![GATT protocol explanation](/github-portfolio/images/ble_flutter_blog/GATT_Diagram.svg "Explanation of GATT services between a client and a server")

GATT is specifically *how*  BLE devices organize and transfer data between one another. The Server/Peripheral has a Service (A group of data) and a Characteristic (A container of data). If you want to read data from a Peripheral - you got to know what Service the data is in, the characteristic its in, and if it has the permissions to be read.

The two for loops iterate through the services and characteristics to find the ones I had explicity mentioned in my code. 

Afterwards - we check if we found the characteristic we were looking for by verifing it wasn't `null` and access its data `List<int> value = await targetCharacteristic.read();`
Which is HUGE. 


# Summary
BLE is an awesome technology you should be developing with. It allows you to asynchronously get data from a peripheral/server to your client which is a mobile app. Be sure to check the adapter's status, permissions, then stream all the data you want. Thank you for taking a look and I hope this helped 💗
