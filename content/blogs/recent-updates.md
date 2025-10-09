+++
date = '2025-09-26T15:40:12-07:00'
title = 'BPM, Supabase, and Breakouts'
draft = false
description = "My hodge-podge of recent updates - including adding BPM, Supabase integration, and using more breakout boards"
author = 'Victor Rodriguez'
image = '/github-portfolio/images/recent_updates_blog/recent_update.gif'
+++

## <span class="cerulean-blue-text "> Work hard in silence, let your success be your noise </span>
I have been very busy upgrading my project. As a recap my project previously
- An ESP32 that uses a SparkFun AD8232 that sends data over BLE.
- Utilizes FIR and Kalman DSP filtering techniques to reduce motion artifacts and increase SNR (Signal to noise ratio)
- Gets sent to a mobile app in real time and charted with Syncfusion's SfCartesianCharts
but I've been very lockedIn ™️ on this project over the past month. Now

## <span class="cerulean-blue-text "> My project has been </span > <span style="color:#34B27B"> SUPA-</span><span class="cerulean-blue-text ">charged </span >
That's RIGHT!! Over the past month I've
- Added Supabase, a cloud database provider—specifically PostgreSQL, to my project
    - Added Auth (sign + log in), table permissions to my supabase database(s)
    - Created three database tables: profiles (user data), ecg_sessions, and ecg_data
- Added the ability to view previous bluetooth sessions. They get saved to Supabase
- Added various animations to the project
- Added the ability to view and connect to previously connected to bluetooth devices
- Added settings page with the ability to change text size
- Added tests to the project
- BPM is now received by the flutter client. So you can also monitor your BPM in real time.
- BPM is now chartable too.

and that's not even all of the work I've done. Can you believe it? Now for the ESP32 changes
- Added a TPS61023 buck booster to the project
- Added a TP4056 usb charger + shield
- Added a 10uF ceramic capacitor 
- The ESP32 is now completely battery powered by a LiPo battery!! It's freeeeeeee
- BPM is now calculated within the ESP32's TaskECG. 
    - It uses R-R detection with a R threshold and refactory period to ensure we accurately detect the R peaks.
- The number of bytes sent over per BLE notification is now 28 bytes as opposed to the original 24 (2 for BPM 2 for padding)


## <span class="jonquil-yellow-text "> BPM Calculation </span>
BPM Calculation was pretty straightforward. The current most popular method is R-R detection. R-R detection involves timing the duration from when you initially see the 1st R (The Peak of a Q**R**S complex) and see another R. From there we can make some decisions such as: has it been a reasonable amount of time since we last saw an R? , is this a reasonable R value? These are called R-Thresholds. To summarize: to calculate BPM we detect an R, wait until we detect another R using our R-threshold, then we get the difference between the two and divide by 60 seconds to get the BPM.


{{< zooming src="/github-portfolio/images/recent_updates_blog/image.png" alt="Screenshot of an ECG graph. With arrows pointing to the highest peaks of 2 of the QRS complexes within it." caption="Visually it is easy to follow. Take the time in which you have seen 2 peaks and there you will find the BPM." >}}


### <span class="jonquil-yellow-text "> R-R logic </span>

So since I already have a batch of data that covers roughly 40 ms of  data, the likelihood of an R being in there is pretty valid. So here's the logic for R-R  detection
- Analyze a batch (10 samples, every 4ms, so 40ms of ecg data)
- Find the max value within the batch (our presumed R)
- Calculate the timestamp of that max value 
- Then, we check if it’s a valid R using our R-threshold
    - Check if the max value was greater than a preset R value. If it's bigger we can safely say it's an R.
    - Check if the time of this R is reasonable value between 300 and 2000 ms, anything else is likely noise
- If all is good, the R we have is valid, and we can calculate BPM, which is our interval 60,000 / (Time of current R  -  Time of previously detected R)
- Add it to our estimated moving average to have a more consistent BPM since the SparkFun AD8232's BPM is sensitive
- And BAM we got it 


## <span class="red-text "> Storing data within Supabase </span>
This was another one of those "as I get more work done the goalpost keeps moving" kind of things.

Initially I just wanted this project to be simple: send ECG data over BLE (and maybe BPM), view it in real time in an app, and call it a day.

However, I thought it’d be cool to replay a session.. Inspiration actually came from a Garmin watch (Instinct) where you could view your past BPM data. I thought that was awesome and wanted to give it a try.

##  <span class="blue-green-text">  Out of all Clouds, why Supabase? </span>
My major 4 criteria for choosing a cloud provider were:
- <span style="color:#004BA8"> Integration </span>  - would it be easy, medium, or hard to integrate into my project?
- <span style="color:#F8C630"> Price </span>  - were there free tiers, and how long would it take for me to exceed it?
- <span style="color:#F18F01"> Bonuses   </span>  - were there any free features I could get alongside it?
- <span style="color:#EA9E8D"> Database type  </span> - what would the database within the cloud provider look like? 

| <span style="color:#011936"> Cloud Provider </span> | <span style="color:#004BA8"> Integration </span>  | <span style="color:#F8C630"> Price </span>    | <span style="color:#F18F01"> Bonuses   </span>  | <span style="color:#EA9E8D"> Database type  </span>   |
| ---- | ----- | ---- |---- | --- | 
| <span style="color:#FFC400"> Firebase (Real time) </span>| **Medium**. Flutter \& Firebase are owned by google. Documentation lacks full clarity.  | Free tier with pay-as-you-go scaling. Hard to exceed limits early on unless you’re doing heavy reads/writes.  |Authentication, end-to-end encryption, login providers (Google, Apple, etc.), analytics, push notifications.| NoSQL, real-time sync, serverless.| 
| <span style="color:#34B27B"> Supabase  </span>| **Easy**. Not owned by google but has clear and concise docs to get started | Pretty Generous free tier — 500 MB database, 1 GB file storage, and good usage limits before paid tiers. | Auth, row-level security, edge functions, Postgres APIs, real-time channels, dashboard UI.| PostgreSQL, relational (RDBMS), ACID compliant.|
| <span style="color:#00ED64"> Mongodb </span> | **Hard**. No official MongoDB library for flutter or dart. Requires setup and configuration of clusters and Flutter integration via SDKs or REST APIs. | Free tier includes 512 MB storage and shared cluster. Scales fast but pricing rises with usage. | Built-in sharding, Atlas search, charts, global clusters.  | NoSQL, document-based, JSON-like structure.| 

### Cloud provider analysis
As you can see they were all pretty similar in terms of price - free tiers are a wonderful thing 🥹

That bumped the criteria down to 3 things: integration, bonuses, database type.

### <span style="color:#FFC400"> Firebase  </span>
I had previously utilized Firebase's Real time database in my senior design class inside of a native Android app. I personally didn't find the documentation to be as clear as it could have been and had difficulty getting it to work. Integration *could* be difficult again. 

The bonuses really revolved around auth, analytics, and push notifs. These were nice but not really dealbreakers for my project. I had also utilized a NoSQL database within Mongo Atlas before, so it wouldn't take long to adjust to it. 

Personally I was **not** too excited to work with Firebase.

### <span style="color:#34B27B"> Supabase  </span>
Never used it before, always saw small projects that utilized this scaled well in the wild (GitHub). Cracking open the documentation it was pretty clear and concise - a breath of fresh air. Integration would be smooth.  

The bonuses were features that were appealing to me - specifically the row layer security (RLS) and edge functions for potential AI integration. And who doesn't love a nice UI?

I have a ton of XP working with RDBMS before so that'd be cool to do again. 

Was pretty interested in using Supabase. 

### <span style="color:#00ED64"> Mongo Atlas </span>
Utilized this in Sophomore year of University to build a recipe website (it is defunct) that was optimized for data handling in the app layer via REST APIs. This was done in the MERN stack. So I was familiar with Mongo Atlas fairly well. There being no official Dart or Flutter library was pretty much a deal breaker for me as integration would be difficult. 

MongoDB's bonus features were more appealing to web developers since it can be extremely useful to have a document based JSON structure to build your database within NoSQL - and not Flutter.

Aside from my prior XP of using MongoDB, there were not many appealing features.


### <span class="ecg-gold-highlight">  Final decision </span>
Supabase handily won out. Alongside wanting to try out a new provider, it's RLS, realtime capability, clear documentation and examples, made it the clear winner. 


## Hardware side changes
Want to keep this one brief since there will be a diagram in a following blog post about it.
### Circuit changes ⚡
I really wanted this thing to be battery powered and I got it to be. It involved soldering 2 components with male pin headers - TP4056, TPS61023, and even a micro-JST female connection.

### Firmware changes 🛠️
I was able to discover that I could include my BPM calculation (including R-R detection) within my normal `Task_ECG` RTOS task, since it's impact on the scheduling between both tasks was negligible. 

## <span class="ecg-gold-highlight">  What's next </span> 
Honestly in terms of this project I feel like it's pretty close to done. I want to showcase the functionality and aesthetics in my next blog post.
Til then ❤️