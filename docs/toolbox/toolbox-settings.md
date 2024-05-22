From this window you can adjust main settings, edit initial pools, set up audios and edit properties in database.

'Toolbox Settings' window has three tabs:

- Main Settings
- Pooler
- Audio Player

Let's look all them.

###Main Settings

<img src="main_settings.png">

There you can edit following properties:

- `Resolve Scenes On Play`: Changes OnPlay behaviour. If enabled, then Toolbox will close all opened scenes and start all with `MAIN` scene (after stop playing, all scenes will be restored).
- `Time Scale`: An ordinary time scale, that affects all scripts, derived from `MonoCached`.
- `Target Frame Rate`: Default target frame rate option, which will be setted at startup.
- `Initial Scene Name`: The scene that will be loaded first after `MAIN` scene loaded.
- `Initial Scene Args`: Arguments for initial scene.

###Pooler

<img src="pooler_empty.png">

From this window you can edit initial pools list, adjust properties of each and set garbage collector interval.

You can create new pool by clicking 'Add Pool' button.

<img src="pool_added.png">

Each pool has following parameters:

- `Pool Tag`: The tag, through which you can retrieve pooled object.
- `Prefab`: Prefab which pool will instantiate objects.
- `Initial Pool Size`: Count of an objects tha will be added to the pool at start.

<img src="pool_filled.png">

To delete pool, simply click button with trash icon.

You can search necessary pool by writing its tag in search field.

###Audio Player

<img src="audio_player_empty.png">

From this window you can edit audio albums list and adjust properties of each.

You can create new album by clicking 'Add Album' button.

<img src="album_added.png">

Each album has following parameters:

- `Album Name`: Name of the album, through which you can get access to clips it containing.
- `Use Separate Audio Source`: If enabled, then Toolbox will create separate audio source for this album.
- `Mixer Group`: Mixer group through which you can control volume, effects etc of the audio source, binded to this album.

To delete album click button with trash icon.

To add clips in album click 'Add Clip' button.

<img src="audio_clip_added.png">

Each clip has following parameters:

- `Clip ID`: ID of the clip, through which you can retrieve it.
- `Audio Clip`: Actual audio clip asset.

<img src="audio_player_filled.png">

To delete clip click button with trash icon.

You can search necessary album by writing its name in search field.