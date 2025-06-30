+++
date = '2025-06-29T17:16:24-07:00'
draft = false
title = 'Successfully soldered an electrical component!'
description = 'I had never soldered - before today. Lets talk about it.'
+++
# Why are you discussing soldering?
I recently had to solder. I want to have a discussion of why, what I used, how I did it, provide useful links, and showcase my work.
## Why did you learn to solder?
I have never soldered before. For an upcoming project (see the end of the blog for more info) - I forced myself to learn how to successfully do it. Here’s why I learned it — and why you should too:

- **Freedom** - You will be freely able to incorporate electrical components in future projects without relying on the OEM attaching electircal connectors. Sometimes, you don't have a choice and the OEM doesn't include *any* electircal connectors - in which case soldering is often the best (though not the only) option.
- **Repairs** - If a wire or an electrical component breaks, having the ability to fix it asap and not re-buy expensive components, or wait until a component arrives, makes you much more agile and self-sufficient.
- **It's fun!** - Completing a successful solder makes you feel awesome!

## What even is Soldering?

Soldering is **Really** cool. It lets you combine 2 pieces of metal together. 

Soldering is similar to welding - both result in 2 being pieces of something joined together. The key difference though is that in welding the pieces of metal are combined directly to one another, whereas soldering uses another metal (***The solder!!!!***) as the combining agent.


## What do I need to solder?
I'm going to provide some non-affiliated amazon links for all the things you need to solder. These are definitely lower-end products that save on $$ but still allow you to perform good solders. I will also explain the necessity of each purchase.


| Item      | Link | Explanation| 
| ----------- | ----------- | -----------|  
| Soldering iron.      | [Click me for soldering kit](https://www.amazon.com/Soldering-Kit-Temperature-Desoldering-Electronics/dp/B07GTGGLXN?ref_=ast_sto_dp&th=1)| Goes without saying you can't solder without the soldering iron. |
| Soldering wire   | (included in kit) | Solder wire is the bonding agent. Required. | 
| A wet sponge   | (included in kit) | Used to remove excess solder, flux, or oxidation from the soldering tip. A dirty tip is an inefficient one. You can use various other things like a wet rag, but sponge is what most people use.|
| Solder flux   |  [Click me for the flux](https://www.amazon.com/Solder-Soldering-Rosin-Lead-Free-Electronics/dp/B08MVXW4RY?ref_=ast_sto_dp&th=1)|  It's purpose is to burn and use nearby Oxygen. By taking the nearby O2, the Soldering wire and the component it gets attached to are able to bond better. |
| An electrical connector   | [Click me for the connectors I used](https://www.amazon.com/Jabinco-Breakable-Header-Connector-Arduino/dp/B0817JG3XN?ref_=ast_sto_dp) | Depending on your use case you may not need breakable pin headers. You could use various things like wires, LEDs, resistors, this part is variable.|
| Safety goggles | Any work. | Non-negotiable. Solder smoke is an irritant. Hot liquid solder entering your eye will hurt and possibly cause major damage.|
| Isopropyl alcohol | Any percent works. | Uses to remove excess flux off of equipment. |


## SAFETY 
Before we get into the main part of the article I just want to quickly explain some things.
- Solder and flux burn. Please solder in a clean workspace to avoid a fire risk.
- Solder and flux burn - so please solder in a well ventilated area to avoid lung irritation.
- The solder iron gets hot. You can burn yourself. Refer to the images below 

***PLEASE*** be careful when you solder.

## Alright I got the required materials. What are the steps to solder???

Easy.
- Find a clean and well ventilated area and bring your materials.
- Wet your sponge. Like really wet it.
- Attach the electrical connector/component to the other piece you want to connect it to. Can be a resistor to a PCB, or pin headers to  through-holes on a PCB, etc etc. Make sure the connection is tight and that the connection maintains itself without you holding it.
    - TIP!!! - A breadboard is a great way to keep pin-headers attached to a PCB
- Apply some flux onto the connection area (ex: the area where the pinhole meets the pin headers), and some on your iron before it is turned on.
    - I personally like to use another a detached solder iron tip to apply flux. Anything but the active solder iron should be used to apply flux.
- Connect and turn on your solder iron. Hold the non-metal area.
- Adjust the temperature. Between 315°C and 400°C is ideal. I like 350°C.
- Once the solder is hot (Since you had some flux on before you turned it on, you can see it melt), place it on one of the connectors. The heat does the work so you shouldn't apply a ton of force.
- Get your solder wire, and make it touch the connector. Since the connect is also being touched by the solder iron - the wire melts.
- YOU JUST SOLDERED!!! Conngrrraaaaatttsssts.
- Maybe wait a minute or two before touching since your PCB / combined component may be a little hot as a result.
- Remove excess flux with isopropyl alcohol and either a brush or a paper towel. 

## It's not *That* easy, right?
Welllll, yeah. There are a few things to be aware of.
- It <mark>will</mark> be an iterative process. 
- Make sure the solder you applied resembles volcanoes. With a thicker base near the bottom and a thinner base the near the top.
    - Don't worry if you applied too much solder - you can always apply more flux on the area with too much solder and you can hold your solder iron there to melt away the excess solder.
- Avoid having 2 pins be connected by the solder. This is a really fast way to short-circuit your component. Refer to the previous list entry.
    - If two pins are connected with solder, you can apply more flux in between both pins and 
- Test the component
    - Once the solder is on it can be used/tested immediately if the board isn't hot.
    - Test incrementally. Here's my protocol
        - Attach the ground + hot wires, and wait ~3 minutes. 
        - If the board isn't hot, attach another pin to the circuit and ensure it also doesn't get hot.
        - Repeat until every pin is connected and it doesn't get hot. Then you can test the software capabilities.
    - If the board is hot, that is indictative of a short circuit or 2 pins making contact. Analyze your soldering and remove some extra solder from your pins. 
- BOOM! You REALLY completed it. Good job.

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
