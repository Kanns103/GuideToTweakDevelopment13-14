# How to get started with tweak development for complete beginners on MacOS for IOS 12/13/14 - Kanns103






## Section 1 introduction:

So, you want to learn how to get started with iOS tweak development? Let’s hope that this guide will make it much easier for you to do so, make sure you have a mac. Let’s get started.






## Section 2 what you need:

To start off with, you need a MacBook or iMac. This tutorial is not for people who don’t own a Mac. Let me list the things required to get started with tweak development which we will set up next:


-	THEOS (A mac/windows environment that lets you create a tweak template and allows you to compile your tweaks).

-	XCode. If you are installing it off the AppStore, then you will automatically be prompted to install XCode 12, which breaks compiling for arm64e (iPhone XS and above). This is due to the XCode 12 toolchain. Luckily for you, I have created a bash script that lets you swap the toolchains easily without typing out the commands yourself and ruining your XCode. Do not use this script until prompted to do so.

-	A code editing software. I used to use Atom and it’s just brilliant, but it heated up my Mac a lot, so I switched to Sublime text. Another good one is Visual Studio Code.

-	Objective C knowledge would help, but it’s not necessary as we shall learn on the way.

-	Homebrew. Homebrew is the missing package manager that is required for tweak development on MacOS and many other things.

-	Latest SDK. SDK’s are what allow you to compile your tweaks.







## Section 3 basic knowledge:

Now that you understand what we need to create a tweak, let’s do some simple terminology within tweak development that most developers should know. If you already know these, you may skip it.


-	iPhone chips. For example, A12, A13, A14 etc.

-	Templates for your tweak. When you make a simple tweak template, you will be presented with 4 files, Tweak.x, .plist, makefile and control file. Your tweak.x is where your whole main code goes for your tweak, all your hooking and methods. Your plist file is where you target your tweak. For example, If you want to hide the dock, you would target com.apple.springboard because that is where you dock is located, on the springboard. But if you would like to make a tweak that targets all sections of your phone, like inside an app and the home screen, you can either use two plists, like com.apple.springboard and/or the bundle id of an app. Or, if you are locating many places and apps on your phone, the most realistic case would be to use com.apple.UIKit, which targets everything, but don’t worry, this doesn’t mean that if you want to change the dock that it will change everything, it means that you are able to hook into everywhere. 

-	Bundle ids. The bundle id of an app is the unique identifier that is mainly used for creating themes and apps. Each app and tweak that somebody makes, has to be given a unique bundle id. This can be anything, but they usually look like this com.name.tweak. So if I wanted to make a tweak that hides the dock, I would use my developer name and the tweak name like com.kanns.hidedock.

-	How to create new files through terminal. I’m not sure what version apple breaks this on, but you used to just be able to create a new file by naming it a specific thing, for example, if you wanted to create a control file, you used to just be able to name the file ‘control’ and it would change it to a control file. But, now on the latest versions, you can no longer do that. To create a file like that now, you need to go into terminal and use the word ```touch```, then the file name. So, like this, ```touch control```.







## Section 4 setting up:

So now that we covered the boring parts, let’s take a look into the juicier sections of tweak development, installing theos and everything that you need to create a tweak. Follow the steps below.


- Install XCode from the Mac Appstore. It’s a meaty download, but it is required.

- Installing THEOS and homebrew. You can follow [this](https://github.com/theos/theos/wiki/Installation-macOS) simple guide to install it. Don't skip this step, you need it.

- Once that is done installing, you need to run my script that I created which can be found on my GitHub named [Xcode11Toolchain](https://github.com/Kanns103/Xcode11Toolchain/blob/main/Xcode11Toolchain.sh) . This script fixes compiling for arm64e like we talked about earlier. Copy and paste everything in the Xcode11Toolchain.sh file apart from the top line ```#!/bin/sh``` into your Mac terminal window, then press enter. It will run and download some stuff, then when you are presented with an option menu, select number 1 and press enter, this will run some more commands. If you are unsure as to what is happening, the code you are pasting is all there. If anything goes wrong, or you just want the XCode 12 toolchain back for some reason, you can run the script again and select option 2. This will bring back the Xcode 12 toolchain and remove the 11 toolchain. Right, so now that you have selected option one, after it has run, just quit terminal.

- Downloading the SDK’s. Go into your name in finder, find the theos folder, go into the sdk folder and delete everything in there. Now download this file [here](https://drive.google.com/file/d/1JehPj-O5cOPmTp_KkDE2dwcLZDAs9mL6/view?usp=sharing) Once that is done, open it, drag the 137SDK folder inside your sdk folder in theos. If it is zipped, unzip it to a folder.







## Section 5 creating the tweak template:

So, now you have everything set up for creating a tweak, let’s get into it. We are going to create a new project without a preference bundle. Open up a fresh terminal window and type this ```$THEOS/bin/nic.pl``` and this should bring up some options (If you have done the steps before correctly). This is theos in action. Find the option that has iPhone/Tweak next to it, type the number next to it and press enter. 

- Next, the project name can be anything, obviously your tweak name. For the tweak we will make, we will be hiding the dock background, so let’s call it HideDock14. Once you have typed that, press enter, and you will be presented with another option. 

- This time, it’s the package name. This is the bundle id that I was talking about earlier. So, type your unique bundle id. Note that this all HAS to be lower-case.

- Next, the Author/Maintainer is your developer name. If you don’t have one, just use your actual name. Type that and press enter. 

- Next, the bundle filter is what fills out the .plist file I was talking about above. So because we will be targeting the dock, it will be ```com.apple.springboard```. By default, it is already that, so just press enter. 

- Then the next option which says Springboard, just press enter. And that’s it, your template has been created. Locate your name in finder and within the list of folders, there should be your tweak, titled hidedock14. Drag it to your desktop so it is easier to work with. 

- Last step for this section is to create a Tweak.h file using terminal and the ```touch``` command like we talked about earlier. To do this, cd into your tweak by opening a fresh terminal window and typing ```cd <drag your tweak file here>```, obviously without the '<>'. After that, type ```touch Tweak.h```, now you're done. This file does not come with the theos tweak template, but it is a nice add-on to make your tweak clearer.







## Section 6 creating the code:

- Open up your Tweak.h and put this at the top. This is normally required for most tweaks and if you don't have it, it won't compile.

```
#import <UIKit/UIKit.h>
```

- Open your Tweak.x and put this at the top of your Tweak.x, this will connect the Tweak.x and Tweak.h together:

```
#import "Tweak.h"
```

- Click your control file. Your control file is the information and restrictions of your tweak. Most things should be filled out so just leave it be. 

- Next, open up your makefile and we need to add in a few things. First, delete the line that says something like ```TARGET = iphone:clang:13.0.:7.0```, then at the top, put this. This will allow your tweak to work on A12 + devices. Remember, if you get confused, take a look at my files on FastCC on my github.

```
ARCHS = arm64 arm64e
```



- Next, you need to install OpenSSH on your iPhone located in your package manager (cydia, zebra etc). This just makes it easier for you to test the tweak and install it on your device. 

- Once that is done, go back into your makefile and put this in it nearer the top and replace ```yourip``` with your IP address of the phone you just installed OpenSSH on and you want to test your tweak on.

```
THEOS_DEVICE_IP=yourip
```

- In your makefile again, in-between your ```THEOS_DEVICE_IP=yourip``` and ```ARCHS = arm64 arm64e```, put this. It makes sure theos uses the 13.7 sdk we downloaded earlier:

```export SDKVERSION=13.7```


- Again, in your makefile, put this at the very bottom. This automatically resprings your device when you install the tweak onto your device.

```
after-install::

	install.exec "sbreload"
 ```
 
- Now that we have the makefile and everything else done, lets create the actual code. Next, open up your tweak.x and delete absolutely everything within it, this is just blank text. Go ahead and take a quick read if you like. If you are using sublime text like me, your code editor won’t recognise it as objective c so it will recognise it as plain text, but this won’t affect anything. Just go down to the bottom right, select plain text and select objective C or C from the list, now it will be coloured.

- So, the piece of code that we are about to write will only be a few lines. Let me break it down bit by bit. To start off with, you need a ```%hook``` and an ```%end```, always. A hook and end are important. A hook is the used so you can use the class you have found, and end is how you stop the code. So, type this,

```
%hook SBDockView
```

- We are doing this because we need to hook into the non-notched dock and hide it. We can find out what we need to hook with a tweak called Flexall. Flexall allows you to hold on the status bar then tap on what you want to change, it will then give you the view. After that, we can put that view into [Limneos header website](https://developer.limneos.net/?ios=13.1.3) and it will give you all the methods that come with it. Now, because you don't want this to be repeated, we will not use layoutSubviews, its generally seen as a last resort. Instead, we are going to use ```-(void)setBackgroundAlpha:(double)arg1```. We are using this because it means that bit will change the alpha of the background. And we want 0 because that's transparent, and because we can't return ```void``` we need to use ```arg1```. It should look like this:

```
%hook SBDockView
-(void)setBackgroundAlpha:(double)arg1
```

- If you’re a beginner, don’t worry, this is all very confusing and will continue to be until you get a hold of this a bit better, just try not to get overwhelmed.

- After that, we need to use ````%orig```` to set it to 0. The final main code should look like this:

```
%hook SBDockView
-(void)setBackgroundAlpha:(double)arg1 {
%orig(0);
}
%end
```



## Congratulations you have just hidden the dock background



- Now, we can compile. To compile, we need to cd into the project. But first, after making any changes, we need to save all changes, in Sublime text to save all, its option + command + s, but just to make sure, you can go to file and save all. 

- Next, open terminal, type ```cd``` then a space and drag your project folder and press enter. Once that is done, type in ```make package``` in terminal. If everything goes to plan, you will get a bunch of coloured writing. If not, you will get a red error. If you have, go back and make sure everything is typed correctly then try to compile again. If you still get the error, contact me on [Twitter](https://Twitter.com/kanns103) and we will resolve it.

- So, if everything went correctly and you compiled fine, you can now compile to your device. Type this again in terminal ```make package install```. Some of you will be presented with yes/no/fingerprint, type yes and press enter. 

- Next it will ask for a password. This is your root password for your iPhone. If you have not changed it, it is ```alpine```. Because we told it to respring after, it will ask for it twice, so type your password again when it asks you. When your device resprings, your dock should be hidden! To delete it off your phone, go into your package manager, refresh and it should be there as a package like all the others.







## Help, I have no idea how to continue from now!

Don’t panic, listen to the advice I am about to give.

I can’t stress this enough, LOOK AT OPEN-SOURCED TWEAKS ON GITHUB!!

Looking at open-source projects is the best way to learn tweak development because you can get an idea of how tweaks are made and the layout that comes with them. On my [website](kanns103.github.io) you can find a link to my GitHub and open-sourced tweaks. Currently there is a tweak called FastCC on there which speeds up your Control Centre for iOS 12, 13 and 14 devices, and you can see the code for it so you can practice. And please don't just read it for the sake of it, work through it and try to understand it. 

### How did I make the code?

Well first I went onto Limneos header website and typed in CC. I typed in CC because I wanted to get a start on what I needed to hook as I had no idea. A few things came up and after I experimented with a few hooks, I found the correct one, which was CCUI2AnimationParameters, I was able to look at the methods that came with it which turned out to be a speed, delay and tension. I tried all of them and returned the value to either 0 or 100. I did this because I needed to test out which works, and which doesn’t. I first tried the speed as it looked like the most logical thing to do but soon realised that it had a weird animation when pulling down to activate the control centre. So, I went onto tension and I wasn’t going to increase it, so I reduced it to 0 and it worked completely fine. If I were to increase the tension to 100 or 50 it would be really slow. It's all about trial and error.

### How did I start?

I started about seven months ago by creating themes. I started making themes because I had no idea how to make a tweak and there were no good tutorials out there, so I decided to work my way up. At the time I had no idea where I was going to be now, so I just took it in my stride. I searched up how to make a theme for iOS 13 and a good guide from PinPal came up. I followed his guide correctly and made my first theme. After that, I started making respring packs and extending my themes, but it got to the point where it was way too easy, so I started to learn how to create tweaks. 

I have already mentioned all of this on my website but for those of you who haven’t read it, the first thing I did like many people was to search up how to make a tweak on YouTube. Take it from me this is completely useless, and they do not teach you anything at all. Of course, they teach you how to install Theos and everything needed to make the tweak, but they just started randomly coding and I had no idea what they were on about. So instead, I decided to look at open-source projects and that’s when things started to take off for me. I looked at how tweaks were created and how people found the code, and the pieces started joining together. One of the main places where I learnt was the jailbreakdevelopers sub reddit. They helped me with any errors I was getting and helped me understand how to find the methods and hooks for my tweaks, which I told you about earlier. Of course, reddit and other users help is great, but you will learn best through trial and error.







## Got errors you can’t solve? 

Feel free to drop me a message on [Twitter](https://twitter.com/kanns103) or by [Email](mailto:kannsbusiness@yahoo.com)








# Done all that and want to make switches and options for the user to choose from?

This is called a preference bundle and it is what appears in settings so the user can select what options they want. To do this, you can use THEOS but you have to edit some folders to make it more simple. Lets say you already have your main tweak template without any preferences. 

- So first, you need to cd into your tweak in terminal, then run the theos command to bring up the options, then type the number that says iPhone/preference_bundle_modern. 

- Once that is done, fill out everything with the same name and identifier as the main code. 

- Then when it says XXX change it to something unique, like WOA. If you don’t, it will merge with a different tweak and mess up. After that, there should be a folder in your tweak called the same name as your tweak. 

- Now, in the new makefile in the main preferences folder you just made in the tweak, get rid of the very top line that says something like ```TARGET := iphone:clang:latest:7.0``` and replace it with ```export SDKVERSION=13.7```, then after that, put these two underneath it, and like before, change ```yourip``` with your ip address:

```ARCHS = arm64 arm64e```
```THEOS_DEVICE_IP=yourip```

- In your preferences makefile, put this at the bottom:

```
internal-stage::
	$(ECHO_NOTHING)mkdir -p "$(THEOS_STAGING_DIR)/Library/PreferenceLoader/Preferences"$(ECHO_END)
	$(ECHO_NOTHING)cp entry.plist "$(THEOS_STAGING_DIR)/Library/PreferenceLoader/Preferences/TweakName.plist"$(ECHO_END)
```

- Inside your preferences folder, delete the whole folder named layout. 

- After that, you need to create a entry.plist file. To do this, cd into the main preference bundle folder inside your tweak, and type ```touch entry.plist``` in terminal. Remember ```touch```, like we discussed before?

- Go inside the entry.plist and put this in:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>entry</key>
	<dict>
		<key>bundle</key>
		<string>TweakName</string>
		<key>cell</key>
		<string>PSLinkCell</string>
		<key>detail</key>
		<string>XXXRootListController</string>
		<key>isController</key>
		<true/>
		<key>label</key>
		<string>TweakName</string>
	</dict>
</dict>
</plist>
```

- Change ‘YourTweakName’ to your tweak name, and the 3 XXX at the start of XXXRootListController to what you made at the start in theos. Make sure you change everything, or you it will not work. Leave the line that says ```<key>label</key>```

Now that you have created the folders needed, follow the next steps.


Alright so that is all done, lets move on.

## Tweak.x

If you followed the code above to hide the dock, then you can just add to it. We need to create a key so we can link them to the Root.plist. To do this, we need to go inbetween the brackets after ```-(void)setBackgroundAlpha:(double)arg1```. First, we need to call ```%orig```. ```%orig``` is a function that calls the original value. Without it, there is a good chance your switch will mess up. After that, we need to add in the switch, which will start with 'if', then in brackets would be your unique value, this can be anything. The final code with the key in it will look like this:

```
%hook SBDockView
-(void)setBackgroundAlpha:(double)arg1 {
%orig;
if(yourvalue)
%orig(0);
}
%end
```


- Now we have done that, we need to put a little code at the bottom. You need to replace ```TweakName```, ```name``` and ```yourvalue``` with the correct information. with your tweak name. Make sure you keep it exactly the same as everything else, apart from the control file where the identifier has to be lowercase. The code is:

```
static void loadPrefs(){
    NSMutableDictionary *prefs = [[NSMutableDictionary alloc] initWithContentsOfFile:@"/var/mobile/Library/Preferences/com.name.TweakName.plist"];

    if(prefs){

    yourvalue = ([prefs objectForKey:@"yourvalue"] ? [[prefs objectForKey:@"yourvalue"] boolValue] : yourvalue);


      }
}

%ctor {

  if ([[[[NSProcessInfo processInfo] arguments] objectAtIndex:0] containsString:@"/Application"] || [[[[NSProcessInfo processInfo] arguments] objectAtIndex:0] containsString:@"SpringBoard.app"]) {
CFNotificationCenterAddObserver(CFNotificationCenterGetDarwinNotifyCenter(), NULL, (CFNotificationCallback)loadPrefs, CFSTR("com.name.TweakName/settingschanged"), NULL, CFNotificationSuspensionBehaviorCoalesce);
loadPrefs();

  %init;
}
}
```

- Let me do a little explaining. This part below is how you create a key for the preferences, specifically a BOOL value, which is either yes/no. This bit, you have to add in another one of these for each key or switch that you add and remember to change ```yourvalue``` with your actual value. 

```
yourvalue = ([prefs objectForKey:@"yourvalue"] ? [[prefs objectForKey:@"yourvalue"] boolValue] : yourvalue);
```

# Tweak.h


- Now we have that, we need to add a few more things to our Tweak.h. You also have to create a new one of these for each switch and change ```yourvalue``` again, like so:


```
static BOOL yourvalue = NO;
```


# Root.plist

Now we have done the Tweak.x and Tweak.h, we can move onto creating the switch. Replace ```YourTweakName```, ```yourvalue``` and ```name of your switch``` and ```com.name.YourTweakName``` to what you made them. And please, make sure you change everything correctly, including the ```name``` bit!!!!! Go into your Root.plist and remove everything and replace it with this:


```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>

		<dict>
			<key>cell</key>
			<string>PSGroupCell</string>
		</dict>
		<dict>
			<key>cell</key>
			<string>PSSwitchCell</string>
			<key>default</key>
			<false/>
			<key>defaults</key>
			<string>com.name.YourTweakName</string>
			<key>key</key>
			<string>yourvalue</string>
			<key>label</key>
			<string>Name of your switch</string>
			<key>PostNotification</key>
			<string>com.name.YourTweak/settingschanged</string>
		</dict>
	</array>
	<key>title</key>
	<string>YourTweakName</string>
</dict>
</plist>
```

- And that is it, go ahead and save everything using the save all button, then compile it to your device by using ```make package install``` like earlier. 



# Have questions? Need help?

Contact me on [Twitter](https://Twitter.com/kanns103) or [email](mailto:kannsbusiness@yahoo.com)

