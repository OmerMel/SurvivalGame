# SurvivalGame - Reverse Engineering


  ![Alt text](Result.jpeg "Survived in Texas")


I downloaded the APK file, uploaded it to the website http://www.javadecompilers.com/apktool, and got the project files.

After looking through the folders and files, I found 2 Java files that matched the files typically used in an Android project.<br />
I opened a new Android project and copied the activity files as well as their corresponding XML files.<br />
I went over the errors, searched for the missing files, and added them to the new project I created.<br />
When I copied a line into the strings.xml file, **I noticed there were zero-width characters**, so I removed them.<br />
I ran the app, and after pressing the button, it crashed.<br />
A quick look at Logcat showed me that the AndroidManifest.xml file needed to be updated:<br />
**There was a missing Internet permission**, because I didn't copied the AndroidManifest.xml from the game, so I added:<br />

```java
<uses-permission android:name="android.permission.INTERNET" />
```

After that, the app worked, and I had to figure out the correct order to press the arrows.<br />
I looked into the Activity_Game file, which receives the id and state from the previous activity.<br />
The state is the string (the city name) taken from the seventh digit of the ID. For example:<br />
id = 846772548 -> state = data[4]<br />
Each digit of the ID undergoes a modulo 4 operation, and the resulting value is stored in the steps array.<br />
There is another array for the buttons:<br />

0 -> Left<br />
1 -> Right<br />
2 -> Up<br />
3 -> Down<br />

According to the values in the steps array, you need to press the corresponding buttons in the game, and that's how I got the message:
"Survived in Texas"

