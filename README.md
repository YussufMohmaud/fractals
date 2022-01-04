# Fractals

![Alternate text](pics/fractals.PNG)

## Introduction

Fractals are generative, they are built with very complex pattern and they allow you to zoom in forever! In this repo I will see how easily it is to generate several kinds of fractals using a technique called L-Systems and the Python Turtle module for the step by step pen plotting.

## Overview

In this repo I'm not going to dive into too many technical details but instead I'll present drawings, a lot of animated examples and at the end, the code to generate these fractals. There will be some resources for both code and math background to explore at the end.

## What's a Fractal?

First lets give a definition of what a fractal is, it is basically a geometric figure which shows the same characteristics no matter how much you zoom in; a recursive non-intersecting line.

A fractal is an object or quantity that displays self-similarity, in a somewhat technical sense, on all scales. The object need not exhibit exactly the same structure at all scales, but the same "type" of structures must appear on all scales. A plot of the quantity on a log-log graph versus scale then gives a straight line, whose slope is said to be the fractal dimension. - [Math World](https://mathworld.wolfram.com/Fractal.html)

## How can we draw Fractals with Python?

Fractals are typically hard to draw, because there is a concept which is recursion. When we talk about graphics and pen plotting we usually talk about [raster](https://en.wikipedia.org/wiki/Raster_graphics) or [vector](https://en.wikipedia.org/wiki/Vector_graphics), but there is always a limit, fractals by definition are infinitely recursive. So when we want to pen plot fractals we should stop at some point, that's why we talk about "iterations". At each iteration the fractal becomes more and more complex, but at some point it is impossible to distinguish between to successive iterations. This happens when changes occur at individual pixel levels, so it is quite reasonable to stop there, sometimes it is quite clear what the shape is and we can stop even earlier.

A two examples for this are the [Quadratic Koch Island](https://en.wikipedia.org/wiki/Koch_snowflake), which with 3 iterations has a clear structure and in the other hand the [Dragon Curve](https://en.wikipedia.org/wiki/Dragon_curve) which has a clear structure with 8 iterations. How many iterations are needed depends highly on the specific fractal we are working with.

Certainly there are lots of plotting libraries in Python, being Matplotlib the most popular but they are usually design to plot statistical data and well known plots. Matplotlib in particular has some low level constructs that allow us to build fractals but this time we will be focusing in a usually forget module in the standard library, the Turtle Module.