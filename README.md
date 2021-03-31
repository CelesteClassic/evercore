# Evercore
A modified version of [Smalleste](https://github.com/CelesteClassic/smalleste) built for modding.

## Features

  * Large / scrolling level support
  * Level table system and mapdata string loading
  * Moving solid / semisolid object system
  * Object draw layering
  * Support for multiple berries per level
  * Tons of free tokens
  * Lots of free sprite space

## Info

Evercore uses a completely reworked level system: Rather than simply loading rooms using coordinates, Evercore uses a table of level strings, each containing the coordinates, size, and name of the level.

## Level Table

The level table uses the following format:

`[id]="[x],[y],[w],[h],[title]"`

The four coordinate variables are stored in **room units**: 16\*16 chunks of the PICO-8 map. For more precise room sizing and placement decimal numbers are supported.
The `[id]` can be omitted and PICO-8 will automatically assign the string to the lowest positive unoccupied index (except zero).
These values are unpacked into the lvl_ variables when load_level() is called.

## Mapdata Table

To load a level from a mapdata string, first set up a string in the levels table at position (0,0) using the same size as the level you're trying to load.
Then, paste the mapdata string into the mapdata table at the same index as the string you added to the levels table.

For example, if your mapdata string was at index 4, you'd put `[4]="000000000000000000000000...",` into the mapdata table.

## Documentation

#### Functions

  * vector(x, y) - Returns a table with the format {x=[x], y=[y]}
  * rectangle(x, y, w, h) - Returns a table with the format {x=[x], y=[y], w=[w], h=[h]}

  * load_level(id) - Load a level from the string stored at index [id] of the levels table
  * next_level() - Load a level from the string stored at index [lvl_id + 1] of the levels table
  
  * get_mapdata(x, y, w, h) - Grab a mapdata string at **tile coordinates** [x], [y] with a **tile size** of [w], [h]
  
  * move_camera(obj) - Attempt to move the camera towards object [obj]
  
  * init_object(type, x, y, tile)- Creates a new object of type [type] at postition [x], [y], with sprite [tile].
  * obj.init_smoke(ox, oy) - Spawns a smoke object at [obj] with an offset of [ox], [oy]
  
#### Globals

  * levels - A table containing the coordinates of every level that can be loaded
  * mapdata - A table containing the mapdata strings to be loaded by specific levels

  * lvl_x - The X position of the currently loaded level, in tiles
  * lvl_y - The Y position of the currently loaded level, in tiles
  * lvl_w - The width of the currently loaded level, in tiles
  * lvl_h - The height of the currently loaded level, in tiles
  
  * lvl_pw - The width of the currently loaded level, in pixels
  * lvl_ph - The height of the currently loaded level, in pixels
  
  * lvl_title - The title of the currently loaded level, can be nil if not set in the level string
  
  * cam_x - The x position of the center of the camera
  * cam_y - The y position of the center of the camera
  
  * cam_spdx - The current x velocity of the camera
  * cam_spdy - The current y velocity of the camera
  
  *cam_gain - How quickly the camera should center on the player
  
#### Object Variables

  * type - The object type table to use as a base
  
  * collideable - Whether other objects can collide with this object
  * collides - Whether this object collides with solids and semisolids
  * solid_obj - Whether this object is marked as a "solid" and will push and carry objects that collide with it
  * semisolid_obj - Whether this object is marked as a "semisolid" and will carry objects on top of it
  
  * spr - The current sprite of the object
  * flip - A vector containing booleans. Used to determine if the object's sprite should be flipped in the set directions
  
  * x - The object's current X position
  * y - The object's current Y position
  * spd - A vector containing decimal numbers. Stores the object's current speed
  * rem - A vector containing decimal numbers. Stores the remainder of the player's postion
  
  * hitbox - A rectangle containing integers. Used to calculate the position and size of the object's hitbox
  
  * fruit_id - The berry index position of the object. Only generated if the object type has fruit_check set to true
  
#### Object Type Variables

  * layer - Which layer to draw the object to
  * fruit_check - Whether the object should check if it's berry index position is stored in the got_fruit table before being initialized.