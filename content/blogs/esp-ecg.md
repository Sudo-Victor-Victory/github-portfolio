+++
date = '2025-07-02T19:06:09-07:00'
draft = false
title = 'Created an ECG with an ESP32.'
description = 'Talking about my process to make the ESP32 ECG and upcoming plans'
author = 'Victor Rodriguez'
image = '/github-portfolio/images/Patient.webp'
+++

## <span style="color:#4682B4">You created an ECG???? What is that?</span>


You may not know what an ECG but you have guaranteed seen one.


It's almost a trope at this point to have someone in a movie be at their deathbed in a hospital, surrounded by loved ones - only to hear a long characteristic ***BEEEEEEEEEEEEEEEEEPPPPPPPPPPPPPPP*** and doctors rushing over to resuscitate them.

[comment]: # (More separates the main content from the Hugo summary)

<!--more-->

<div style="text-align: center; max-width: 75%; margin: 0 auto;">
  <a href="/github-portfolio/images/ECG_example.jpg">
    <img src="/github-portfolio/images/ECG_example.jpg" alt="My solder job" 
         style="display: block; max-width: 100%; height: auto; border-radius: 8px; margin: 0 auto;">
    <p style="font-size: 14px;">An ECG.</p>
  </a>
</div>

The cause of the BEEP and the doctors rushing in is due to the ECG machine monitoring the patient. 
</br> With all that being said, what even is an ECG?

| ECG Purpose 💖     | How it works ⚙️  | Explanation 🧠| 
| ----------- | ---------------- | -----------|  
| An ECG is used to gain information about a person's heart.      | The ECG detects electrical impulses from the heart. | Electrodes (see below) are attached to the patient, and they send the patient's electrical signals to an ECG machine. The machine then displays & records the electrical activity of the patient's heart, for the doctors to analyze and diagnose the patient. |

<div style="display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
  <div style="text-align: center; flex: 1 1 300px; max-width: 40%;">
    <a href="/github-portfolio/images/MAC-5.jpg">
      <img src="/github-portfolio/images/MAC-5.jpg" alt="My solder job" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">A GE MAC-5 ECG machine</p>
    </a>
  </div>

  <div style="text-align: center; flex: 1 1 300px; max-width: 33%;">
    <a href="/github-portfolio/images/Electrode.jpg">
      <img src="/github-portfolio/images/Electrode.jpg" alt="Alt solder angle" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">A 3M electrode </p>
    </a>
  </div>
</div>

## <span style="color: #ff7043 ">That's pretty cool. Why'd you do it?</span> 

It stemmed from noticing family members lacking an ECG, being interested within the biomedical engineering realm ever since I used a Event monitor (Holter) myself, and having a genuine interest about learning the heart's biomechanics after learning about it from a fellow COE EXCEL Peer Mentor in undergrad.

## <span style="color: #f33e62 ">How'd  you do it?</span> 

I'm going to save some information for an upcoming blog post relating to my overall journey on embedded programming, but this is how it went down.

1. Picking the micro-controller. ESP32 vs Arduino Uno R3
    | ESP32 Pros     | ESP32 Cons | Arduino Uno R3 Pros |   Arduino Uno R3 Cons| 
    | ----------- | ---------------- | -----------|  -----------|
    | Has bluetooth connectivity     | Personally newer to it compared to Arduino | Have had more experience with it than the ESP32. |  No Bluetooth connectivity| 
    | Roughly 15x faster than Arduino | Not as much documentation compared to Arduino | Has more documentation and chip support for the platform | Dramatically slower than the ESP32 | 
    | Has 2 cores for parallelism |  | | Physically much larger than the ESP32. |  
2. Selecting important criteria the ECG Module must have.
    - This process was much easier as I had a micro-controller in mind and needed to find something that fit this criteria:
        - Power draw - ensuring that the board was powered either 3.3V or 5V, and ideally has a continuous low power draw for BLE.
        - Compatibility - that the board had a library or support for the ESP32.
        - Price - By utilizing a more affordable IC, it allows the end result to be more economically viable and accessible.
        - Portability - The ESP32 is pretty compact, and having an board that is also compact would greatly increase the portability of the end result.
3. Then it came down to selecting a module.
      |  SparkFun AD8232 Pros   | SparkFun AD8232 Cons | MAX30003 Single-lead ECG Pros | MAX30003 Single-lead ECG Cons | 
      | ----------- | ---------------- | -----------|  -----------|
      | Documentation & articles allow for the device to be more easily understood  | Output is analog, filtered by a HI-LO pass in an RC network. Tl;dr resistors and capacitors remove weird-looking values due to a cutoff values. Will require DSP implementation for higher accuracy.  | MAX30003 already does DSP and it can be configured (has an ADC+DSP logic) Resulting in higher accuracy. |  The official MAX30003 library in C is not beginner friendly, and various community-lead wrappers often get abandonded, privated, or have poor start guides. | 
      | Aside from soldering, connecting the AD8232 to the ESP32 is simple. Essentially plug and play ***module***. | AD8232 does not specify what electrode is not making proper contact with the person, only that an electrode in the system isn't make proper contact. (Non-descriptive lead detection) | Outputs more bio information like R-R, heart rate out of the box. Has more descriptive lead detection.  | MAX30003 is the raw chip and ***not a module***.  Finding a pre-built module / breakout board for the MAX30003 is difficult due to low stock and limited vendors, extremely higher purchase and shipping prices. | 
      | The output is analog, meaning reading is as simple as using `analogRead();` | The AD8232 limits the end result's  reach due to the AD8232 not being a medical-grade chip| The MAX30003  **IS** clinical grade increasing its reach dramatically | If no module can be attained, I'd have to make my own PCB and attach the MAX30003 through a  Hot Air Rework Station just to connect it to the ESP32| 
4. I chose the SparkFun AD8232 for its simplicity in connecting to the ESP32, being an excellent entrypoint into DSP, being attainable & affordable, only requiring soldering, and still providing some ECG data to be analyzed and sent through BLE into a mobile app. In the future I would *LOVE* to attempt building a module out of the MAX30003 and making a custom PCB for it.
5. Unexpected setback - soldering.
    - Having limited experince in circuit building I had not noticed at the time that the chip I selected did not have any pins attached. I described my process of soldering some male pin headers onto the chip in a [previous blog post](/github-portfolio/blogs/my-first-time-soldering/).
6. **Hookup!**
<div style="display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
    <div style="text-align: center; max-width: 90%;">
      <a href="https://github.com/Sudo-Victor-Victory/ESP32-ECG/blob/main/ESP_ECG.pdf">
      <img src="/github-portfolio/images/SparkFun_AD8232_ESP32_Circuit_Design.png" alt="Component diagram showing what GPIO pins connect to what SparkFun AD8232 pins" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">My Circuit design for the ESP32 + SparkFun AD8232. Tap on the img to go to my github for the full pdf + KiCad project files.</p>
      </a>
    </div>
</div>

</br>
<div style="display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
  <div style="text-align: center; flex: 1 1 300px; max-width: 60%;">
      <a href="/github-portfolio/images/My_Solder_Job.jpg">
      <img src="/github-portfolio/images/My_Solder_Job.jpg" alt="Showing the physical connection between the ESP32 and SparkFun AD8232 directly" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">Asserting that the SparkFun AD8232 can be hooked up to the ESP32 directly.</p>
      </a>
    </div>
    <div style="text-align: center; flex: 1 1 300px; max-width: 46%;">
      <a href="/github-portfolio/images/Sparkfun_Electrode_hookups.png">
      <img src="/github-portfolio/images/Sparkfun_Electrode_hookups.png" alt="Recommended electrode placement from the SparkFun website." style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">Recommended electrode placements on the body. In my opinion the right's electrode placements produce more accurate results. </p>
      </a>
    </div>
</div>

## <span style="color: #003bf7 ">Results</span> 

<div style="text-align: center;">
  <video style="max-width: 100%; " controls>
    <source src="/github-portfolio/images/ECG_SerialPlotter_Recording.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-size: 14px;">My waveform captured by my ESP32's ECG.</p>
</div>

Overall, the results look *alright*. </br> Here's an annotated ECG taken from 33 seconds into the recording. 
<div style="display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
    <div style="text-align: center; flex: 1 1 300px; max-width: 60%;">
      <a href="/github-portfolio/images/My_annotated_ECG.png">
      <img src="/github-portfolio/images/My_annotated_ECG.png" alt="My solder job" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">My Annotated ECG with each segment. </p>
      </a>
    </div>
        <div style="text-align: center; flex: 1 1 300px; max-width: 40%;">
      <a href="https://learn.sparkfun.com/tutorials/ad8232-heart-rate-monitor-hookup-guide/all#understanding-the-ecg">
      <img src="/github-portfolio/images/SparFun_Example_ECG.png" alt="My solder job" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">SparkFun's example ECG with annotations. Click the img to be routed there.</p>
      </a>
    </div>
</div>

## <span style="color: #003bf7 ">Analysis and remediation</span> 

My annotated ECG does indeed resemble the example ECG. However, there's a few critiques that need to be pointed out in this annotation and the overall ECG.
- PR segment is rather small; U wave is *veerrryyyy* small. This tells me the recordings are lower in resolution than in clinical use boards.
- I had to stay very still to get accurate recordings.
  - Noise caused by movement are called 'motion artifacts'. Essentially movement causes the electrodes to display innacurate signals.
  - Leaving motion artifacts in allows for misinterpretation of a patient’s condition — potentially leading to severe consequences if not addressed.
  - This indicates that extra filtering and processing is required to help remediate artifact detection.
-  If the sampling rate is too low or inconsistent, it can distort waveform shape. I would like to test more sampling rates to find the one that produces the best results.
- The QRS complex looks fantastic here. Although due to noise sometimes the two voltage drops within it are absent or not as pronounced.
- T wave looks beautiful.
- PR Segment appears short. It could stem from sampling rate issues or a fast heart rate. I believe my heart rate was around 70 BPM at time of recording, so this further implies that the sampling rate should be investigated. 
-  The serial plotter in Arduino's IDE constantly changes its Y-axis scale to keep all the data in frame. This makes it difficult to compare waves as the left side of the monitor's graph becomes distorted to accomodate the peak of the next QRS complex.

## <span style="color: #f33e62 ">Reflection</span> 
Overall, I'm very pleased with these results. Two weeks ago I had no clue if I could even get any of this done. Since then I've research electrical components, learned to solder, and  created code to monitor & display an actual ECG waveform. There is work to be done in refining these voltages possibly through DSP to make them more accurate and reliable. That'll be an upcoming post I'm sure.

## Thank you
Thank you for reading. This project means a lot to me and has been insanely fun to work on. I hope that you continue to join me on my journey of making the world a healthier place :heart:

