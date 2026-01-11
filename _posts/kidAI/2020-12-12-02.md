---
layout: post
title: Graphic Basics
category: KIDAI
tags: p5js
keywords: javascript random
---

# 01 Graphic Basics


### 1 My AI robot

Draw your first AI robot with P5js. You can use line, ellipse, rectangle and text.

Draw your robot on the white [borad](https://wbo.ophir.dev/boards/KID) first, then translate it into code.
Here are some drawings:
![](https://fabocloud.cn/fab/2020/wikidoc/-/raw/master/docs/images/p5js/myairobots.png)

Here is an example:
```javascript
let i = 0;

function setup() {
  createCanvas(320, 350);
  frameRate(4);
}

function draw() {
  background(220);

  fill(249,194,163);
  circle(150, 100, 100);
  circle(125, 240, 50);
  circle(175, 240, 50);
  fill(96,151,235);
  rect(100, 100, 100, 140);
  fill(255);
  line(100, 120, 50 + i % 2 * 10, 160);
  line(200, 120, 240 - i % 2 * 10, 160);
  fill(249,194,163);
  circle(50 + i % 2 * 10, 160, 20);
  circle(240 - i % 2 * 10, 160, 20);
  fill('yellow');
  quad(100, 120, 200, 120, 152, 160, 148, 160);
  fill('red');
  textSize(30);
  text('S', 140, 145);
  fill(255);

  i++;
}
```
<iframe src="https://editor.p5js.org/oneandzeros/present/WtI1HZdCl" width="320px" height="350px" frameborder=""> </iframe>

[P5Editor](https://editor.p5js.org/oneandzeros/sketches/WtI1HZdCl)


### 2 Four Corners

举一隅不以三隅反，则不复也。

```javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);

  for (let j = 0; j < 400; j++) {

    line(0, j * 10, j * 10, height)

  }
}
```

<iframe src="https://editor.p5js.org/oneandzeros/present/-yrstBRkp" width="400px" height="400px" frameborder=""> </iframe>

Make your own verion of corners:
<iframe src="https://editor.p5js.org/oneandzeros/present/bggAkAZi8" width="400px" height="400px" frameborder=""> </iframe>
<iframe src="https://editor.p5js.org/oneandzeros/present/VesCyLD9l" width="400px" height="400px" frameborder=""> </iframe>


### 3 Mouse Events: click and drag

We use ** mousePressed ** function to process the logic when use click mouse. And ** mouseDragged ** function is for dragging a mouse.

```javascript
function setup() {
  createCanvas(600, 600);
  noLoop();
}

function draw() {
  background(220);
}

let snapx;
let snapy;

function mousePressed() {
  snapx = mouseX;
  snapy = mouseY;
}

function mouseDragged() {
  let r = sqrt((mouseX-snapx)*(mouseX-snapx) +
              (mouseY-snapy)*(mouseY-snapy))
  fill(random(255),random(255), random(255), random(100,255));
  ellipse(snapx, snapy, r, r);
  return false;
}
```
<iframe src="https://editor.p5js.org/oneandzeros/present/p9yPqnBu_" width="600px" height="600px" frameborder=""> </iframe>

You can drag the mouse to create an picutre like this:
![](https://fabocloud.cn/fab/2020/wikidoc/-/raw/master/docs/images/p5js/dragandcircles.png)
