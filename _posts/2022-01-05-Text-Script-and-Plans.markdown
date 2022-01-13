---
layout: single
title:  "Text-Script and Plans for the Future"
date: 2022-01-05 23:00:00 -0500
categories: Bootstrap Programming Python Text-Script
comments: true
typora-root-url: ..
---

Hello World, today is one of the first days where I blogged two days in a row. Granted, yesterday's blog was pretty depressing,  but it was an important step in clearing my mind and is probably the reason I can blog again so quickly.

# Inspiration

Today, I am starting a new series on an upcoming project I have been planning to work on for a while. Around 10 years ago, I worked for Humber College IT and used a very helpful app called "Texter". It allowed you to save blocks of text that could later be recalled by typing a shortcut into any text input. Typing this would replace the shortcut with the saved text. When I started working at my current job, I once again found a use for this program since we had to type template responses to customers very frequently. Sadly, I found that this program had not been maintained for years, and similar programs that provided the same utility cost money, and often had per user licenses. Instead, I chose to code a replacement for it and share it with my team.

# The Birth of Text-script

Well, I ended up being successful and wrote an app called "Text-Script", and after a few versions, I halted development on this app as it did the job well, and I wanted to move onto another project. I shared this app with my L1 team as well as our L2 team and was happy to see that many people found uses for it. My team uses it to this day, and I have had people approach me to share their statistics from the app to show me how much time it saved them. I [shared my project on Github](https://github.com/GeorgeCiesinski/text-script) and was very happy to see that it accumulated 74 stars, which was a great accomplishment for a fledgeling developer as myself. Despite this, the app is actually.... pretty bad. In utility, it works exactly as I intended, but there is no GUI and it outputs all of the statistics and relevant data to a console. The settings had to be manually changed in a `.ini` file, and the individual "text blocks" had to be stored in `.txt` files named `#shortcut` where the actual shortcut would be whatever shortcut you wanted to type after the pound key. There was also no way to change the pound prefix to something else, because it was hard coded. I will give myself some credit though because through sheer coincidence, it turned out that saving a folder with these text blocks in a google drive or onedrive folder allowed you to keep a central text block repository that could be updated, and all of the agents with access to this folder would have their version automatically update to include any new text blocks.

# Text-Script 2.0

Well, since then I have pursued many other projects as I planned. I discovered an interest in programming REST APIs, working with SQL databases using PostgreSQL and SQLAlchemy, and learning Bootstrap 5. I am very lucky to be a part of an ambitious project with a group of developers named "CaterClock" where we worked on a Catering app using a Bootstrap 5 front-end and a Flask back-end. We have been developing this app for about two years now and have grown from three founding developers to a team of 10. I learned a lot about this process and decided that this was the future of Text-Script. My idea for the next version of this app is to build it in a similar way to SABnzbd. This is essentially a nzb server running on your computer which hosts a front end you can visit in any website to change settings or view the progress of downloads. The difficulty is that the listener needs to run in parallel with the rest of the program, so multi-threading may be necessary.

## Listener and Controller

The backbone of this project is the Listener and Controller. As the name implies, the Listener listens to key strokes, and the controller controls the keyboard. I will go into these two functions separately below. There are a few Python libraries which provide these functions. [Pynput](https://pypi.org/project/pynput/) which I have used in the original Text-Script has worked wonderfully in the past, but I will also consider an alternative library called [PyAutoGUI](https://pypi.org/project/PyAutoGUI/). I have very little experience with the latter library, so I will have to do some research before I decide which one to use.

### Listener

The purpose of the listener in Text-Script is similar to a key logger, but instead of logging keystrokes, it listens for whatever prefix the user chose for their shortcuts. Upon detecting this, it then listens to the following keys until either space, tab, or enter is pressed. Those three keys would signal the end of the shortcut and tell the program to search the database to see if this shortcut exists. If it does, then it would instruct the controller to copy the text block into the clipboard, and paste it.

### Controller

As the name implies, the controller controlls the keyboard. This is neccessary because some kind of input is required in order for this app to interact with various text fields, such as the body of an email, a word processor, or a messaging app. Earlier, I mentioned that the controller would copy the text block into the clipboard and paste it. Originally, I thought it would be better to instruct the keyboard to type the text-block out, but in practice it was kind of ridiculous. Using a shortcut would result in the text-block being typed out kind of like the 80s hacking movie where the hacker is typing a giant command into a console. If the text-block was large, then this process could take several seconds, and clicking during the process would shift the focus to another window and interrupt the text-block part way through. On the other hand, copying the text-block to the clipboard and pasting it was instantaneous.

It did come with a drawback that was never addressed in the original Text-Script. Using a text-block would overwrite the last item in the clipboard so that pasting would result in the text-block being pasted. Despite the fact that I came up with a solution, I never ended up coding it. The solution is quite simple. Instead of simply copying the text block, all that is needed is to first store the previously copied text into a variable, use the copy/paste function to paste the text-block, and then copy the original text. The user wouldn't notice any difference unless they relied on the Windows 10/11 clipboard function which allows you to see a number of previously copied items, but even then they can just ignore the text-blocks.

## Back-end

Instead of downloading nzb files, my plan is to host a REST API which interacts with some kind of simple SQL version like SQLite to store shortcuts, text-blocks, and metadata that will let users organize the text blocks. I am not exactly sure which version of SQL to use, although I am leaning towards SQLite because it stores the database in a single file. This would save me the time of building some kind of central database that a team could utilize to create common text blocks. Instead, I could store the database on any kind of cloud drive, and write an update method that checks the metadata of the remote database to see if there have been any changes from the locally downloaded version. Alternatively, if the user has access to the cloud drive and it updates automatically so that their local version is always up to date, then I can leave the process of updating from the remote database entirely to the cloud service. At the end of the day, I want this to be open-source free ware that anybody could modify for their needs so that people have free options instead of having to rely on paid ones.

As for the REST API, I honestly don't know if this is a good idea or not. In theory, it should work as the API would run on the localhost and would listen for requests from any kind of front end. This might even be a good idea as it might open this app to the possibility of using customized front ends if anybody wants to develop one down the line. On the other hand, I might be going overboard, and there might be an easier way to communicate between the front-end and the database. After researching this topic for months, I figured it doesn't hurt to try, and I can always rewrite the back-end in the future if I need to.

## Front-end

As for the front-end, this is mostly uncharted territory to me. I know how to make a website in Bootstrap 5, and I have some experience with HTML and CSS if needed. This said, I am weak when it comes to including variables from the database in the rendered web page. We use this functionality in CaterClock which updates the front-end with data from the back-end but I am only starting to get exposure to this as I am mainly a back-end developer on this project. I can't speak much for the mechanics of this, as I will have to write another update in the future when I have more experience with it, but I can speak about the design.

The back-end or listener would run as a process in the system tray, so it should be possible to give it a context menu when it is right clicked so that the user can select the option to open up the interface. Upon clicking this, the user would be brought to a landing page. To be honest, I haven't thought about this part too much because I will probably have to end up writing another block where I plan out the various pages needed, so I will just ball park it for now.

One of my favourite things is minimalism and simplicity. To apply this to the front-end, it would mean that the interface would bring up the information the user would want to see first. In the case of Text-Script, I imagine the primary function a user would use is to create, edit, and delete text-blocks, so the landing page would need some way to display the various user defined categories, and the text-blocks within them. If possible, this would be shown in some kind of tree heirarchy on the left, and would display the selected text-block on the right. Of course, this would mean one of two things. Either every single click would require a new request to retrieve the data, or the database could be preloaded and only larger items like images would be requested separately. In addition to this, there could be a button to delete or edit the name, shortcut, and content of the text-block.

Besides the landing page, I think this app could also use a settings page where the prefix (or prefixes) can be defined. It can also contain some additional settings such as the local and remote databases, administrator settings that can be used to lock a database from being edited by others, and a few other functions which don't come to mind right away.

Finally, I would probably need an about page which would be similar to an app landing page and would link to the github, website, and any other information the user might want.

# Platforms

I will primarily be developing this app on Windows, but as a Windows, Mac and Linux user I appreciate that there are some users who are native to the other platforms and will want to use this as well. I will do my best to test all of the features on all the platforms. Some parts of this like the front and back-end will work natively on any of the platforms, but my biggest concern is the listener and controller, and my intent to run this as a service. I do not have a lot of experience writing services and I don't know how easily it will be to write this for all of the platforms. To remedy this, I will probably fragment the code into independent files that can be more easily modified, and open the project up to the open-source community once I am ready for contributions. I have had a lot of luck in the past getting help from contributors with more skills in certain areas than myself, so I am confident that the open source community would help me create a multi platform project.

# Name

I also wanted to touch on the name briefly. My original app was called Text-Script, but I am not 100% happy with this name. I will probably be workshopping some names, and I might even ask for some feedback to help me select a name that is suitable, and gives a hint of what the app does.

# Conclusion

So that about wraps up the components I will need to create for this app. I plan on making more blog posts as I build the specific parts of this project. This post didn't go into too much detail partly because it is nearly midnight and I have to go to sleep soon, and partly because despite being a "simple" and "minimalistic" project, the reality is that it will probably take many thousands of lines of code, months of development, and lots of debugging. I will likely have to do a lot of research during development, and alter my plans as I find either better ideas, or that my current ideas simply wont work - but that is the fun of blogging. At the end of the day, it is to keep a log of my development including the good and the bad, and to learn from it.

I hope that you enjoy this series. Until next time!
