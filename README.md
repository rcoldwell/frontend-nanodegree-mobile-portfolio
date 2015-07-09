####Website Optimization - Udacity project 4


####Part 1: Optimize PageSpeed Insights score for index.html

PageSpeed mobile:96  desktop: 97

Optimization on index.html was accomplished by:

    • Inlining CSS
    • Setting the Google analytics.js and perf.js files to async
    • Moving the analytics inline script and Google font reference to the end of the body
    • The images pizzeria.jpg, pizza.png and profilepic.jpg were resized and recompressed


####Part 2: Optimize Frames per Second in pizza.html

- scrolling 60 frames in 0.6 secs

    addEventListener total number of images reduced to only enough to display on screen
    updatePositions() 0-4 offset in phase calculation changed from % calculation to array and calculation consolidated to one line
    
- resizing pizzas optimized to 1.1 ms

    addEventListener global items array removing rebuilding of array on each change
    changePizzaSizes() element acquisition method changed to getElementsByClassName and set as array to prevent multiple calls of same method