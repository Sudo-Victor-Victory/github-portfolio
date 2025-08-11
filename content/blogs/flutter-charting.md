+++
date = '2025-08-08T12:44:44-07:00'
title = 'Charting in Flutter'
draft = false
description = "Discussing and showcasing my ECG charted in real time on a flutter app."
author = 'Victor Rodriguez'
image = '/github-portfolio/images/flutter_charting_blog/My_charted_ecg.gif'
+++

## Yes that is my actual ECG. On my phone. 
Being read from an ESP32 + SparkFun AD8232, being sent over BLE (bluetooth low energy), and being charted </br> on my phone </br> in real time. </br> THAT'S AWESOME.


## That's cool. How'd you chart it?
The short answer is Syncfusion. However, I want to have a discussion of how I got here.

## Architecture / Project summary
{{< zooming src="/github-portfolio/images/flutter_charting_blog/Project_Architecture.png" alt="An image showing a the design of my project. SparkFun AD8232 to, to ESP32, with multi-coring using FreeRTOS, and using BLE to send the data to a flutter client" caption="The architecture of my project so far" >}}

This is my full system design. 

The flow of logic is that we start with the SparkFun AD8232, reading ECG data from a user using its leads. The Task ECG reads that data, filters the data using FIR and Kalman, and batches + inserts the data into a queue. All the while Task BLE is running and reads data from the queue, and if there is data within the queue, it will go ahead and send it to a connected client. The ESP32 is what manages memory management, timing, power, and ESP32 libraries that are used to make this project work. 


## RTOS? Batching? Why?
When it comes to timing - Task ECG runs every 4ms but sends data to the queue every 40ms. Task BLE runs every 10ms. This was intentional. The reason being that if data that comprises an ECG is not sampled at a consistent rate, you'll distort the waveform. However the ESP32's bluetooth chip can only really handle ~8ms consistently.

Alongside that, having two essential functions share memory and resources on the same core would have 100% caused interference to and from one another - further reducing the stability of the project.

Therefore RTOS and batching were the solutions. ESP32 natively runs FreeRTOS under the hood, so using its functions to make one task belong to one core was simple. Batching also made sense - instead of sending ECG data every 4ms - we'd collect 10 and transfer it every 10ms (realistically every 50ms since it only activates if the queue has 10 values or more). The ESP32 can handle a slightly larger transmission package every 50ms. Problem solved. 


## Integration and project flow

- At the beginning of my foray into BME/Embedded, I created an ESP32 BLE server to *prove* I can send data over BLE.
- Next, I created a Flutter client for the BLE server to send data to. 
- I stared with a soldering a SparkFun AD8232 chip with pin headers to it up to an ESP32 to capture ECG samples. 
- From there I utilized filtering techniques like FIR and Kalman to further reduce the noise/motion artifacts present within the system. 
- Afterwards I modified my original ESP32 BLE server to be incorporated into my project.
- I then integrated my ECG sampling and BLE server into FreeRTOS tasks to make my project a dual-processor setup to make each task isolate, reliable, and interfere with one another less. 
- Similarly, I also introduced an inter-process FreeRTOS queue to allow TaskECG to queue data, and TaskBLE to dequeue and send the data to a client.
- Subsequently, to remedy an issue relating to the queue, I implemted batchinng and fixed some sampling rates.
- Lastly, I took my initial BLE client made in flutter and modified the way it read BLE data, connected it to a SyncFusion chart - and BAM. Its done!

## Here are some bad charts before getting to the final version.
Initially I tried to use FL_Chart. While it can make a lot of really beautiful charts (pie, bar, etc.) - it is not optimized for real time charting or modification of the X axis. So it lead to having this beauty of a chart right here. Even though I forgot to record it, it speaks for itself.

{{< zooming src="/github-portfolio/images/flutter_charting_blog/Bad_FL_chart.jpg">}}




Afterwards I switched to using Syncfusion which not only was optimised for real time charting, but had pretty decent documentation on how to get it started. 
Even so - it took a while to get it working. Here are some more fun and silly charts that got closer to a solution.


<!-- clear: both; tells the browser that this element should be under any floated content
     without it, the videos would try to overlap with the TOC -->
<div style="clear: both;">
  <div class="video-row">
    <div class="video-item">
      <video controls>
        <source src="/github-portfolio/images/flutter_charting_blog/Initial_SF_chart.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="video-caption">Initial attempt at using Syncfusion. Somehow better and worse than FL charts</p>
    </div>

<div class="video-item">
      <video controls>
        <source src="/github-portfolio/images/flutter_charting_blog/Second_SF_chart.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="video-caption">Second attempt. Much closer but Syncfusion is distorting the morphology of the waveform due to how it scales the X axis. <p>
    </div>
  </div>
</div>



## The final result
<div style="clear: both;">
  <div class="video-row">
    <div class="video-item">
      <video controls>
        <source src="/github-portfolio/images/flutter_charting_blog/Initial_SF_chart.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="video-caption">Initial attempt at using Syncfusion. Somehow better and worse than FL charts</p>
    </div>
    </div>

This is a much more stable solution. While there is some distortion at the beginning of the charting - the rest of the time charting  is consistent. I suspect the initial bad charting is due to how I scale the x-axis when the charting starts. However, that will be for another time.

There are no delays in receiving or charting the data. This is ideal.

## Next steps
There's so much I want to do. But the next few steps are simple
- Turn my POC (proof of concept) ble scanner/ecg charter into its own app. I will keep the POC scanner as a learning reference for people who want to learn.
- Add authentication + authorization to log in and view your own data
- Save data to a server (most likely cloud)
- Encrypt data in transit from peripheral (ESP32) to client (Flutter), client (Flutter) to cloud (undecided)


