+++
date = '2025-06-29T17:16:24-07:00'
draft = false
title = 'Successfully soldered an electrical component!'
description = 'I had never soldered - before today. Lets talk about it.'
author = 'Victor Rodriguez'
+++

[comment]: # (Steel blue may be a system-wide color for the future)
# <span style="color:#4682B4">Why are you discussing soldering?</span>
I recently had to solder. I want to have a discussion of why, what I used, how I did it, provide useful links, and showcase my work.
## <span style="color: #ff7043 ">Why did you learn to solder?</span> 
I have never soldered before. For an upcoming project (see the end of the blog for more info) - I forced myself to learn how to successfully do it. Here’s why I learned it — and why you should too:
<html>
  <head>
    <style>
    .red-text {
    color:   	#FF3131;    
    font-size: 18px; 
    margin: 0;      
    }
    .yellow-highlight {
      background-color: #FFDB58; /* MUSTAARRDDD Yellow */
      color: black;
      font-weight: bold;
      padding: 2px 4px;
      border-radius: 4px;
    }
        .yellow-highlight {
      background-color: #FFDB58; /* MUSTAARRDDD Yellow */
      color: black;
      font-weight: bold;
      padding: 2px 4px;
      border-radius: 4px;
    }
     </style>
  </head>
</html>


- <span class="yellow-highlight">**Freedom**</span> - You will be freely able to incorporate electrical components in future projects without relying on the OEM attaching electircal connectors. Sometimes, you don't have a choice and the OEM doesn't include *any* electircal connectors - in which case soldering is often the best (though not the only) option.
- <span class="yellow-highlight">**Repairs**</span>  - If a wire or an electrical component breaks, having the ability to fix it asap and not re-buy expensive components, or wait until a component arrives, makes you much more agile and self-sufficient.
- <span class="yellow-highlight">**It's fun!**</span>   - Completing a successful solder makes you feel awesome!

## What even is Soldering?

Soldering is  <span class="red-text"> **Really** </span>   cool. It lets you combine 2 pieces of metal together. 

Soldering is similar to welding - both result in 2 being pieces of something joined together. The key difference though is that in welding the pieces of metal are combined directly to one another, whereas soldering uses another metal (***The solder!!!!***) as the combining agent.


## What do I need to solder?
I'm going to provide some non-affiliated amazon links for all the things you need to solder. These are definitely lower-end products that save on $$ but still allow you to perform good solders. I will also explain the necessity of each purchase.


| Item      | Link | Explanation| 
| ----------- | ----------- | -----------|  
| <span class="red-text">Soldering iron. </span>       | [Click me for soldering kit](https://www.amazon.com/Soldering-Kit-Temperature-Desoldering-Electronics/dp/B07GTGGLXN?ref_=ast_sto_dp&th=1)| Goes without saying you can't solder without the soldering iron. |
| <span class="red-text">Soldering wire </span>      | (included in kit) | Solder wire is the bonding agent. Required. | 
|<span class="red-text">A wet sponge  </span>      | (included in kit) | Used to remove excess solder, flux, or oxidation from the soldering tip. A dirty tip is an inefficient one. You can use various other things like a wet rag, but sponge is what most people use.|
| <span class="red-text">Solder flux </span>      |  [Click me for the flux](https://www.amazon.com/Solder-Soldering-Rosin-Lead-Free-Electronics/dp/B08MVXW4RY?ref_=ast_sto_dp&th=1)|  It's purpose is to burn and use nearby Oxygen. By taking the nearby O2, the Soldering wire and the component it gets attached to are able to bond better. |
| <span class="red-text">An electrical connector  </span>     | [Click me for the connectors I used](https://www.amazon.com/Jabinco-Breakable-Header-Connector-Arduino/dp/B0817JG3XN?ref_=ast_sto_dp) | Depending on your use case you may not need breakable pin headers. You could use various things like wires, LEDs, resistors, this part is variable.|
| <span class="red-text">Safety goggles </span>    | Any work. | Non-negotiable. Solder smoke is an irritant. Hot liquid solder entering your eye will hurt and possibly cause major damage.|
| <span class="red-text">Isopropyl alcohol </span>    | Any percent works. | Uses to remove excess flux off of equipment. |


## <span class="yellow-highlight">**S A F E T Y**</span>
Before we get into the main part of the article I just want to quickly explain some things.
- Solder and flux <span class="red-text"> BURN</span>. Please solder in a clean workspace to avoid a fire risk.
- Solder and flux <span class="red-text"> BURN</span>. So please solder in a well ventilated area to avoid lung irritation.
- The solder iron gets <span class="red-text"> BURNING HOT</span>. You can burn yourself. Refer to the images below 

<span class="yellow-highlight">***P L E A S E***</span> be careful when you solder.

## Alright I got the required materials. What are the steps to solder???

Easy.
1. Find a clean and well ventilated area and bring your materials.
2. Wet your sponge. Like really wet it.
3. Attach the electrical connector/component to the other piece you want to connect it to. Can be a resistor to a PCB, or pin headers to  through-holes on a PCB, etc etc. Make sure the connection is tight and that the connection maintains itself without you holding it.
    1. TIP!!! - A breadboard is a great way to keep pin-headers attached to a PCB
4. Apply some flux onto the connection area (ex: the area where the pinhole meets the pin headers), and some on your iron before it is turned on.
    5. I personally like to use another a detached solder iron tip to apply flux. Anything but the active solder iron should be used to apply flux.
6. Connect and turn on your solder iron. Hold the non-metal area.
7. Adjust the temperature. Between 315°C and 400°C is ideal. I like 350°C.
8. Once the solder is hot (Since you had some flux on before you turned it on, you can see it melt), place it on one of the connectors. The heat does the work so you shouldn't apply a ton of force.
9. Get your solder wire, and make it touch the connector. Since the connect is also being touched by the solder iron - the wire melts.
10. YOU JUST SOLDERED!!! Conngrrraaaaatttsssts.
11. Maybe wait a minute or two before touching since your PCB / combined component may be a little hot as a result.
12. Remove excess flux with isopropyl alcohol and either a brush or a paper towel. 

## It's not *That* easy, right?
Welllll, yeah. There are a few things to be aware of.
1. It <mark>will</mark> be an iterative process. 
2. Make sure the solder you applied resembles volcanoes. With a thicker base near the bottom and a thinner base the near the top.
    1. Don't worry if you applied too much solder - you can always apply more flux on the area with too much solder and you can hold your solder iron there to melt away the excess solder.
3. Avoid having 2 pins be connected by the solder. This is a really fast way to short-circuit your component. Refer to the previous list entry.
    4. If two pins are connected with solder, you can apply more flux in between both pins and 
5. Test the component
    1. Once the solder is on it can be used/tested immediately if the board isn't hot.
    2. Test incrementally. Here's my protocol
        3. Attach the ground + hot wires, and wait ~3 minutes. 
        4. If the board isn't hot, attach another pin to the circuit and ensure it also doesn't get hot.
         Repeat until every pin is connected and it doesn't get hot. Then you can test the software capabilities.
    3. If the board is hot, that is indictative of a short circuit or 2 pins making contact. Analyze your soldering and remove some extra solder from your pins. 
6. Clean up any excess solder/flux with alcohol and you're done.
7. BOOM! You REALLY completed it. Good job.

## My work
Here's what my soldering resulted in


<div style="display: flex; gap: 20px; justify-content: center;">
  <div style="text-align: center;">
    <a href="/github-portfolio/images/Me_With_My_Solder.webp">
      <img src="/github-portfolio/images/Me_With_My_Solder.webp" alt="Me with my solder" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">Me with my soldered board</p>
    </a>
  </div>

  <div style="text-align: center;">
    <a href="/github-portfolio/images/My_Solder_Job.webp">
      <img src="/github-portfolio/images/My_Solder_Job.webp" alt="Second Image" style="width: 100%; border-radius: 8px;">
      <p style="font-size: 14px;">My solder job</p>
    </a>
  </div>
</div>

## What's next?
The eagle-eyed viewers see the heart icon on the chip I soldered. It's specifically a <mark>Sparkfun ad8232</mark> used to create ECGs. I am planning on turning this into a full-fledged circuit and evolve it into a real-time mobile app. Now you see why the last post was BLE, huh. 😼
