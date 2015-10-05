[Metadata]
title: Design a Music Player
date: 2015-10-05 23:07:46

[Tags]
difficulty: 3
categories: mvc
source: anonymous


[Description]

Design a music player like Winamp.

![Alt text](http://wizmann-pic.qiniudn.com/15-10-5/49325408.jpg)

[Solution]

### 1. Requirement analysis

| Requirements | Solutions |
| - | - |
| Play music | Use the api to play audio file |
| Show the info of the playing music file | load the info from the file, and use a timer to show the play time / remain time |
| Control the volumn (and some other properties) | use the api provided by the audio playing api |
| The playlist | use an array / list to store the list |

### 2. Design anaylisys

In the requirements analysis, we can find out that the music player is strongly rely on the auto playing API which provided by the codec. So a "Codec" is necessary for our music player to encapsulate the audio APIs.

At the same time, the "Playlist" is also a important part for the player. User can "add", "remove" or "save" the playlist. And user can play the whole list in order, in shuffle or keep playing a same music repeatly.

At last, we need a "ControlCenter" to provide all the information which is needed by the user interface.

### 3. the MVC-like pattern

![Alt text](http://wizmann-pic.qiniudn.com/15-10-5/26286902.jpg)

The "view" module is actually the user interface. It communicate with the "controller" module to change the status of the player and get some essential information.

The "controller" modules offers multiple interface for UI. To avoid make a "god object", we create two handler classes: "PlaylistHandler" and "CodecHandler" to divide the "controller class", to make them focus on specific problems of different facet of a music player.

The "model" modules is in charge of the playlists. And the "CodecUtils" modules is the one responsible for the music playing, file detection, etc.
