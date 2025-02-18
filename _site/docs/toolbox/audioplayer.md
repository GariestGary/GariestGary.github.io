##Basics

`AudioPlayer` allows you to structurize sounds used in your project and manipulate them. It uses albums as containers for clips, each album can have it's own audio source component binded and can be controlled through mixer group.

To add album go to Toolbox settings and click on 'Audio Player' tab. Here you can add a new album by clicking 'Add Album' button. Enter its name and decide if you want to use separate audio source, or use default. If you are using separate source, then you can define a mixer group for it if you want.

After album setted up click on 'Add Clip' button to add new audio clip. Enter its name and select actual audio clip, you can preview it by clicking 'Play' button or stop playing by clicking 'Stop' button below the 'Play' button or right after 'Add Album' button.

<img src="album_filled.png">

##Playong audios

To play audio you can call `AudioPlayer.Play(...)` method. It takes 6 parameters:

- source: an album name.
- id: clip name
- volume: volume of an AudioSource component (by default = 1)
- pitch: pitch of an AudioSource component (by default = 1)
- loop: will audio clip be looped (by default = false)
- playType: play behaviour (by default = ONE_SHOT):
	- ONE_SHOT: plays clip even if another clip is playing
	- STOP_THEN_PLAY: stops current playing clip if there is one and then plays provided clip
	- NO_INTERRUPT: does nothing if other clip currently playing, otherwise plays it normally