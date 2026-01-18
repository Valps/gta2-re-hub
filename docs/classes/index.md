# Classes

Here we explain some classes used by GTA2.

## Essential classes

* [Fix16](fix16.md) - Fixed points, which replaces floating points.
* [Ang16](ang16.md) - Also fixed points but with lesser precision.

## Other classes

* `miss2_0x11C` - The dorsal spine of missions design and some map features.
* `Gangs` - The code behind GTA2 gangs.
* `Frontend` - Main menu
* `Player` - Handling inputs, weapon list, storing and loading save player data etc
* `Ped` - Most of Ped AI
* `Char_B4` - The class behind the Ped sprite: walking & running behaviour happens there.
* `map_0x370` - Functions dealing with the map: get blocks, get highest non-air block at XY, get road arrow directions, find pavement/railways at XY, update Z from gradient slope etc.
* `MapRenderer` - Graphical rendering of the map: blocks & tiles
* `PedGroup` - Group of peds which have a leader. Instant gangs, groups created from script and police/ambulance crews are examples.
* `Police_7B8` - Police Manager: it's on the top of police classes hierarchy.
* `PoliceCrew_38` - Seems to be police crew.
* `PoliceRoadblock_A4` - Police roadblocks
* `Ambulance_20` - Ambulance crews (`Ambulance_110` is the pool)
* `Firefighter_28` - Firetrucks
* `Sprite` - Sprites: 2D objects in the game
* `PublicTransport` - Trains & Buses.
* `TileAnim` - Tile animations
* `Weapon_30` - Weapon mechanics
* `Weapon_8` - Weapon related
* `Object_2C` - Object (like cones, balls, tokens, powerups etc)
* `Object_5C` - Seems to be Object Manager
* `Particle_4C` - Seems to be the particle mechanics (such as flames from flamethrower, bullets etc)
* `Particle_8` - Particle Manager: contains functions that emit particles (like water cannon, flamethrower, water particles etc).

## Unnamed classes

Classes not renamed yet, so their names are provisory.

### Known

We didn't give them a name yet but we kinda know what they do:

* `gtx_0x106C` - Kind of texture manager. Deals with sprite indices, palettes and tiles.
* `sharp_pare_0x15D8` - Something related with textures. Often worked with `gtx_0x106C`.
* `sharp_bose_0x54` - Maybe related with time elapsed or frame counting
* `jolly_poitras_0x2BC0` - Deals with Frontend player scores table and loading player slots from save data.
* `BurgerKing_67F8B0` - Keyboard/gamepad input handling (for live and replay modes)
* `Hamburger_500` - Related with car paths.
* `KFC_1E0` - Seems to be a emergency crew (for police and paramedics)
* `sleepy_stonebraker_0x6C` - Something related with the credits roll.
* `magical_germain_0x8EC` - Something to do with text sprites and their connection with characters.
* `lucid_hamilton` - Contains many infos about the game: map name, scr name, sty name, current statistcs (vehicles hijacked etc), gamemode type & max number of players etc.
* `infallible_turing` - Sound object (such as the ones created from script).
* `Shooey_14` - Cop crime report. That cop radio sound we hear when we commit crimes.
* `thirsty_lamarr` - The class behind the numbers on player Hud (lives, multipliers & score). They slide up/down as you increase/decrease the number.
* `eager_benz` - Player score class.
* `Orca_2FD4` - Some kind of path finder for peds or cars.
* `PurpleDoom` - Some Sprite Handler: Something to do with collisions checks and sprite rendering order.
* `Rozza_28` - Seems to be related with the sounds of pickups (like tokens), some specific actor voices liked "Machine-Gun" and car clash sounds.
* `Wolfy_30` - Explosions. We don't know if it's only the sprites or also includes shockwaves physics.
* `xenodochial_morse` - Something to do with credits rolling.
* `Montana_2EE4` - Related with sprite rendering.
* `Marz_3` - Ped patrol point.
* `Phi_74` - Object properties (like mass, friction, if it has shadows etc)
* `youthful_einstein` - Multiplayer class. It includes mostly tag game mechanics.

### Unknown

We know very little or nothing about what they do:

* `sad_mirzakhani` - Very sad we know nothing about it.
* `distracted_einstein_0xC` - Unlike the youthful one, we don't know nothing about this Einstein (besides being distracted).
* `Varrok_7F8` - Maybe related with gun / projectile collision.
* `zealous_borg` - Maybe related with crimes
* `silly_saha_0x2C` - Silly

## Not a class: files

* `winmain.cpp` - Win Main & functions on the top of call hierarchy.
* `error.cpp` -  Fatal Error handler

## Museum

See other not-longer-used names [here](museum.md).
