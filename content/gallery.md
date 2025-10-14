---
title: "Image Gallery"
date: 2022-06-25T18:35:46+05:30
draft: false
description: "Various pictures and videos for my projects. Tap on images/gifs to open and view in more detail and with better ratios. </br> At the bottom of my page is a demo of my BLE ECG project 🌎"
layout: "gallery"
galleryImages:
 - src: ../images/Intro_RealTime.gif
   caption: "Updated the diagram to better represent the project's current architecture"

 - src: ../images/ble_app_demo_blog/ESP32_To_Flutter.png
   caption: "Updated the diagram to better represent the project's current architecture"


 - src: ../images/recent_updates_blog/recent_update.gif
   caption: "After one month of dedication I fixed the desync issue, added cloud integrations, & made it more visually appealing"

 - src: ../images/flutter_charting_blog/Final_chart.mp4
   caption: "Best attempt at using Syncfusion charts at the time. Beginning was prone to desync"

 - src: ../images/flutter_charting_blog/Bad_FL_chart.jpg
   caption: "Initially attempted to use the FL Chart library. It did not work well"

 - src: ../images/flutter_charting_blog/Project_Architecture.png
   caption: "Initial design of the project's architecture"

 - src: ../images/esp_dsp_blog/Kalman_Final.png
   caption: "Reduced noise by applying FIR and Kalman DSP methods"

 - src: ../images/esp_dsp_blog/No_Filter_ECG_Moving.png
   caption: "After moving to Teleplot with an ESP32, I noticed a lot of noise while moving around"

 - src: ../images/esp_ecg_blog/My_annotated_ECG.png
   caption: "Annotating one of my ECG with its segments"

 - src: ../images/esp_ecg_blog/ECG_SerialPlotter_Recording.mp4
   caption: "First captured version of my ECG"

 - src: ../gallery_images/ESP_vs_Arduino.png
   caption: "Comparing and contrasting my thought process on selecting an ESP32 vs an Arduino"

 - src: ../images/first_time_soldering_blog/My_Solder_Job.jpg
   caption: "Hookup of my Soldered SparkFun AD8232 to my ESP32"
 - src: ../images/first_time_soldering_blog/Alt_Solder_angle.jpg
   caption: "Side view of my first solder job adding male pin headers"

 - src: ../images/first_time_soldering_blog/Me_With_My_Solder.jpg
   caption: "Picture of me after I successfuly soldered male pin headers on a SparkFun AD8232"

 - src: ../gallery_images/Player_Movement_State_Machine_Flowchart-1.png
   caption: "Flowchart of my Finite State machine for player movement"
 - src: ../gallery_images/Nano.png
   caption: "My Nano <3"
   
viewer : true
viewerOptions : {
    title: true
    # you can add more options here. refer https://github.com/fengyuanchen/viewerjs?tab=readme-ov-file#options
}
---
<div style="text-align: center">
    <span class="jonquil-yellow-text"> 
        Here is a video of me explaining my project and providing a demo. It is slightly long 
    </span>
</div>

<div class="gallery-video">
  {{< youtube ULPSuycPSoY >}}
</div>

