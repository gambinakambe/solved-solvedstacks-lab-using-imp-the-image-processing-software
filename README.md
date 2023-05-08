Download Link: https://assignmentchef.com/product/solved-solvedstacks-lab-using-imp-the-image-processing-software
<br>
StartDownload Imp.java and create a new project with the stubbed code.About the SoftwareImp is the shell of a image processing application. It will allow you to open an image, .jpg or (I think, but don’t really remember) .gif file.Once an image is open it breaks down each pixel of the image into first a single dimensional array, and then converts that into a 2D array of pixels.

Each pixel in the 2D array, which is called “pixels” in the array is an integer value, a large integer value that is of no use to you as just an int. Embedded into each integer value is a one byte value for red, green, blue and a alpha channel (transparency). In order to get the values of a pixel at some random location, let’s say X=100, Y=200, you would request an int from the array:

int currentPixel = picture[Y][X]; //notice Y is first, Y is the height of the image, X is the width.

Then in order to breakdown that pixel to it’s red, green, blue, alpha channel you need to set up an array and send that int to getPixelArray method like this:

int rgbArray[] = new int[4];rgbArray = getPixelArray(currentPixel);

Now the rgbArray has the value for Red in rgbArray[1], Green in rgbArray[2], and Blue in rgbArray[2]. rgbArray[0] is transparency, don’t mess with this for now, it should always come back as 255. If you switch it to 0 your pixel becomes transparent.Here is how the integer gets broken down to 4 bytes, it’s bit twiddling:

private int [] getPixelArray(int pixel){int temp[] = new int[4];temp[0] = (pixel 24) &amp; 0xff;temp[1] = (pixel 16) &amp; 0xff;temp[2] = (pixel 8) &amp; 0xff;temp[3] = (pixel ) &amp; 0xff;return temp;

}

Then you get a rgbArray back from this method, do tests, manipulate based on values, and when you’re ready to put it back to an int you can put back into an array that can be drawn to the screen you call this method:

private int getPixels(int rgb[]){int alpha = 0;int rgba = (rgb[0] &lt;&lt; 24) | (rgb[1] &lt;&lt;16) | (rgb[2] &lt;&lt; 8) | rgb[3];return rgba;}This method puts the array of 4 bytes back to a single integer value.

Once you get your image array completely updated call:

resetPicture();

This redraws the picture to the screen.

The fun1 method in the code is an example of how to take out all red in the image.AssignmentNow for this assignment, you will open an image, use this one:

For a start use the image above, then build your own with Paint or Gimp and test it out.

Pick a pixel, don’t pick the background, pick a shape.When you use the mouse and pick a pixel on the screen, it will print out the x, y coordinate and the color value at that pixel in the console window. I already wrote this part of the lab, the mouseListener won’t work until you have a picture open. The Start button won’t be active until you click on the picture at some location.The following code is from the method that takes the coordinates of the mouse click and prints out the results.

int pix = picture[colorY][colorX];int temp[] = getPixelArray(pix);System.out.println(“Color value ” + temp[0] + ” ” + temp[1] + ” “+ temp[2] + ” ” + temp[3]);

The area you click will give you a colorY and colorX coordinate where you start your lab. colorX and colorY are global instance fields you can use as a starting point in your fun2 method.After you click the mouse the start button will become active, clickable.When the button is clicked it calls a stub method in the code called public void fun2() that is where you will write your stack region grow.

To DoIn fun2 create a stack and in the stack you will hold another class you create, call it PixelRegion, inside the PixelRegion class you will have the X, Y coordinate and what adjacent pixels you have checked so far.Start at colorX, colorY and get the color, build a instance of your PixelRegion class with the information.Check the pixel at ColorX-1, ColorY-1 if it’s within a certain threshhold of the color you picked build another PixelRegion class with that pixel and put it on top of the stack.Change each pixel within the threshhold to some color such as black or white.Check all 8 surounding pixels of all pixels put on the stack, ignoring pixels that are not within the threshold and adding pixels that are within the threshhold to the top of the stack (Making that pixel the next one you start with).Only check the pixel on top of the stack, pixels do not get removed from the stack until all the pixels around it (8) have been checked, once all surrounding pixels have been checked you pop that pixel off the stack and then check the next pixel on top of the stack.The major part of the fun2 method is finished once the stack is empty, then you do your redraw (If you want to get fancy you can attempt to do an update draw when you pop a pixel off the stack but there might be timing issues, drawing is only done when the processor is idle, you might have to force the drawing with a yield() call, which would be little more tricky).Scenerio: Start at redpixel and change it to black, check the pixel #8, it is within the threshhold so it goes on top of the stack, change it to black and you use it next. Next check pixel A in the picture, it’s not within the threshhold so you check the next one surrounding pixel 8 because pixel8 is still on top of the stack, depending on whether you are traversing counterclockwise(Below pixel A next) or clockwise (to the right of pixel A next) that would be your next selection.

So if the first pixel (red pixel) checks the pixel at position 8 and it meets the threshhold it would put pixel position8 on top of the stack and start checking that one (and possibly many more pixels would get put on the stack, but eventually you’d get back to position8 on top of the stack), once position8 is finished it gets popped off the stack and the top of the stack would go back to the first pixel. At that point you already checked position 8 so now you need to check position 7, meaning you would need to keep track in the PixelRegion class which of the adjacent pixels you have checked, and check them in a clockwise fashion (or counterclockwise).When you have put a pixel such as postion8 on the stack there’s a good chance you already checked position 7 pixels as an adjacent pixel to postion 8, so you won’t need to worry about it in the start pixel. When you change the value to black or white it should move it out of the threshhold so you will check it and ignore it anyway.

To Turn InThe source code will be due: March 29th, 6pm.