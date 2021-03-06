# Fractals

![](pics/fractals.PNG)

## Introduction

Fractals are generative, they are built with very complex pattern and they allow you to zoom in forever! In this repo I will see how easily it is to generate several kinds of fractals using a technique called [L-Systems](https://en.wikipedia.org/wiki/L-system) and the Python Turtle module for the step by step pen plotting.

## Overview

In this repo I'm not going to dive into too many technical details but instead I'll present drawings, a lot of animated examples and at the end, the code to generate these fractals. There will be some resources for both code and math background to explore at the end.

## What's a Fractal?

First lets give a definition of what a fractal is, it is basically a geometric figure which shows the same characteristics no matter how much you zoom in; a recursive non-intersecting line.

A fractal is an object or quantity that displays self-similarity, in a somewhat technical sense, on all scales. The object need not exhibit exactly the same structure at all scales, but the same "type" of structures must appear on all scales. A plot of the quantity on a log-log graph versus scale then gives a straight line, whose slope is said to be the fractal dimension. - [Math World](https://mathworld.wolfram.com/Fractal.html)

## How can we draw Fractals with Python?

Fractals are typically hard to draw, because there is a concept which is recursion. When we talk about graphics and pen plotting we usually talk about [raster](https://en.wikipedia.org/wiki/Raster_graphics) or [vector](https://en.wikipedia.org/wiki/Vector_graphics), but there is always a limit, fractals by definition are infinitely recursive. So when we want to pen plot fractals we should stop at some point, that's why we talk about "iterations". At each iteration the fractal becomes more and more complex, but at some point it is impossible to distinguish between to successive iterations. This happens when changes occur at individual pixel levels, so it is quite reasonable to stop there, sometimes it is quite clear what the shape is and we can stop even earlier.

A two examples for this are the [Quadratic Koch Island](https://en.wikipedia.org/wiki/Koch_snowflake), which with 3 iterations has a clear structure and in the other hand the [Dragon Curve](https://en.wikipedia.org/wiki/Dragon_curve) which has a clear structure with 8 iterations. How many iterations are needed depends highly on the specific fractal we are working with.

Certainly there are lots of plotting libraries in Python, being Matplotlib the most popular but they are usually design to plot statistical data and well known plots. Matplotlib in particular has some low level constructs that allow us to build fractals but this time we will be focusing in a usually forget module in the standard library, the Turtle Module.

## Turtle Module

According to [Python docs](https://docs.python.org/3.6/library/turtle.html):

```txt
"Turtle graphics is a popular way for introducing programming to kids.
It was part of the original Logo programming language developed by Wally Feurzig
and Seymour Papert in 1966."
```

The key here is that turtle recognizes basically 2 commands:

* Move Forwards and Backwards by side (distance)
* Turn Left and Right by angle (direction)

Note: The standard library provided other commands but here we are going to just those 2.

Additionally we have the option to:

* Up and Down (position)

This characteristics seems too simple for plotting such complex graphics as fractals but we will use another tool that uses just this little set of instructions, I'm talking about [L-Systems](https://en.wikipedia.org/wiki/L-system).

## L-Systems

An L-System is a way of representing recursive structures (such as fractals) as a string of 8 bit characters, this is done by rewriting the string over and over. Again, the formal definition is the following:

An L-system, is a string rewriting system that can be used to generate fractals with dimension between 1 and 2. - [Math World](https://mathworld.wolfram.com/LindenmayerSystem.html)

Once we understand what an L-System is we can produce recursive structures, but before we are able to do that we need to understand what are the instructions we need. Every L-System has:

* `An alphabet`: The set of symbols the L-System is going to use.
* `An axiom`: The initial string for the generation.
* `A set of production rules`: These rules tells how each symbol should be replaced in the following iteration.

This is very similar to the definition of a [Formal Grammar](https://en.wikipedia.org/wiki/Formal_grammar), the key difference is that in each iteration, as opposed to grammars, as many rules as possible are applied instead of just one. So L-Systems are a subset of [Context-Free Grammar](https://en.wikipedia.org/wiki/Context-free_grammar).

Since we are going to use Turtle to plot and L-Systems to represent what we want to generate we need to create a relationship between them.

Since the only commands we have in Turtle are the mentioned above we will assign each a symbol which will represent the alphabet.

* `F`: Move Forwards
* `+`: Turn Right
* `-`: Turn Left

In order to make this work, each fractal should also provide a direction, which will be the angle the turtle will turn either right or left, for simplicity reasons only one angle should be provided and the L-System should be written taking that into consideration.

The [axiom](https://en.wikipedia.org/wiki/Axiom) and the production rules will depend on the fractal only, but the fractal should be written in a way that can be represented by these only three symbols. This introduces a limitation, we will be able to produce only one-line fractals, so some such as the [Cantor Set](https://en.wikipedia.org/wiki/Cantor_set) won't be able to be produced this way, this is only a simplification, since we can introduce two other commands to move forwards without writing and analogously for the backwards movement, but to keep things simple we will keep that simplification.

Now let's move to some drawing!

## Drawing

The following drawings were compiled from several places publicly available on internet. I decided to port them to Python with the help of the Turtle module, center them, adding colors and provide a way to export them in vectorial SVG format for the pen plotter.

Because the browser execute Python via [Skulpt](https://skulpt.org/) and [Colorsys](https://docs.python.org/3/library/colorsys.html) isn't supported yet, the sketches will be black & white but images with colors and the corresponding code to generate them will be provided. Each sketch has a code associated which you can open and fork in [Repl.it](https://replit.com/)

WARNING: The sketches you are about to see are quite large in size, it is recommended to see them only with a good connection. The Repl snippet may not work since it uses your resources so mobile users might not be able to see it properly.

Note: Skulpt uses YOUR BROWSER to render and to make the sketch so if in any case you experience delays, lags or strange behaviour it may be a problem with the browser, replaying the sketch or reloading the page should fix most issues. It may not work properly on mobile.

The sketches are ordered by their complexity, so the best ones are at the end.

### Koch-Snowflake

```python
axiom = "F--F--F"
rules = {"F":"F+F--F+F"}
iterations = 4 # TOP: 7
angle = 60
```

![](pics/01_koch_snowflake.JPG)

```python
import turtle
from math import cos, sin, radians

def create_l_system(iters, axiom, rules):
    start_string = axiom
    if iters == 0:
        return axiom
    end_string = ""
    for _ in range(iters):
        end_string = "".join(rules[i] if i in rules else i for i in start_string)
        start_string = end_string

    return end_string

def draw_l_system(t, instructions, angle, distance):
    steps = len([i for i in instructions if i in "FB"])
    step = 1 / steps
    i = 0
    for cmd in instructions:
        if cmd == 'F':
            t.forward(distance)
        elif cmd == 'B':
            t.backward(distance)
        elif cmd == '+':
            t.right(angle)
        elif cmd == '-':
            t.left(angle)

def calc_length_height(instructions, angle, correction_angle):
    current_angle = correction_angle
    x_offset = 0
    y_offset = 0
    min_x = 0
    min_y = 0
    max_x = 0
    max_y = 0
    for inst in instructions:
        if inst == "F":
            x_offset += cos(radians(current_angle))
            y_offset += sin(radians(current_angle))
        elif inst == "B":
            x_offset -= cos(radians(current_angle))
            y_offset -= sin(radians(current_angle))
        elif inst == "+":
            current_angle -= angle
        elif inst == "-":
            current_angle += angle
        max_x = max(max_x, x_offset)
        min_x = min(min_x, x_offset)
        max_y = max(max_y, y_offset)
        min_y = min(min_y, y_offset)
    
    width = abs(max_x) + abs(min_x)
    height = abs(max_y) + abs(min_y)


    return width, height, abs(min_x), abs(min_y)

def main(iterations, axiom, rules, angle, length=None, size=None, correction_angle=0,
        y_offset=None, x_offset=None, offset_angle=None, inverted=False, flip_h=False, 
        flip_v=False, width=None, height=None, margin=None, aspect_ratio=None):

    inst = create_l_system(iterations, axiom, rules)

    width_, height_, min_x, min_y = calc_length_height(inst, angle, correction_angle)

    if width_ == 0 and height_ == 0:
        return

    if aspect_ratio is None:
        if 0 in [width_, height_]:
            aspect_ratio = 1
        else:
            aspect_ratio = width_ / height_

    if width is None and height:
        width = height / aspect_ratio

    if height is None and width:
        height = width / aspect_ratio
    
    if margin is None:
        margin = 35

    if offset_angle is None:
        offset_angle = -90

    if length is None:
        if width_ > height_:
            length = (width - 2 * margin) / width_
        else:
            length = (height - 2 * margin) / height_
    
    if width_ * length > width:
        length = (width - 2 * margin) / width_
    elif height_ * length > height:
        length = (height - 2 * margin) / height_
    
    if x_offset is None:
        if width_ >= height_  and (width - width_) <= width_ - 2 * margin :
            x_offset = -(width / 2 - margin) + min_x * length
        else:
            x_offset = -(width / 2) + (width - width_ * length) / 2 + min_x * length
    
    if y_offset is None:
        if height_ >= width_ and (height - height_) <= height_ - 2 * margin :
            y_offset = -(height / 2 - margin) + min_y * length
        else:
            y_offset = -(height / 2) + (height - height_ * length) / 2 + min_y * length

    if inverted:
        inst = inst.replace('+', '$')
        inst = inst.replace('-', '+')
        inst = inst.replace('$', '-')
        inst = inst.replace('F', '$')
        inst = inst.replace('B', 'F')
        inst = inst.replace('$', 'B')
    
    if flip_h:
        inst = inst.replace('F', '$')
        inst = inst.replace('B', 'F')
        inst = inst.replace('$', 'B')
        y_offset *= -1

    if flip_v:
        inst = inst.replace('+', '$')
        inst = inst.replace('-', '+')
        inst = inst.replace('$', '-')
        y_offset *= -1

    if size is None:
        if length < 3:
            size = 1
        elif length < 12:
            size = 2
        elif length < 25:
            size = 3
        else:            
            size = 5

    t = turtle.Turtle()
    wn = turtle.Screen()
    wn.setup(width, height)

    t.up()    
    t.backward(-x_offset)
    t.left(90)
    t.backward(-y_offset)
    t.left(offset_angle)
    t.down()
    t.speed(0)
    t.pensize(size)
    draw_l_system(t, inst, angle, length)
    t.hideturtle()
    
    wn.exitonclick()

# Global parameters
width = 450

title = "Koch-Snowflake" 
axiom = "F--F--F"
rules = {"F":"F+F--F+F"}
iterations = 3 # TOP: 7
angle = 60

main(iterations, axiom, rules, angle, aspect_ratio=1, width=width)
```