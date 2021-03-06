Making Your First Program with Quil
===================================

Now that you know a bit about how to write Clojure code, let's look at
how to create a standalone application.

In order to do that, you'll first create a *project*. You'll also learn how to
specify your project's *dependencies*. Finally, you'll learn how to
*build* your project to create the standalone application.

## Create a Project

Up until now you've been experimenting in a REPL. Unfortunately, all
the work you do in a REPL is lost when you close the REPL. You can
think of a project as a permanent home for your code. You'll be using
a code editor called "Nightcode" to create and edit your project.

To create a new project, first download and install Nightcode. Because
newer versions removed the project helper that we want to use, please
use Nightcode version 2.0.4.  You can download it from
[this website](https://github.com/oakes/Nightcode/releases). Once it's
installed, start it up and click "Start" in the upper
left.

![](/curriculum/images/nc-scrn1.png?raw=true)

At the menu, Nightcode will offer you several types of projects you
want to start. Your project will be graphical, so choose the "Graphics
App" option:

![](/curriculum/images/nc-scrn2.png?raw=true)

Nightcode pops up a file chooser menu where you can select the name
and the place where to store your new project. 

Now, your project has been created, and you should see a directory
structure that looks like this on the left side of the Nightcode
screen:

```
drawing
├── README.md
├── project.clj
├── resources
└── src
    └── drawing
        └── core.clj
```

There's nothing inherently special or Clojure-y about this project
skeleton. It's just a convention used by Nightcode. You'll be using
Nightcode to build and run Clojure apps, and Nightcode expects your
app to be laid out this way. Here's the function of each part of the
skeleton:

- `project.clj` is a configuration file. It helps answer questions
  like, "What dependencies does this project have?" and "When this
  Clojure program runs, what function should get executed first?"
  Generally speaking, it describes your project.
- `README.md` is a file in the
  [markdown](https://daringfireball.net/projects/markdown/) format.
  By convention, it contains the first things a user of your project
  has to know. 
- `src/drawing/core.clj` is where the Clojure code goes

Because you chose the "Graphics App" option, your project is
configured to use a Clojure library called
[Quil](https://github.com/quil/quil), which creates drawings called
"sketches."

By default, your project has been setup with an example sketch. Let's go
ahead and run this code to make sure everything is working. In Nightcode,
look for the "Run with REPL" option on the bottom of the main window and click it.

![](/curriculum/images/nc-scrn3.png?raw=true)

As in the screenshot above, you should see a separate window popup with a
simple circle within it. If you don't see the window right away, wait a bit
and make sure it hasn't appeared somewhere behind your other windows
or on a different screen if you have multiple screens connected.

Great, you've run your program!

## Your first real program

### Drawing with Quil

Quil is a Clojure library that provides the powers of [Processing](https://processing.org/), a
tool that allows you to create drawings and animations. We will use
the functions of Quil to create some of our own drawings.

Let's start reading at the top of the file. As your programs get more complex, you'll need to organize them. You
organize your Clojure code by placing related functions and data in
separate files. Clojure expects each file to correspond to a
*namespace*, so you must *declare* a namespace at the top of each
file.

There are a couple of things going on here. First, the `:require` in
`ns` tells Clojure to load another namespace. The `:refer :all` parts of
`:require` tells Clojure that you want to call every function in that 
namespace with no prefix.

The first function looks a bit like this:

```clojure
(defn draw []
   ;; Do some things
   )
```

Inside, will can call functions, like the quil function to set the background color:
```clojure
   ;; Call the quil background function
   (background 255)
```

Put it together:
```clojure
(defn draw []
   ;; Call the quil background function
   (background 255))
```

In order to create a drawing (or sketch in Quil lingo) with Quil, you
have to define the `setup`, `draw`, and `sketch` functions. `setup` is
where you set the stage for your drawing. `draw` happens repeatedly,
so that is where the action of your drawing happens. `sketch` is the
stage itself. Let's define these functions together, and you will see
what they do.

In Nightcode, in the core.clj file, add the following after the
closing parenthesis of the ns statement from before.

```clojure
(defn setup []
  (smooth)
  ;; New lines!
  (color-mode :rgb)
  (stroke 255 0 0))
```

This is the `setup` function that sets the stage for the
drawing. First, we call quil's `smooth` function to say that the
drawing will have smooth (not pixelated) edges. Since we did `:refer :all`
in the namespace, we can use the `smooth`, `color-mode` and `stroke` 
directly from the quil library. 

Second, we set the color mode to RGB (meaning we can define every color
using a combination of **r**ed, **g**reen and **b**lue colors).

Third, we set the color of the lines we will draw with `stroke`. The
code 255 0 0 represents red. You can [look up RGB codes](http://xona.com/colorlist/) for other
colors if you would like to try something else.

In Nightcode, in the core.clj file, find where the `draw` function is defined. Remove the 
line that says `(ellipse 100 100 30 30)` and replace it with the following lines:

```clojure
  (line 0 0 (mouse-x) (mouse-y))
  (line 200 0 (mouse-x) (mouse-y))
  (line 0 200 (mouse-x) (mouse-y))
  (line 200 200 (mouse-x) (mouse-y))
```

Now the function should look like:

```clojure
(defn draw []
  (background 255)
  (fill 192)
  ;; the new lines!
  (line 0 0 (mouse-x) (mouse-y))
  (line 200 0 (mouse-x) (mouse-y))
  (line 0 200 (mouse-x) (mouse-y))
  (line 200 200 (mouse-x) (mouse-y)))
```
The call to `(background 255)` sets the background to white (this is
the same as calling `(background 255 255 255)`).
The next call `(fill 192)` sets the color of every thing we draw afterwards.
`192` is a gray color.
After, we call the quil `line` function four times. 
We also call two functions repeatedly as the arguments to the `line` function:
`mouse-x` and `mouse-y`. These get the current position (x and y
coordinates on a 2d plane) of the mouse. 

The `line` function takes four arguments - two sets of x, y coordinates. 
The first x and y are the starting position of the line. The second 
x and y are the ending position of the line. So we start each of 
these lines at a fixed position, then end them wherever the mouse
is when the sketch is drawn.

```clojure
(defsketch example
  :title "You can see lines"
  :setup setup
  :draw draw
  :size [500 500])
```

This is our sketch. You can set attributes of the sketch such as the
title and size. You also tell it what are the names of the setup and
draw functions. These have to match exactly the function names we used
above.

Save the file and press to "Reload" to evaluate the file. Your drawing should appear.

### Exercise: Rainbow lines
Update your drawing so that:
* the lines are a different color
* the title is different
* the lines start at a different place

Bonus: Make each of the four lines a different color.

Bonus #2: Change the color of the lines based on the mouse position.

Hint: You can browse the [Quil API](http://quil.info/api) for ideas and function definitions.
