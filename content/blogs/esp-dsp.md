+++
date = '2025-07-17T13:13:44-07:00'
draft = false
title = 'ESP DSP'
description = "Discussing my process for filtering noise for my ESP32's ECG"
author = 'Victor Rodriguez'
image = '/github-portfolio/images/esp_dsp_blog/static.jpg'
+++


## Noise sucks.


You 100% have encountered unwanted noise in your life. 
- Analog / Digital TV not having a clear image or static? **Noise**.
- Poor WiFi connection even though the router is *right* there? **Noise**.
- Microphone on your computer has static? **Noise**.
- Cars honking in Manhattan even though you're like 16 floors up? **Noise**. 
- ECGs, too, are susceptible to **Noise**.
## Noisy ECGs are bad, right?
100%. A noisy ECG could result in a misdiagnosis, mistreatment, or even results and time being lost. At every step we want to reduce noise for clearer, safer, and more reliable results.

## You're building an ECG. Did *you* have noise???
Yup. And I still do. Noise will never be 100% removed. Let alone noise like motion artifacts (electrical signals being picked up/ misinterpreted as heart signals due to muscle contraction). However, I **did** reduce noise in my ECG with:

## DSP (Digital Signal Processing)
DSP are mathematical techniques used to transform, analyze, or information from digital representations of analog signals. There's various aspects to these techniques, such as what domains they reside in. Frequency domains represent the number of occurances of each frequency, as opposed to a time domain which shows how the frequency changes over time.

{{< zooming src="/github-portfolio/images/esp_dsp_blog/ft_v_t.gif" alt="An image showing a frequency domain compared to a time domain" caption="Visual representation of  a frequency domain compared to a time domain" >}}

Notice how Frequency domain only has 3 occurances of an amplitude, compared to time domain shows all signals changing over time.
Okay enough nerding out. </br>
There are a few powerful and common functions in DSP, such as:
- Matrix multiplication: reference
- Dot product: reference, example
- FFT: reference, example
- IIR: reference, example
- FIR: reference
- Vector math operations: reference
- Kalman filter: reference </br>
All of which are found in the [Offical ESP32 DSP library (ESP-DSP)](https://github.com/espressif/esp-dsp)

## What DSP methods did you use?
I primiarly used *FIR* and *Kalman*. For FIR, I utilized ESP-DSP's implementation. For Kalman, I implemented my own. Both will have their own sections.

## FIR Filter (Low Pass)
FIR is simple, powerful, and reliable filter. And I love it. Without going too into the weeds, FIR is essentially a weighted sum. You define a number of taps (or coefficients) which are specially formulated to alter a signal in a predictable fashion (due to a cutoff, and transition bandwidth which defines how much to attenuated/reduce noise) and are summed up to give you a more-concise signal. In human terms, you have a special set of numbers that mathematically change your signal in a predictable manner. In my case, I decided to implement a low-pass filter (ignore high frequencies) for my FIR filter. Here's the reasoning.
- Reduces high-requency noise: 
    - Upon researching what frequencies each wave resided in, the highest peak in frequency was the QRS Complex at roughly 40 Hz. Meaning I could ignore higher frequencies, as they stemmed from muscle noise or motion artifacts.
    - Low-pass could cheaply filter these out
- Preserve waveform morphology (preverse the 'look' of the ECG)
    - by filtering out higher frequencies, I can still preverse the look of the ECG.
    - Similarly, you could have a transition bandwidth to modify how much signals are attenuated in proximity to a cutoff. This *also* was huge in removing unwanted jittery noise.

So ultimately it would eliminate higher frequency noise, attenuate lower frequency noise due to the transition bandwidth. I used the website FIIIR.com to generate my FIR coefficients. It will be discussed more in Methodology. 

{{<  zooming src="/github-portfolio/images/esp_dsp_blog/FIR_Website.png" alt= "A screenshot of FIIIR.com" caption="A screenshot of FIIIR.com with 250 Hz sampling rate, 25 Hz cutoff freq, 20 Hz transition bandwidth, window type Hamming" >}}

## Kalman Filtering
Kalman is also insanely cool, and surprisingly simple. Kalman filtering is a system that makes a prediction of what a signal should look like, then compares with the actual measurement (aka signal). Then, based on two values you can fine-tune, the system will either use the real measurement or rely/modify the predicted measurement.
In more detail - the two values are
| Value      | Explanation 🧠|  Intuition |
 ---------------- | -----------|  -----------| 
| Q | Represents how volatile we believe our signal is.| If our signal fluctuates quickly, and that is expected, a high Q is desired. If our signal is steady and smooth, low Q.|  
| R | Represents how noisy we believe our system is. | A higher R means that we believe our system will intristically have a lot of noise. A lower R means we trust our system to be less noisy.|

These values direclty modify K - the ***Kalman*** Gain. A higher K means we rely more on the measurement, a lower K means we rely more on the prediction. The equation itself is (Ignore P, its the previous error estimate look up a vid on it)
- **K** = (P + Q ) / (P + Q + R )
So we can reason that a higher Q means we bias towards the measurement more, high R biases more towards the prediction. Mixing them gets you something in between. I discuss how I chose my Q and R in Methodology.

I decided to implement my own simple Kalman filter instead of using ESP-DSP's implementation. The ESP-DSP implemention is a Extended Kalman Filter (multi-variable state estimation), which is overkill for a linear problem like ECG filtering. That and it didn't look too complex to both understand and implement in C++. 
{{< zooming src="/github-portfolio/images/esp_dsp_blog/ESP_Kalman_Vs_Custom.webp" alt="A dramatic comparison between ESP-DSP's implementation of Kalman vs my own" caption="ESP-DSP's Kalman vs my requirements">}}

## Pre-DSP Results 
<div class="gallery">
{{< zooming src="/github-portfolio/images/esp_dsp_blog/No_Filter_Ecg.png" alt="A screenshot of my ECG - with all parts of the waveform being jittery" caption="Staying still.">}}
{{< zooming src="/github-portfolio/images/esp_dsp_blog/No_Filter_ECG_Moving.png" alt="An even more jittery screenshot with more egregious motion artifacts" caption="Moving my right arm.">}}
</div>

As you can observe, there is a lot of jagged-ness to the ECG waveform. These were captured in a rested state, with no filtering. Staying still looks relatively normal, however when you zoom in or use a smaller time-frame the jagged nature is far more apparent. The image on the right, with movement, looks the worst. With the T wave after :09 looking almost like a complex itself. I needed to filter.

## Testing Methodology
I feel my methodology was simple. I would wave my right arm around in a circular motion towards the ceiling for approx. 6 seconds, and take a screenshot. I chose my right arm to be moving as the black wire/electrode is placed on my right pectoral, which would generate electrical signals for the movement. It would also allow my left arm to press the screenshot button.

## FIR Tuning Methodology
Finding good coefficients - which relied on sampling rate Hz, cutoff frequency Hz, a transition bandwidth Hz, and a window type, was hard. </br>
Luckily a lot of information relating to it was google-able. 
- 250 Hz is a common sampling rate in Holter monitor ECGs, fitness devices, and *some* clinical ECGs. 
- Cutoff frequency is 40 Hz due to the highest frequency, the QRS complex, peaking at around 40 Hz.
- The transition bandwidth + cutoff was meant to be approx 40 Hz. So I had a bit of trial and error. 
- Window type is hamming to reduce rippling effects, decent performance cost, and a moderate sharpness to further reduce noise. 

From there it was a matter of trial and error - visually inspecting the results of the filter while staying still and moving. The selected one will not be here, but in #Results.</br>
They will all follow the pattern of staying still | moving. 
### 23 Hz Cutoff , 16 Hz Transition 
<div class="gallery">
{{< zooming src="/github-portfolio/images/esp_dsp_blog/23_16_non.png" alt="A screenshot of my ECG - with all parts of the waveform being jittery" caption="Staying still">}}
{{< zooming src="/github-portfolio/images/esp_dsp_blog/23_16_moving.png" alt="A screenshot of my ECG - with all parts of the waveform being jittery" caption="Moving arm around.">}}
</div>

### 24 Hz Cutoff, 20 Hz Transition 
<div class="gallery">
{{< zooming src="/github-portfolio/images/esp_dsp_blog/24_20_non.png" alt="A screenshot of my ECG - with all parts of the waveform being jittery" caption="Staying still">}}
{{< zooming src="/github-portfolio/images/esp_dsp_blog/24_20_moving.png" alt="A screenshot of my ECG - with all parts of the waveform being jittery" caption="Moving arm around.">}}
</div>


## Kalman Tuning Methodology
Kalman was far easier to test, due to only modifying 2 variables instead of modifying an entire list and associated variables. The best part was that I am doing Kalman **after** FIR, so it would make detecting any changes to the signal considerably easier as it'd just amplify the changes. From here it was a matter of trial and error, with the intuition of understanding the Kalman Gain (K)'s relationship with Q and R. 

### 23 Hz Cutoff , 16 Hz Transition 
{{< zooming src="/github-portfolio/images/esp_dsp_blog/kal_10_20.png" alt="Kalman filter with Q 10, R 20, with little difference" caption="Left is FIR, Right is FIR+Kalman. Very little change. ">}}

### 23 Hz Cutoff , 16 Hz Transition 
{{< zooming src="/github-portfolio/images/esp_dsp_blog/kal_20_10.png" alt="Kalman filter with Q 10, R 20, with little difference aside from some peaks being too smooth" caption="Left is FIR, Right is FIR+Kalman. For the most part similar, aside from smoothing of some edges.">}}

## Results 
Finally, lets look at this nice graph.
{{< zooming src="/github-portfolio/images/esp_dsp_blog/Final_Results.png" alt="A combination of FIR and Kalman, with the results being smoooooothhhh." caption="Left is FIR, Right is FIR+Kalman. Increadibly SMOOTH despite moving. ">}}

## What'd I do better

## Summary 

## Next Steps