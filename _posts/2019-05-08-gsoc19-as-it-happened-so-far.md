---
layout: post
title: "GSoC'19: As it Happened So Far"
image: gsoc19.png
categories: GSoC
excerpt: "MacPorts accepted my proposal for GSoC'19. Here is a little about the project and how the journey has been through the application period"
comments: true
---
It was 11:30 PM IST, day before yesterday, I picked up my phone and headed over to GSoC'19 website. My proposal was approved and for a while I did not know how do I react! It was one hell of a moment for me. But the celebration didn't last long- I had to prepare for the exam which I just took today. Here is me quickly trying to wrap a summary of the last two months. I will try cover most things with various upcoming posts. This is a really short summary, I have got to vacat the hostel and travel to my hometown- the deadline is on head.

I wasn't totally sure of what I was entering into when I started preparing for GSoC'19- 100% new to open source.
All I knew before was that source code open to all- "Open Source". Well, I was very wrong. Open-source has much
more associated with it. It is about learning and sharing, it is about keeping all the processes open to
everybody, it is about doing things that are helpful to a user and a developer willing to learn. It is hard
to define open source- well a Google Search can give you the technical definition but it is barely near the feeling
which you get everyday when you see volunteers around the globe coming in to share, suggest and help.

I was late with my preparations for GSoC'19. I began only after the participating organisations were announced
around 27th Feb, but I made sure that I gave my best in the time that was left. I read about a few organisations- and the first one that I decided to go with was the one I stick
to throughout the process- MacPorts. People suggest that you should at least submit two proposals- within the same organisation
or with different organisations. That might be true- but I acted differently- one organisation and one project and
give your everything to it.

So, I found about MacPorts and decided to go with it. MacPorts is a package manager for MacOS- packages are called ports
here. Initially it was hard to keep going because of the language in which most of the code-base of MacPorts is written- TCL.
TCL was completely new to me but I started learning it- it is a nice language. To learn about MacPorts I read documentation, 
subscribed to the mailing lists and went through the list of suggested projects. They were hard to grasp 
because the language and the software (MacPorts) both were totally new to me. But I did stick till I found something
I was totally comfortable with- a Django App, with a twist.

### The Project I Chose
My project is to make an app that would serve as an all-in-one place to find all the information related to ports- build history, installation statistics and the obvious one- basic details (descrition, version, dependencies, maintainers etc.). As it may sound- this is not a straightforward django app and a significant amount of tricky work has to be done outside of django. Django here is just a means of storing and showing the information. The most of the amazing work has to be done outside Django.

To start- how do you get those basic details about each port? The command `portindex` is of help here. You generate the Portindex which gives a TCL array containing the information of about each port. This array is then converted to JSON using `portindex2json.tcl` and the JSON is then loaded into the PostgreSQL database. There are around 23000 ports and this process is slow- so keeping it up-to-date is a challenge (yet to be solved).
Build data is extracted from buildbot. 'Installation stats' is my favourite part- from database design to the views everything is going to be lot more challenging and fun. A port 'mpstats' (aleady available) is supposed to submit a JSON object weekly for users who install it. The object would contain info about installed ports and the versions of OS, Xcode, MacPorts. App would catch this object and utilise the object with correct database design to display detailed statistics.

I know I have just scratched the top about the project and I apologise if the details are not enough (well, they are not) to make things clear. You can have a look at the [proposal](https://drive.google.com/open?id=1MwUHiwcOA4qozKRObIq16v940w6Zm_1_) to take a deeper dive.

### Mentors and Community
I was entering into something new and what could be the biggest help to me was someone to guide. I could not get things more right in this case- the potential mentors of my project (Mojca Miklavec and Umesh Singla) and the amazing MacPorts Community is the very first reason why I could create a proposal worth considering. I received help anywhere I got stuck and that too quite fast. We already have some code in place and I have received some very crucial code feedback already. You can visit the repository here [macports-webapp](https://github.com/macports/macports-webapp).

### The Two Most Productive Months
Beginning from first week of March to today. These two months have taught me the most when it comes to my career as a developer. At every step I realise- "Oh! I have been so wrong about this". I have been loving it- learning new things and correcting old things. But this is fun only because I have a great guidance in form of my mentors. I have learnt about databases (doing a Coursera course on suggestion of Mojca), about Git, about coding ethics and a lot more. There is still so so much room for improvement and so much new to learn that I believe these summers are going to be amazing.


### What Now?
I am in love with my project- I was counting each day of my exams so that I could get back to working on it. We are in the Community Bonding Period right now (7th - 26th May) and official coding begins from 27th. In the community bonding period I am supposed to learn more about the community and plan with my mentors about the course of the coming 3 months. But I will keep working on my project during this period also. It will nice to fix some things and bring the already available code in a nice shape before the actual coding begins.

Bye Bye for now! I am excited about what is coming, and also that I am going Home!

Thanks for the read- feel free to discuss anything in the comments below.