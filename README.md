# SurvivalGame - Reverse Engineering

<p align="center">
  <img src="Result.jpeg" alt="Survived in Texas" width="150">
</p>

I downloaded the APK file, uploaded it to the website http://www.javadecompilers.com/apktool, and got the project files.

After looking through the folders and files, I found 2 Java files that matched the files typically used in an Android project.
I opened a new Android project and copied the activity files as well as their corresponding XML files.
I went over the errors, searched for the missing files, and added them to the new project I created.
When I copied a line into the strings.xml file, I noticed there were...
I ran the app, and after pressing the button, it crashed.
A quick glance at Logcat showed me that the AndroidManifest.xml file needed to be updated:
There was a missing Internet permission, so I added:

```java
<uses-permission android:name="android.permission.INTERNET" />
```

After that, the app worked, and I had to figure out the correct order to press the arrows.
I looked into the Activity_Game file, which receives the id and state from the previous activity.
The state is the string (the city name) taken from the seventh digit of the ID. For example:
id = 846772548 -> state = data[4]
Each digit of the ID undergoes a modulo 4 operation, and the resulting value is stored in the steps array.
There is another array for the buttons:

0 -> Left
1 -> Right
2 -> Up
3 -> Down

According to the values in the steps array, you need to press the corresponding buttons in the game, and that's how I got the message:
"Survived in Texas"

