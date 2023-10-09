# ogf game framework
this is an SDL2/Box2D framework. it has **a lot** of features to mess around with, that will assist you when making games in C++.

# what do i need to install?
SDL2
Box2D
SDL_ttf.h

# features
the following list is a list of all the features this header has.

# button class
a button class, so you don't have to make one
**constructor:**

``button(x, y, width, height)``
position and dimentions of the button

**functions:**

``press(mouse x, mouse y, clicking)``
check if the button is being pressed.

**variables:**

``x,y,w,h``

positions and dimentions of the button

``pressed``

if the button is being pressed, this variable is being set by the ``press()`` function

# Ease class
An ease class, with 30 diffrent ease types.

**note:** 
the ease starts when it gets defined.
**constructor:**

``Ease(duration, from, to, mode)``

how much time the ease takes to finish, the starting value, the ending value and the mode.

**functions:** 

``Get_Ease()``

get the value.

``reset()``

reset the time and values.

**variables:**

``elapsed`` 

time since last reset or start.

``done``

if the ease is done.

**modes:**

```
0 SineIn
1 SineOut
2 SineInOut
3 CubicIn
3 CubicOut 
4 CubicInOut
5 QuintIn 
6 QuintOut
7 QuintInOut
8 CircIn
9 CircOut
10 CircInOut
11 ElasticIn
12 ElasticOut
13 ElasticInOut 
14 QuadIn
16 QuadOut 
17 QuadInOut
18 QuartIn 
19 QuartOut
20 QuartInOut
21 ExpoIn 
22 ExpoOut
23 ExpoInOut 
24 BackIn 
25 BackOut
26 BackInOut
27 BounceOut
28 BounceIn
29 BounceInOut
```


# Timer class

a simple timer based on real time, it begins when you define one.
it is in seconds.

**functions:**

``Reset()``

reset the time.


``Get_Time()``

get the time since last reset or start.

# animation class

an animation that can be drawn.

**define this as a pointer.**


**constructor:**

``animation(file, row count, coloumn count, sheet width, sheet height, duration)``
the sprite sheet file of the animation as a bmp, the amount of rows and coloumns,
used to calculate frame positions and amount, the width/height of the sprite sheet, the duration of the
animation in seconds.


# camera class

a camera used for physics objects and sprites.
**constructor:**

``camera(x, y, window w, window h)``

start position of the camnera, and window dimentions.
there is no reason to define a ``camera`` yourself, the ``Window`` and ``Obj`` class uses them.

**variables:**

``zoom``

used to zoom in on the window.

//----------------------------------------
//           namespace Physics           |
//----------------------------------------

# type class
an object type, used to define physics objects.

**constructor:**

``type(sprite, collision type, width, height, body type, mass)``
the default sprite of the object, it will play when the object is not playing an animation.
the collision shape of the object. (``CIRCLE_SHAPE``, ``RECT_TYPE``, or ``POLYGON_TYPE``).
the standard body dimentions
the body type. ``b2_dynamicBody`` if you want your object to move, otherwise use ``b2_staticBody``.
the mass of the object, prefferably 40.

**variables:**

``points``
if you define your object as ``POLYGON_TYPE``, you will need to set this variable first. its a b2Vec2 vector.

# raycast class

this is the output of the Obj raycast function.

**variables:**

``typesHit``
a vector of ``type*`` with all the types the raycast hit.

``hit`` if the ray hit the object

``normal`` the normal angle of the ray.

``dist`` the distance at hit

``x``,``y`` the hit point of the ray

``length``,``angle`` the length and angle of the ray

# Obj class

a physics object.

**variables:**

``Type`` the type* of the object.

``c_type`` the collisions type of the ``type``

``DoGravity`` if the object is affected by gravity

``DoRotation`` if the object gets rotated automatically by forces

``SleepWhenOutOfView`` sleep the object when it exits the window ``ViewBufferPixels`` away

``ViewBufferPixels``     ^^^

``w``,``h`` do not set these variables, they are the width and height of the object, 
even if the object is a circle or a polygon, 
its used to draw the sprite, and do all sorts of other important stuff. ``w`` also is the radius if the object is circlular.

``mass`` the mass of the object

``xVel``,``yVel`` the velocity of the object

``DisplayOrder`` the display order of the object when rendering

``flip`` and ``SDL_RendererFlip`` variable that flips the sprite when drawing

``col_body`` a ``b2Body*, bool`` unordered_map of all the objects colliding this frame

``col_type`` this is ``col_body`` but using type* instead.

``body`` a ``b2Body*`` you can use for Box2D related functions, or ``col_body``

``rend`` the renderer of the window this world is attached to, used for drawing 
raycasts and proximity

``sprite`` when an animation is not playing, this sprite will show instead.

``anim`` an ``animation*`` variable, when this is ``nullptr``, ``sprite`` will be 
drawn, otherwise, this animation.



**functions:**

``Get_Angle()``

get the angle of the object **in radians**



``Set_Angle(double angle)``

set the angle of the object **in degrees**



``Set_Renderer(renderer, camera)``

set the renderer and the camera of the window to draw raycasts and proximity



``impulse(x, y)`` 

impuslse the object, or in other words make it jump.



``Angluar_Impulse(force, angle)`` 

impulse the object at an angle



``velocity({up, left, down, right}, vel)``

make the object move using the list above, this is to make wasd controls easier to 
make



``Get_Shape_Points()`` 

get the points of the shape as a b2Vec2 vector



``RayCast(dist, angle, object, stop at fist object, draw ray, ray color = ORANGE)``

``Raycast(x, y, object, stop at fist object, draw ray, ray color = ORANGE)``

shoot a ray, and return a ``raycast``. 



``proximity(object, radius, draw, density = 10, color = BLACK)``

create a circle, and check if the object is within the radius of that circle. change 
the density

depending on how small your objects are and how accurate you want it.



``Set_Camera(&camera)``

set the window camera to be on this object.



``Get_Pos()``

get a ``b2Vec2`` of the objects position



``Set_Pos(x, y)``

set the position of the object.



``Destroy()``

destroy the object.





# Physics::Window class

a game window, a physics world, and event handling all in one class.

**constructor:**

``Window(name, width, height, resizable)``

window title, dimentions and if the window is resizable.



**variables:**



``k_a_preseed`` bool that tells you if the key was pressed this frame.

``k_a_held`` bool that tells you if the key was held down this frame.

``k_a_released`` bool that tells you if the key is realeased this frame.

``win`` this is an ``SDL_Window*``, for SDL functions

``name``  the windows display name.

``rend`` ``SDL_Renderer*`` to the windows renderer, for functions and object raycast 
drawing

``fullscreen`` if the window is fullscreen, will automatically quit fullscreen when 
esc key is pressed.

``display_FPS`` if the window displays frame rate at the top of the window in the 
title.

``fps`` frame rate as a variable

``dt`` delta time, time since last frame

``Draw_hitboxes`` if the window draws physics hitboxes (for debugging)

``max_FPS`` framerate cap/limit

``color`` the windows background color

``clear`` if the window is gonna get cleared every frame

``paused`` if the physics are paused, for pause menu's

``world*`` the Box2D world

``event`` event gathered from the  ``Event_Loop()`` function.

``cam`` the world camera, use this to zoom in on the window.



**functions:**

``Update()``

this is the most important function in this class. it updates the window and its 
values.

add it to your game loops argument.



``Set_Icon(file)``

 set window icon (this will not really work when you export your app,
t
he icon will just be the app icon).



``Set_Gravity(horizontal, vertical)``

set the worlds gravity



``inView(object, viewBufferPixels)``

check if an object is in the window



``Spawn(type, x, y)``

spawn an object of a type at a position



``Emmit(from, type, force, angle, max distance, rotate, allow slow down)``

shoot an object of ``type``, with ``force`` and ``angle`` ``from`` another object.

the object will get destroyed at distance ``maxDistance`` if its not 0, will rotate 
at the angle it was shot at, and will not allow slowing down after its shot.



``Attach(from, type, offset x, offset y, match angle)``

attach an object to another with offset, and allow having both objects to have the 
same angle



``Get_Obj_From_Body(body)``

get an ``Obj*`` from its ``b2Body``



``Draw(x, y, color, layer)``

draw a pixel on the screen with ``color`` color, and if layer is GAME_LAYER, it will 
get affected by the camera.



``Load_Sprite(file)``

load a bmp file as an SDL_Texture



``Draw_Texture(x, y, w, h, angle, texture, sorcrect, layer)``

draw an SDL_Texture.

sorcrect is the source rectangle as an ``SDL_Rect``, meaning any pixels outside

of the rect will not be drawn.



``Draw_Sprite(x, y, width, height, angle, file, display order, layer)``

draw a sprite on the screen.



**all other Draw or Fill functions are self explainatory, exept polygons.**



``Draw_Polygon(points, color, x, y)``

draw a polygon outline.



``Fill_Polygon(points, color)``

draw a filled non convex polygon polygon



``Event_Loop()``

this function does exactly the same thing as the SDL event loop function. it uses the 
``event`` variable



# No_Physics::Window class

this function is the same thing as the Physics::Window class, but it has none of the 
physics functions and variables involved, meaning it has less lag.



# other functions

``Blend_color(color1, color2, factor)``

blend two colors together. add a factor as a third argument, and the closer it is to 
0, the closer the color is to color 1, factor can only be from 0 to 1



``Play_Sound(file)``

play an mp3 sound file.



``Angle_To(x1, y1, x2, y2)``

find the angle between two points



``Distance_To(x1, y1, x2, y2)``

find the Distance between two points



``AngleAndDistance_To(x, y, angle, distance)``

find the position at distance and angle from x and y



``Random(from, to)``

find an random int number between two numbers



``Random_Color()``

get a random SDL_Color.



``Save(content, file)``

save content to a string file called ``file``



``Load(file)``

load a string from a file named ``file``



``Clear_File(file)``

clear a file named ``file``



``bmp_To_Vector(file)``

convert a bmp image file to a 2d vector of SDL_Color



``vector_To_Bmp(data, location, name)``

basically do the exact opposite of ``bmp_To_Vector()``.
