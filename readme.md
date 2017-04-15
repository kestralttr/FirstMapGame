# Martian Attack

[Play here](https://kestralttr.github.io/MartianAttack/)

After some experimentation with importing custom maps into LeafletJS, I was intrigued by the possibility of simulating a world other than Earth and allowing users to interact with it.

After some concepting, I decided to build a simple game that allowed for basic movement and combat around a fully scalable custom map.

## Creating the Map

I used a Photoshop script called the [Google Maps Tile Cutter](https://github.com/bramus/photoshop-google-maps-tile-cutter) that allowed me to process my image into the correct tiles and folder structure so that LeafletJS could properly use it.

After the files were generated and properly placed, generating a map in LeafletJS immediately allowed for full zooming and scrolling.

One detail I had to be aware of was the need to convert the map object into a pixel format, since the default measurements are performed in latitude and longitude.  This assumes that you want the area at the poles of your image to behave like they are modeled on a globe, but when working with a static, 2D image, the latitude and longitude measurements caused unwanted distortion.

Thankfully, LeafletJS gives us a method called .unproject(), which set coordinates via pixels and removes any compensation for globe curvature.  I used .unproject() to set the position adjustments for the map and all markers, which kept my entire code pixel-friendly.

## Game Design

I set out to design a game that was simplistic, but fun enough to try a couple of times.

I settled on a standard battle against aliens.  The user would control a tank that moves slightly slower than the multiple aliens trying to find it.  The tank however, would have more health, deal more damage, and have the ability to repair itself.  But in a fight against more than one alien, the tank would almost always lose, which would prompt the user to exercise a little strategy in how they played the game.

In that vein, I decided to make the game progress each time the user performed an action.  The user can choose to move in a direction, repair, or attack the closest alien within firing distance (which is set at 500 pixels around the tank).

In this fashion, by ensuring that essential game functions were ran every time a button is clicked, I was able to build a system that progressed every time a user selected an option.

By creating Javascript objects to represent the health, position, damage, icons, and opacity of the markers, I could update those values and thereby simulate actions taking place on my map.

## Alien AI

The aliens on the map are spawned in random positions at the start of each game, and at each step of the game they move closer to the user's tank, firing continuously once it is within range.

By utilizing a simple compass object, at every logic step each alien determines which direction will bring it closest to the tank's position and then moves in that direction.

This mechanic makes use of LeafletJS' .distanceTo() method, which seems to rely on latitude/longitude-based computation of distance.  I first sought to correct this, but after noticing that the aliens had both slightly erratic movement and apparent momentum preservation under this system, I decided to keep it in this format because it resulted in less predictable gameplay.

## HUD Design

I wanted to emulate some sort of retro, sci-fi monitor.  By placing the map object over a blue-on-black grid, I was able to generate the effect of it hovering on a screen.

I kept the buttons and readouts brightly colored so that the user would instantly know where to interact with the game, and updated them with simple game logic functions.

## Future Plans

I have a couple of ideas to make this game more immersive.

First, I'd like to explore reassigning the icon images every time either a tank or alien moves around the map.  This would allow me to create the illusion of movement, since each icon would automatically face the direction it was traveling.  By cutting out various angles of each icon in Photoshop and reassigning them in the game logic functions, this would help add realism to the gameplay.

I also debated binding keys to the game, since currently the only way to interact is by clicking the buttons.  My only concern is how I would make the key assignment intuitive enough so that people would know what to click in order to travel diagonally.  I'll continue to think about this feature and add it if I find a satisfactory solution.

I'd finally like to add more fun effects to the gameplay.  For instance, when the player takes damage, causing the display to shake or appear to shimmer would be a nice addition.  As I think of ways to implement these effects, I'll be sure to add them.
