# CS-355-Lab-1-Simple-Drawing-solution

Download Here: [CS 355 Lab #1: Simple Drawing solution](https://jarviscodinghub.com/assignment/lab-1-simple-drawing-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

In this lab you will implement a simple interactive 2-D drawing program. This program is very basic—it
includes only simple placement of the shapes without further interaction. You will extend it further in the
next couple of labs.
The program must the overall model-view-controller structure:
• The model stores the shapes that have been drawn independent of the interactions used to specify the
shapes or the API used to drawn them on the screen.
• The view component renders these accordingly using the Java 2D Graphics API.
• The controller handles the input events and make calls to the model to add new shapes or update them
as they change. It should also maintain all state information for the interface (selected drawing tool,
selected color, etc.).
We will give you a shell that builds the basic GUI and provides a model-view-controller framework.
You will need to provide all of the functionality for handling actions within the GUI. In particular, you are
responsible for all interaction and drawing within the primary drawing area.
User Interface
The program should operate in a single window and include user-selectable menu options, a palette of userselectable drawing tools, an area indicating the current color, and a primary drawing area. The primary
drawing area should be at least 512 × 512. The shell program will build the menus, tools palette, and other
GUI elements. Handling these are your responsibility. If you wish to, you are welcome to extend this shell
to improve the interface, but that’s not the aim of the program. Don’t waste a lot of time making it look
pretty—focus on the drawing portion.
Color Selection
Clicking on the button for the color selector should bring up the Java color selector. (See the shell program
to see how this is done.) Once it returns to your program the selected color, you should update the current
color indicator to show this color. All future drawing should be done using that color until the user selects a
new one.
1
Figure 1: Lab #1 screen shot
Drawing Functions
Your program should implement drawing the following shapes. Each shape should be drawn (lines only) or
filled with the currently selected color. When each shape is drawn, your program should add that shape to
the stored model such that the most recent shape is drawn on top of the preceding ones. (You might find it
simplest to add the shape to the model as soon as the drawing starts and then update it during the drawing,
or you may add it once the user finishes drawing it.)
During the drawing you will need to refresh the display. This will also be necessary when Java’s GUI
system indicates that the area has been damaged and needs to be redrawn. The shell program provides
the necessary functionality for double buffering: you’ll draw to a new offscreen buffer and then update the
drawing area with the contents of this buffer.
Lines
When the line tool is selected, a mouse-down event places one endpoint of a line. As the mouse is held down
and dragged, you should show the line between the starting point and the current mouse position. Releasing
the mouse button places the other end of the line.
Rectangles
2
When the rectangle tool is selected, a mouse down places one corner of the rectangle (either upper left or
lower right). As the mouse is held down and dragged, the current mouse position is used as the opposite
corner. Releasing the mouse button places the other end of the opposite corner.
Squares
Interaction is the same as for placing rectangles, but the sides of the square are constrained to be the same
length, which should be the smaller side of the rectangle indicated by the initial mouse click and the point
where the mouse button is released.
Ellipses
Same interaction as for rectangles, except that an ellipse should be drawn such that it exactly fills this
bounding box.
Circles
Same interaction as for squares, but a circle should be placed at the center of this bounding box (the equivalent square) such that it fills the square.
Triangles
The method for placing a triangle is different from the usual click-and-drag placement of the other shapes
since it involves three points. The first click should place the first corner, the second click should place the
second corner, and the third click should place the third corner and close off the triangle. After the third
point is placed, the triangle should be added to the model and drawn.
Model
Your model should conceptually store an ordered list of shapes in back-to-front order. It should provide
methods that allow the view/controller to add new shapes and to query the model and retrieve the shapes in
order.
The Shape Class
You should have an abstract Shape class that for now stores only the shape’s color. You should have accessor
methods for the color. Each shape is stored with the following information, each of which should also have
accessor methods.
Lines
The Line class should store the two endpoints for the line.
Rectangles
The Rectangle class should store the location of the upper left corner, the height, and the width.
3
Squares
The Square class should store the location of the upper left corner and the size of the square.
Ellipses
The Ellipse class should store the center of the ellipse, the height, and the width.
Circles
The Circle class should store the center of the circle and its radius.
Triangles
The Triangle class should store the location of each of the three corner points.
Notes
The ways you represent the shapes in the model are not the same as the ways you draw the different shapes,
nor are they the same as the parameters used to draw the shape using Java’s 2D Graphics API. This is
intentional—the model should be as general as possible, independent of the choice of interface used by the
controller or the means of drawing used by the viewer. These representations will change in future labs.
Program Shell
Our program shell handles the basic GUI elements and lets you focus on event handling and object drawing.
It also provides a basic MVC architecture. You will write and plug in your own classes that implement the
controller and the viewer. The shell knows nothing about your model, which you should create separately.
Your controller and viewer then interact with your model.
First, download and unzip the provided src.zip file. This will create a source directory with two
subdirectories:
• cs355: the source files we provide — these will not need to be modified
• lab1: contains only a stub CS355.java main file
When you open up CS355.java you’ll see a line like this:
GUIFunctions.createCS355Frame(null,null,null,null);
This line builds the Frame through which the program operates. The four nulls are placeholders for classes
you will pass to it. This is where you plug in the parts of your viewer and controller:
• A class that implements the CS355Controller interface — this is the main part of your controller
• A class that implements the ViewRefresher interface – this is the viewer and is responsible for all
drawing
• A MouseListener
• A MouseMotionListener
4
The Controller
This controller takes the form of a CS355Controller object. The interface for this object is included
with the framework. Simply make an object that implements the CS355Controller interface, create a
new instance of it, and pass it in as the first argument in the CS355Frame constructor. By doing so, you
will be able to tell the framework how to handle events that take place, such as button presses and menu
selections.
The Screen Refresher
The framework also has a drawing area. It is up to you to control what is drawn there, but we have simplified
the process by allowing you to draw to a Graphics2D object instead of requiring you to figure out how to
manage buffer strategies and redraw images yourself. You do this by creating a ViewRefresher and
passing it as the second argument into the framework constructor.
Each time it redraws the screen, the framework will create a Graphics2D object that it then passed
to the ViewRefresher you provide it. After the ViewRefresher has had a chance to draw on the
Graphics2D, it displays the Graphics2D onto the canvas. Simply create a new object that implements
ViewRefresher and pass a new instance of it to the constructor of the framework. (This is very much
like the strategy design patter you have learned about/will learn about in CS 360). The ViewRefresher
you implement should query the model and use what it learns to draw on the Graphics2D.
The Mouse Listener
In addition to processing button events and displaying your model, you must also be able to respond to
mouse-clicks on the canvas. The framework will attach a MouseListener, which you provide, to the
canvas. We dont provide an interface for this; a perfectly good one already exists in the default Java libraries.
Just create an object that implements java.awt.event.MouseListener and pass it in as the third
argument, and your listener will be notified at the appropriate times whenever the canvas is clicked. Don’t
forget that you can get more specifics about the nature of the click (such as its position or which mouse
buttons were involved) by using the methods of the MouseEvent object provided by the interface.
The Mouse Motion Listener
Since the program involves dragging the mouse while the button is pressed, you also need to attach a
MouseMotionListener. Create an object that implements java.awt.event.MouseMotionListener
and pass it in as the fourth argument, and your listener will be notified at the appropriate times whenever the
mouse is moved.
Implementation Notes
You are welcome to implement your own framework if you wish, as long as it has the same functionality.
We give you this framework in an attempt to make your life easier, not to restrict you in any way.
There are many ways to handle updating the shape continuously while it is being drawn. For lines, you
might find it easiest to store one endpoint for the line where the mouse-down event occurs, then immediately
place the other endpoint at the same place (essentially adding a 0-length line to the model). Subsequent
5
mouse-drag events would result in updating the second endpoint and initiating a screen refresh. Similar
strategies can be used for the other shapes.

