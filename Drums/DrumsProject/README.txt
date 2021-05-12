Goal: given two arrays representing functions/sequences and a non-negative
integer n, calculate convolution n times and print a histogram that serves
as a graphic representation of the result.

First step: parsing
-------------------
The program operates using input it receives from the user, consisting
two arrays and a non-negative integer. This part of the exercise deals
with the fact that we cannot assume the validity of the user input anymore,
so we had to parse the input ourselves.

I made use of the code that was given to us on Moodle to accept input
using fgets and strtok to iterate over the elements from the input, seperated
by space/linebreak/etc. In each iteration I did the following:
1) Adjusted counter of elements we received in the input.
2) Made sure we didn't exceed 100 elements.
3) Checked if the element represented a valid double.
4) Saved the number represented by the element in my array.
If any of steps 2-4 failed I returned EXIT_FAILURE, which would subsequently
crash the program.
The check for whether the element represented a valid double is performed
by isValidNum. It goes over all the chars to see if they are either a digit
or a decimal point, and also makes sure there isn't more than one decimal
point and that the number doesn't start with a zero (unless the number
is between 1 and 0, or is just 0).

Having done that for both array inputs I compared the counters I kept
for each array, to check if the second array had more numbers than the
first, in which case I'd crash it.

Then I'd do the same for the number of rooms (n), but in a different function
because I didn't need to check for decimal points or other things that are
irrelevant here.

Step two: arrays
----------------
Having parsed everything, I now know that I have a valid input that I can
work with.
First thing I do is centralize the arrays. Seeing as I know I've received an
input (because an empty array would have crashed the program), I can start
checking for empty elements, i.e. 0.0, starting from the end of the array.
once I've reached an element that is not 0.0, I save the index I've reached,
which is the index of the rightmost non-negative element in the array, and also
I keep the amount of 0.0 I've found. In order to know how many spaces I need
to move the elements in the array I simply divide the amount of 0.0 I've found
by 2 and take its floor value. Then in order to avoid deleting elements by
accident, I start moving elements starting from the index of the last non-negative
number going backwards.
Next I normalize the arrays. I simply go over the entire array and add the values
to a sum, then I go over the array again and divide every element by the sum.

Step three: convolution and histogram
-------------------------------------
I initialize an array that will store the results from the convolution. Then
I use the function convolution using the centralized, normalized arrays I've
created from the user input and the number of rooms (n). convolution itself is
just a for loop that calls the function littleConvolution n times.
I thought about using recursion in littleConvolusion, but I found it was
much simpler to just feed the result at the end of littleConvolution back
into itself. That way all we need is a for loop, without the need for
edge cases.
This algorithm runs in O(n^2), because it basically consists of two for
loops, one for the index in the first sample array and one for the index
in the second sample array, both of which are of the same size n.

Once that is once that is done I use histogram, which calls the function
roundMax to round all the elements in the array and save the max value in
it, and go over all the elements in the array while printing both the
elements and the the amount of stars representing them.