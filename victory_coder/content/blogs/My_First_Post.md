+++
title = 'My First Post'
date = '2025-03-13T14:32:22-07:00'
draft = false
+++
## Introduction

Making this website was an experience. Having never used Bootstrap, or Hugo, it was **exciting** to get all of this working. There were some parts that were frustrating, some parts fascinating, and the end result is amazing. Here's a bit of reflection, and advice on how to make your own website using Hugo too.


## Here is what I learned.
<html>
<head>
<style>
.red-list {
    color:  #ee1c1c;      /* Change text color to red */
    font-size: 18px; /* Change text size */
    margin: 0;       /* Remove default margin */
    padding: 0;      /* Remove default padding */
}
.green-list {
    color:     #10d500
 ;      /* Change text color */
    font-size: 18px; /* Change text size */
    margin: 0;       /* Remove default margin for nested lists */
    padding-left: 20px; /* Add some indentation for nested lists */
}

</style>
</head>
<body>
Pros:
<ul class="green-list">
    <li>Hugo is a pretty fast & powerful static site generator for POCs or Portfolios.</li>
    <li>Hugo lets you write your content (Like blogs!) in Markdown files.</li>
    <li>Its really nice to use community-created Themes to focus more on content than styling.</li>
    <li>Hugo's folder structure is jarring at first - but it is simple and elegant. </li>
    <li>The website leverages a TOML config file that allows you to pass around data easily. </li>
</ul>

Cons:
<ul class="red-list">
    <li>Web-technologies differ between templates - can be a problem for beginner developers.</li>
    <li>Limited community support makes learning some of the quirks of Hugo difficult.</li>
    <li>The layer of abstraction between front-end code and the TOML file can make it hard to edit unwanted features.</li>
    <li>Since it creates Static Sites - if you want any more dynamic features you are out of luck :c </li>
</ul>

</body>
</html>

## How to learn Hugo quicker than I did.
To help your adoption of Hugo, here are some of my suggestions.
- The File System is confusing at first - but here's some things that will help.
    - The TOML file in the root directory passes data through PARAMS. Something you can access in all files.
    - You can add your own Params to existing Params (ex: adding "img" to my work experience params in TOML to pass an image to the Experience section's HTML file for rendering)
    - From there you can go into a Section html file, and reference said params to render or do something extra.
-  [**Luke Smith's**]( https://www.youtube.com/watch?v=ZFL09qhKi5I&t=0s) video is a godsend. If you are at all wanting to look into Hugo, start here!!
- Images should be stored in the Static folder. From there they are easily accessed. (Ex: image in /static/docs/flowchart.png would be accessed like "/docs/flowchart.png")  
- The **TOML Rules all**. The TOML config file in your Root Directory is the main entry point of the site. It defines what themes are used if any, what sections are present, and more. If your page doesn't render - check here first.

I hope this was educational. This is the beginning of my website. I will update it more with things I am working on - like my Arduino projects, game development, Azure Certificate progress, and job hunting news. I hope to keep in touch.