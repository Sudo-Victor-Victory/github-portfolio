+++
date = '2025-10-10T15:39:47-07:00'
draft = false
title = 'Ble App Demo'
description = "A video discussing my project in it's entirety"
author = 'Victor Rodriguez'
image = '/github-portfolio/images/ble_app_demo_blog/demo.gif'
+++

## <span class="jonquil-yellow-text"> Lessons learned throughout development </span>
This has been roughly 3 months in the making.
It was an insane amount of fun. This project gave me a better understanding of 
- Bluetooth Low Energy (BLE)
    - GATT architecture and characteristic design
    - Advertising and connection workflow
    - Character descriptions for notifications
- Electrocardiograms (ECGs)
    - Purpose and signal interpretation fundamentals
    - Electrical conduction pathway of the heart
    - Segments of an ECG waveform
    - Digital signal processing (DSP) techniques such as
        - FIR
        - Kalman
- FreeRTOS
    - Task scheduling and priority management
    - Multi-processor configuration on a dual-core ESP32
    - `delay()` vs `vTaskDelay()` and timing accuracy
    - Preemption behavior and deterministic task execution
- Cloud Databases (Supabase)
    - Designing relational schemas for sensor data 
    - Implementing Row-Level Security (RLS) for user isolation
    - Using the Supabase REST API for CRUD operations
- Flutter
    - Built a complete mobile application from start to finish
    - Utilized `Syncfusion`’s charting library for real-time ECG visualization
    - Produced motion effects to enhance user experience

All of those topics are mentioned in my previous blog posts. Please check them out for more information :sparkling_heart:
Here are the repos to both parts of this project.


## <span class="ecg-gold-highlight"> It is time. Time to Demo </span>
I created a video fully showcasing my application. Please enjoy.

<div style="text-align:center;">

{{< youtube ULPSuycPSoY >}}

</div>

## <span class="blue-green-text"> Repositories for each system </span>
<a class="github-card" href="https://github.com/Sudo-Victor-Victory/ESP32-ECG" target="_blank">
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=Sudo-Victor-Victory&repo=ESP32-ECG&theme=default" alt="Repo card preview">
</a>

<a class="github-card" href="https://github.com/Sudo-Victor-Victory/BLE_Demo" target="_blank">
  <img src="https://github-readme-stats.vercel.app/api/pin/?username=Sudo-Victor-Victory&repo=BLE_Demo&theme=default" alt="Repo card preview">
</a>

## What's next
I will most likely keep working on these repositories since there are little enhancements I can perform. No matter what state something is in -  <span class="ecg-gold-highlight"> It can always improve </span>. Thank you for joining me on this journey.
