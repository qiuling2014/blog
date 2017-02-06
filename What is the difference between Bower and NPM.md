### What is the difference between Bower and NPM

npm is most commonly used for managing Node.js modules, but it works for the front-end too when combined with Browserify and/or $ npm dedupe.

Bower is created solely for the front-end and is optimized with that in mind. The biggest difference is that npm does nested dependency tree (size heavy) while Bower requires a flat dependency tree (puts the burden of dependency resolution on the user).

A nested dependency tree means that your dependencies can have its own dependencies which can have their own, and so on. This is really great on the server where you don't have to care much about space and latency. It lets you not have to care about dependency conflicts as all your dependencies use e.g. their own version of Underscore. This obviously doesn't work that well on the front-end. Imagine a site having to download three copies of jQuery.

The reason many projects use both is that they use Bower for front-end packages and npm for developer tools like Yeoman, Grunt, Gulp, JSHint, CoffeeScript, etc.

All package managers have many downsides. You just have to pick which you can live with.

![](http://7xpwoh.com1.z0.glb.clouddn.com/16-1-21/25700947.jpg)


#### 链接
[1].[stackoverflow](http://stackoverflow.com/questions/18641899/what-is-the-difference-between-bower-and-npm)
