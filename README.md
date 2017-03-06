# hackenstein3D
Raycasting game for the Hack computer from Elements of Computing Systems / Coursera NAND2Tetris

The Hack Computer is a very simple 16-bit computer with no built-in multiply, divide, bit-shift or floating point as designed an implemented in the book Elements of Computing Systems by Noam Nissan and Shimon Schoken. http://nand2tetris.org

It is written in Jack, a Java-like language also implemented in the book. And designed to run on the VMEmulator provided by the website.

This is a short demo video.
[hackenstein3D](https://youtu.be/inFJ5EyOhpM)


HACKENSTEIN 3D - Escape from Castle Hackenstein, Part II
--------------------------------------------------------

ABOUT

Hackenstein is an attempt to create a Castle Wolfenstein type experience
on the Hack computer. The limitations of the Hack machine meant that
several compromises were necessary! There are no Nazis for a start.

The most technical part of the program is the method of ray casting
to work out what is visible at a given location on the screen. The
algorithm here is purely 16-bit integer and minimises the number of
divisions, and multiplications of large numbers. All of the trigonometric
functions are provided via lookup tables. Most maths is done in 1/64ths
to approximate floating point. I've used every trick I know
to speed things up and get it working, so I hope you enjoy reading
the code and working out how it was done.

The program runs as fast as I can make it - the rendering to the screen
on the VM Emulator is the main bottleneck. There are also a couple of
minor visual glitches since it isn't practical to work around all possible
integer overflows, but hopefully it won't spoil the experience too much.

OBJECT OF THE GAME

The object of the game is to find and destroy 3 targets painted on the
walls and find the door to escape. You only have a small amount of time
to achieve your goal.

GAME INSTRUCTIONS

Use the Arrow Keys to move around. And the Space Bar to shoot.
To quit the game, press Q.

You need to destroy the 3 targets and find the exit to win the game.

You have 1000 "actions" to do this. Moving and turning take 2 actions
each; shooting takes 4.

The eagle flags painted on the room are there to help you navigate.


CODE OUTLINE

Main.jack
	Controls user interaction and manages the game mechanics
	The main function handles everything from initialisation,
	user interaction, scores, initiating movement, and initiating
	rendering.

Player.jack
	Manages movement, prevents walking through walls, and caches
	some of the trigonometric calculations.
	Player is not an object class. It is a singleton module which
	operates using statics and functions. This is not ideal OOP, but
	it avoids unnecessary indirection that would clog the stack and
	slow down the game even more.

Walls.jack
	Calculates the ray casting to identify what walls are visible,
	what they look like and how far away they are.
	It also keeps the details of the maze.
	Again it is a singleton module, not an object class, for the same
	reasons. Also notable is that instead of passing the data for
	the display via an array, it receives a memory address and casts
	it to an array. It's analagous to C pointers and carries the
	same dangers.

Display.jack
	Performs the screen rendering of the walls (using texture maps),
	the scores (using the OS Output library), the gun/sights (using
	bitmaps). All graphics is done by writing whole 16-bit words to
	the screen. Bitmaps are provided in words, while walls are
	rendered in 16-bit strips. Note that the bitmaps are provided
	via the same Array casting trick.
	
RUNNING THE GAME

Run with the VMEmulator using the built-in OS library with animation
turned OFF and speed set to FAST.
