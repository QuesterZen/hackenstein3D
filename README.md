HACKENSTEIN 3D - Escape from Castle Hackenstein, Part II
--------------------------------------------------------

ABOUT

Hackenstein 3D is a (very) simple first-person shooter /
3D maze game. It is an attempt to recreate the experience 
of Castle Wolfenstein on the Hack computer, whose many
limitations have necessitated several compromises. 
There are no Nazis, for a start!

I have posted a short demo video of the game on [youTube](https://youtu.be/inFJ5EyOhpM).

OBJECT OF THE GAME

The object of the game is to find and destroy 3 targets painted on the
walls and reach the exit. You only have a small amount of time
to achieve your goal.

GAME PLAYING INSTRUCTIONS

Use the Arrow Keys to move around. And the Space Bar to shoot.
To quit the game, press Q.

To win the game, you need to shoot all three targets and reach the
exit. You have 1000 "actions" to do this. Moving and turning take one action
each; shooting takes 4 actions but you can move at the same time without
additional cost.

The eagle flags painted on the room are provided to help you navigate.

RUNNING THE GAME

. Download the .vm files into a single directory. 
. Download the Hack VM Emulator from the NAND2tetris website: http://nand2tetris.org.
. Run the VMEmulator from a shell.
. Select 'File > Load' Program from the menu and point to the directory containing the .vm files
. Select 'View > Animate > No Animation' and set the slider to 'Fast'
. Finally press the '>>' button or F5 to run the game.

ABOUT THE PLATFORM

Hackenstein 3D runs in conjunction with the Hack OS services on the Jack virtual machine which is designed to be compatable with the specifications of the Hack computer, it was programmed in the Jack programming language.

The *Hack Computer* is a specification for a microcomputer with a very simple 16-bit CPU and 64K of built-in RAM (half of which is set aside for IO memory maps). The CPU has no built-in facility for multiply, divide, bit-shift or floating point. The Hack computer's design and implementation are detailed in the book 'Elements of Computing Systems' by Noam Nissan and Shimon Schoken and in the Coursera courses 'NAND2Tetris Part1 and Part2'. The website for the book and course is: http://nand2tetris.org

The *Hack Operating System* conforms to an API specified in the book and course and provides functionality for text output, graphical output, simple maths functions, strings and memory management.

The *Jack Virtual Machine* is a stack-based virtual machine whose specifications are also described in the book and course. The machine assumes that it is running on a Hack-compatible architecture and memory map and has available the services of the Hack Operating System. In practice, virtual machine programs are run on the VM Emulator supplied by the course authors and which can be downloaded from the course website. Other (faster) implementations are also available from students who have completed the course.

The *Jack Programming language* is a simple Java-like language whose specification and implementation are also described in the book and course. A Jack compiler is available from the website.

ABOUT THE DESIGN

The key technical components of the program are: 

*Ray casting*, which identifies what is visible 
at a given location on the screen. It operates by identifying intercept
points between a 'ray' from the player through a screen pixel and a
possible wall location using simple trigonometry. In reality, the ray-
casting is only 2D, with wall heights and textures added to give the
illusion of true 3D. The primary difficulties in ray casting on the
Hack platform involve using trigonometric functions without floating 
point numbers, and avoiding integer overflow while maintaining accuracy. 
The program achieves this by performing most calculations in 1/64ths 
as an approximation of floating point numbers. The maths functions are
pre-generated by Python scripts and imported in the form of large
lookup tables.

*Rendering*, which draws the scene with heights and textures to the
screen. Because every pixel on the screen needs to be drawn to, the
rendering loop is by far the most time critical part of the program.
Many tricks are used to avoid function calls, multiplications, divisions,
OS calls and array dereferencing. The code is very low-level and very
fast, managing to redraw the entire scene in around 0.2 seconds.

I've used pretty much every trick I know to get these two routines
as fast, smooth and efficient as I can, so I hope the game is fun
to play as well as being a demonstration of what is possible on
the Hack computer. I hope you take the time to read some of the
code and gain some enjoyment in working out how it was done.

SOURCE CODE OUTLINE

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

