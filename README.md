# hackenstein3D
Raycasting game for the Hack computer from Elements of Computing Systems / Coursera NAND2Tetris

The Hack Computer is a very simple 16-bit computer with no built-in multiply, divide, bit-shift or floating point as designed an implemented in the book Elements of Computing Systems by Noam Nissan and Shimon Schoken.

It is written in Jack, a Java-like language also implemented in the book. And designed to run on the VMEmulator provided by the website.

The raycaster uses elementary trigonometry using lookup tables. Most values are in 64ths to give an approximation of floating point maths using 16-bit integers. Multiplication and division are largely avoided by using modulo arithmetic and counters for linear interpolation.

http://nand2tetris.org
