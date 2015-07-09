####Website Optimization - Udacity project 4


####Part 1: Optimize PageSpeed Insights score for index.html
Launch url: http://rcoldwell.github.io/frontend-nanodegree-mobile-portfolio/

#####PageSpeed Insights:
 - mobile:95/100  desktop: 96/100

Optimization on index.html was accomplished by:

    - Inlining CSS
    - Setting the Google analytics.js and perf.js files to async
    - Moving the analytics inline script and Google font reference to the end of the body
    - The images pizzeria.jpg, pizza.png and profilepic.jpg were resized and recompressed


####Part 2: Optimize Frames per Second in pizza.html
Launch url: http://rcoldwell.github.io/frontend-nanodegree-mobile-portfolio/views/pizza.html

#####Scrolling the page will render moving pizzas faster
Scrolling renders faster than 60fps
1. Scrolltop dom query moved out of for loop
2. Switched from querySelectorAll to getElementsByClassName

##### Original:
    function updatePositions() {
        ...
        var items = document.querySelectorAll('.mover');
        for (var i = 0; i < items.length; i++) {
            var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
            items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
        }
        ...

##### Modified:
    function updatePositions() {
        ...
        var items = document.getElementsByClassName("mover");
        var top = document.body.scrollTop;
        for (i = 0; i < items.length; i++) {
            var phase = Math.sin(top / 1250 + i % 5);
            items[i].style.left = items[i].basicLeft + 100 * phase + "px";
        }
        ...

    
####Pizza size slider performance improvements
1. Creating pizzas in 18 ms on page load
2. Columns reduced to 6 and pizza count reduced to 300 to only render visible pizzas

##### Original:
      document.addEventListener('DOMContentLoaded', function() {
        var cols = 8;
        var s = 256;
        for (var i = 0; i < 200; i++) {
          var elem = document.createElement('img');
          elem.className = 'mover';
          elem.src = "images/pizza.png";
          elem.style.height = "100px";
          elem.style.width = "73px";
          elem.basicLeft = (i % cols) * s;
          elem.style.top = (Math.floor(i / cols) * s) + 'px';
          document.querySelector("#movingPizzas1").appendChild(elem);
        }
        updatePositions();
      });

##### Modified:
document.addEventListener('DOMContentLoaded', function () {
    var cols = 6;
    var s = 256;
    for (var i = 0; i < 30; i++) {
        var elem = document.createElement('img');
        elem.className = 'mover';
        elem.src = "images/pizza.png";
        elem.style.height = "100px";
        elem.style.width = "73px";
        elem.basicLeft = (i % cols) * s;
        elem.style.top = (Math.floor(i / cols) * s) + 'px';
        document.querySelector("#movingPizzas1").appendChild(elem);
    }
    updatePositions();
});

Pizza slider resizing performance
1. Resizing in approximately 1.1 ms
2. Variables moved outside the for loop to prevent unnecessary additional queries/calculations
3. Variable reference used to set new width instead of making another dom query
4. Switched from querySelectorAll to getElementsByClassName

##### Original:
      function changePizzaSizes(size) {
        for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
          var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
          var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
          document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
        }
      }
##### Modified:
    function changePizzaSizes(size) {
        var pizzacontainers = document.getElementsByClassName("randomPizzaContainer");
        var dx = determineDx(pizzacontainers[0], size);
        var newwidth = (pizzacontainers[0].offsetWidth + dx) + 'px';
        for (var i = 0; i < pizzacontainers.length; i++) {
            pizzacontainers[i].style.width = newwidth;
        }
    }
    
#####graphics optimizations
   - The images pizzeria.jpg, pizza.png and profilepic.jpg were resized, recompressed and metadata removed
