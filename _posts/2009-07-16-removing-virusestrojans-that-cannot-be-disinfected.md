---
key: 167
title: 'Removing Viruses/Trojans that cannot be disinfected'
date: '2009-07-16T05:43:00+00:00'
author: Kiran
guid: 'https://kiran.co/removing-virusestrojans-that-cannot-be-disinfected/'
permalink: /2009/07/removing-virusestrojans-that-cannot-be-disinfected/
blogger_blog:
    - musemewithcoffee.blogspot.com
blogger_author:
    - 'Kiran (a.k.a. ThePhoenics)'
blogger_permalink:
    - /2009/07/removing-virusestrojans-that-cannot-be.html
categories:
    - Security
---

<div style="text-align: justify;">Hi, been a while since I blogged. Completely confused in life. Anyway, just to keep my blog updated, I wish to blog a procedure that I use to disinfect programs that cannot be disinfected using any popular anti-virus softwares. We often encounter viruses that cannot be disinfected using Anti-virus softwares. They only provide an option to delete the program infected by the virus. In case the program is rare or you feel it is important enough not to be deleted, follow the following method.

<span>The first question is “Why cannot the anti-virus disinfect the program?”.  
The virus softwares work on the basis of signatures. Often, this means a few parts of the program that are common to the virus. If the program contains these parts, then its infected. The viruses often attach themselves to the EXEs using some packing method. Packing is a process like zipping, if you allow me to be extremely vague (The exact procedure would be a topic of another post, on demand). So, you need to unzip…. uhm… unpack it :). So, how can you do it. There are several unpackers available online for each kind of packing. We can use any of them. However, my choice is [7zip](http://www.7-zip.org/) ! Amazingly, 7zip also acts as an unpacker and an unzipping program. Unzips any kind of compressed format automatically detecting its type.</span>

So, how to do it?  
Check the following virus program: It is compressed and uploaded in the following location  
http://the.phoenics.googlepages.com/Keymaker.7z  
It is password protected with the password: Virus  
Download it, and try to unzip and scan it using your favorite anti-virus program. The only option you find would be to delete it. So, if you want to use this program, its not possible.  
Install 7zip and use the Context Menu to unzip the exe.  
Once unzipped, you get a directory named $PLUGINDIR (or something like that). In it you find two exes. The one named svhost.exe is the virus. Delete it and use the other program, which is the desired program.

Congrats, you have disinfected the virus without deleting the exe, and without using any antivirus program!

Any questions, comments or if you need a more detailed info, add comments.

</div>