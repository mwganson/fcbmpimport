# fcbmpimport
FreeCAD Macro for importing black and white bmp images as FreeCAD objects.

<h3>Installation</h3>

To install place FCBmpImport.FCMacro.py in your macros folder in FreeCAD.  If you don't know the location of the macros folder, go to the Macros menu, select Macros... and you will find at the bottom of the dialog your user macros location.  You may also simply create a new macro using the Macro -> Macros.. -> Create button, name the new file FCBmpImport.FCMacro.py, and copy/paste the macro code from FCBmpImport.FCMacro.py into your new file.  Don't forget to save the changes before exiting FreeCAD.

<h3>Limitations</h3>

The only image file format supported is Black and White (1 bit per pixel) Windows BMP.

While the macro does not impose any size/complexity limitations, there are practical limitations in what FreeCAD is able to handle in terms of the number and types of FreeCAD objects in the same document.  FreeCAD was not designed for hundreds, or thousands, or even 10's of thousands of objects, which can easily be produced when importing images even of modest resolution.  The macro is best used with relatively simple images, such as cartoon type drawings, clipart, etc., rather than more complex images, such as photographs.

<h3>Usage Instructions</h3>

<h4>Screenshot</h4>

<img src= "screenshot.png" alt="screenshot">

<h4>Image Preview Panel</h4>

In the upper right corner of the user interface you will find the image preview panel.  While it is not, strictly speaking, necessary to first preview the image before importing it, you will find it is often useful to do so.  When an image is being previewed a red and green axis cross, very similar to the one used in FreeCAD to mark the origin, will be superimposed over the image.  This axis cross marks the position the objects created to represent the image will be placed relative to the origin.  Other features of the image preview panel include the ability to zoom in/out with the mouse scrollwheel.  If the zoom level is too high for a particular image to fit entirely within the image preview panel, the image can be dragged around inside the image preview panel with the left mouse button.

<img src="screenshot2.png" alt="butterfly-preview">

In this screenshot image you will notice we have an image of a butterfly being previewed.  Above the image preview panel there is some information about the currently previewed image.  When hovering your mouse over the displayed information a tool tip will appear giving the name of the file currently being previewed.  In this example, the following information is displayed about the image:

Image Preview (200x137), px,py = 210.53,163.04 Zoom = 1.8953x

The (200x137) tells us the image resolution is 200x137 pixels (including white background pixels).  Notice the image preview panel has a gray background in order to contrast with the white background for this image.  px,py = 210.53,163.04 means that at this particular zoom (1.8953x) the image preview panel can (but not always will) contain 210.53x163.04 pixels.  This can be useful information when scaling an image (read more about scaling below) so that it will be represented in FreeCAD with a FreeCAD object of the desired size.  If you press Shift while moving the mouse scrollwheel vertical grid lines will be displayed to aid in counting the number of pixels between 2 points on the image.  This will also cause the zoom value to be one such that px will be equal to an integer value.  Thus by alternating between dragging the image around inside the image preview panel and Shift+scrollwheel movements one can measure (using the px value) distances (in pixels) between various points on the image.  Ctrl+scrollwheel movements can be used to help measure vertical distances.  Ctrl+Shift+scrollwheel movements will behave the same as Shift+scrollwheel except no grid lines will be shown.

<h4>Various Options</h4>

<h5>Scale Factor</h5>

Below the image preview panel is a group of line edit widgets that can be used to set various options and preferences, including scaling and offsets.  The scale factor edit box is used to set the scale of the FreeCAD object created to represent the image in FreeCAD.  By default, each pixel is 1mm x 1mm x 1mm for solid, extruded, and mesh import types, and 1mm x 1mm for wire, face, and sketch import types.  The value entered as the scale factor will be multiplied against those default 1mm values (except for part height), and used to size the object(s).  As an example, if you wish each pixel to be 3.5mm x 3.5mm x 3.5mm you would simply enter 3.5 into the scale factor edit box (or 3,5 depending on your locale) and 3.5 in the part height edit box. (Note: part height is only applicable to mesh, solid, and extruded import types.)

But usually the desired scaling will be for the entire image or for some subset of that image rather than per pixel.  For example, you might wish to engrave this butterfly into a 6 inch x 6 inch plaque.  Assuming you are content with the white background as a border or margin, the way to go about scaling this particular image would be to enter this into the scale factor edit box:

(6 * inches) / width

Notice a few things here.  First of all, we can enter mathematical expressions into these edit boxes.  Secondly, we can access some useful mathematical constants (the constant value "inches" = 25.4).  Thirdly, we can dymanically access some global variables being kept in macro memory, in this case width, which refers to the width of the currently previewed image.  Upon pressing Enter (or leaving the box) the above "(6 * inches) / width" gets replaced with 0.762, which is the numerical evaluation of that expression.  Note: if you select a different size image to preview after doing this, the value in the scale factor edit box DOES NOT CHANGE.  There is no permanent reference to "width" being created.  The current value for "width" is used, and then immediately discarded.  It is equivalent to simply entering 0.762 into the box in the first place.

The following constant values are accessible within the Various Options edit boxes:
<ul>
  <li>pi - the constant defined in python as math.pi, 3.14159265359</li>
  <li>e - Euler's constant, math.e, the base of the natural logarithms, 2.71828182846</li>
  <li>phi (alias golden, alias golden_ratio) - 1.6180339887</li>
  <li>inch (alias inches) - the number of millimeters in an inch = 25.4</li>
  <li>thou - the number of millimeters in a thousandth of an inch = .0254</li>
</ul>

The following reference values can be accessed within the Various Options edit boxes:
<ul>
  <li>width (alias w) - width (in pixels) of currently previewed image</li>
  <li>height (alias h) - height (in pixels) of currently previewed image</li>
  <li>vx - width (in pixels) of the image preview panel</li>
  <li>vy - height (in pixels) of the image preview panel</li>
  <li>px - the number of horizontal pixels that will fit in the image preview panel at the current zoom level</li>
  <li>py - the number of vertical pixels that will fit in the image preview panel at the current zoom level</li>
  <li>zoom - the current zoom level, e.g. 1.89525 in the above screenshot</li>
  <li>x - current value contained in the X Offset box</li>
  <li>y - current value contained in the Y Offset box</li>
  <li>z - current value contained in the Z Offset box</li>
  <li>part (alias part_height) - current value contained in the Part Height box</li>
  <li>cheat (alias cheat_factor) - current value contained in the Cheat Factor box</li>
  <li>scale (alias scale_factor) - current value contained in the Scale Factor box</li>
</ul>

The following mathematical functions can be called from within the Various Options edit boxes:
<ul>
  <li>cos - cosine - math.cos()</li>
  <li>acos - arc cosine - math.acos()</li>
  <li>sin - sine - math.sin()</li>
  <li>asin - arc sine - math.asin()</li>
  <li>tan - tangent - math.tan()</li>
  <li>atan - arc tangent - math.atan()</li>
  <li>log - natural logarithm to base e - math.log()</li>
  <li>tlog - base 10 logarithm - math.log10()</li>
</ul>

Note: a special syntax is required for calling the mathematical functions.  Only immediate values (numbers) can be used as parameters for these functions (none of the above constants are references can be used as parameters to the math functions).  The format to be used is best illustrated by giving a few examples:

log32 -> returns value from call to math.log(32) = 3.4657359028, the natural logarithm of 32.
tlog17p52 -> returns value from call to math.log10(17.52) = 1.24353410183, notice the p is a stand in for the decimal point (or comma)
cos177p25d -> returns value from call to math.cos(177.25 * math.pi / 180.0) = -0.998848386485, the d denotes 177.25 is in degrees.
tan41r -> returns value from call to math.tan(41) = 0.160656698681, the r denotes 41 is in radians.
asin72 -> returns value from call to math.asin(72) = 0 because this is an error (produces a complex number 1.5708 - 4.96977 * i)
sin13 -> returns value from call to math.sin(13) = 0.420167036827 (notice no r or d at the end defaults to radians)

Each time a value or expression is entered into an edit box, that value or expression is appended to the associated label's tool tip.  Thus, by mouse hovering over the associated label one can view the history of values previously entered during this session.

<h5>Setting up the X, Y, and Z offset values</h5>

You will notice in the above screenshot there is a red and green axis cross centered between the two butterfly antennae.  This axis cross marks the location the FreeCAD objects created to represent this image will be positioned relative to the origin at (0,0,0) in 3d space, or (0,0) in 2d space if importing as a sketch.  I positioned this by entering "-w/2" in the X Offset box and "-h * 8/10" in the Y Offset box.  (Recall, w and h refer to the image width and height, respectively.)  By default, the axis cross is set to the lower left corner of the image.  To move it to the right, enter a negative value in the X Offset.  To move it up, enter a negative value in the Y Offset.  Actually, what's happening is the image is moving left and down, then getting re-centered as the image preview is updated.  This is why negative numbers are needed.

<h5>Scaling</h5>
The easiest way to scale the FreeCAD object to be created to represent the image is to enter the desired final size (in mm) into the scale factor box, and then divide that number by horizontal size (width) of the image (in pixels).  I gave the previous example of 6 inches above.  If we wish to scale the butterfly object such that it is 6 inches, we can enter this into the scale factor box:

(6 * inch) / width

But suppose we wanted this in millimeters.  Suppose we want 300 mm as the final width:

300 / width

<img src="screenshot3.png" alt="zoomed butterfly">

In the above image we have zoomed in on the butterfly so that the antennae fit exactly into the image preview panel.  With px = 43.0 we know the horizontal distance between the antennae tips is 43 pixels.  We can also count the vertical lines to determine / confirm this.  Notice there are blue lines, yellow lines, and dark red lines.  Each dark red line is 5 pixels from the next yellow line.  The blue lines are each one pixel apart from the next line, whatever the next line's color is.  Yellow lines are 10 pixels apart from each other, as are the dark red lines.  Beginning on the left side, notice the first dark red line is 1 pixel to the right of the left antenna.  By counting the dark red lines we see there are 4 of them, plus 2 additional blue lines to the other antenna.  Thus 1 + 40 + 2 = 43.  If we wanted to scale the image such that the distance between the antennae was 100mm we could enter:

100 / px 

into the scale factor box to achieve this result.  Or, alternatively:

100 / 43


