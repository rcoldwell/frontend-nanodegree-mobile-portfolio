####Website Optimization - Udacity project 4


####Part 1: Optimize PageSpeed Insights score for index.html
Launch url: http://rcoldwell.github.io/frontend-nanodegree-mobile-portfolio/

#####PageSpeed Insights:
 - mobile:91/100  
 - desktop: 92/100

#####Optimization on index.html was accomplished by:
- Inlining CSS
- Setting the Google analytics.js and perf.js files to async
- Minified style.css and main.js
- Moving the analytics inline script and Google font reference to the end of the body
- The images pizzeria.jpg, pizza.png and profilepic.jpg were resized and recompressed



####Part 2: Optimize performance in pizza.html
Launch url: http://rcoldwell.github.io/frontend-nanodegree-mobile-portfolio/views/pizza.html

#####Rendering animated pizzas performance improvements
- Scrolling renders pizzas faster than 60fps
- Scrolltop dom query moved out of for loop
- Switched from querySelectorAll to getElementsByClassName

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
        var phase;
        for (i = 0; i < items.length; i++) {
           phase = Math.sin(top / 1250 + i % 5);
           items[i].style.left = items[i].basicLeft + 100 * phase + "px";
        }
        ...

#####Pizza creation performance improvements
- Creating pizzas takes 18 ms on page load
- Number of pizzas dynamically created based on window height

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
        var cols = 8;
        var s = 256;
        //number of pizzas calculated based on viewport
        var windowheight = window.innerHeight;
        var rowheight = 254;
        var rows = Math.ceil(windowheight/rowheight);
        var pizzacount = rows * cols;
        var elem;
        var movingpizzas = document.getElementById("movingPizzas1");
        for (var i = 0; i < pizzacount; i++) {
            elem = document.createElement('img');
            elem.className = 'mover';
            elem.src = "images/pizza.png";
            elem.style.height = "100px";
            elem.style.width = "73px";
            elem.basicLeft = (i % cols) * s;
            elem.style.top = (Math.floor(i / cols) * s) + 'px';
            movingpizzas.appendChild(elem);
        }
        updatePositions();
    });

#####Pizza resizing performance improvements
- Resizing occurs in approximately 1.1 ms
- Variables moved outside the for loop to prevent unnecessary additional queries/calculations
- Variable reference used to set new width instead of making another dom query
- Switched from querySelectorAll to getElementsByClassName

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
        var containercount = pizzacontainers.length;
        for (var i = 0; i < containercount; i++) {
            pizzacontainers[i].style.width = newwidth;
        }
     }
    
#####Graphics optimizations
- The images pizzeria.jpg, pizza.png and profilepic.jpg were resized, recompressed and metadata removed
