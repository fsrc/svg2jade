SVG to Jade partial
===================
Lately I have been working on a SVG intensive Node.js project. So I created a Node.js script that takes a SVG-file straight from Illustrator and converts that to a usable partial for including in other Jade-files.

Features
========
 * Variables
 * Variadic ID
 * Workspaces
 * Translations
 * Inline HTML text (SVG 1.1 doesn't support text flow)
 * CSS referencing (eg. you can put the name of any class in the id of your object)

Limitations
===========
 * Illustrator doesn't export Artboards in any form.
 * You can not have more then 1 (one) root layer. If you do, they will be wrapped in a new root layer.

Dependencies
============
 * Html2Jade    (https://github.com/donpark/html2jade)
 * Node.js      (http://nodejs.org/)
 * CoffeeScript (http://jashkenas.github.com/coffee-script/)
 * node-lazy    (https://github.com/pkrumins/node-lazy)

Shortcut to success
===================
1. Create a Illustrator file and name your layer to "svg" (That just what I usually name it. You can use any name you like.)
2. Draw any shape (path) and name it "cool.anyclass".
3. Create a rectangle shape and name it "workspace".
4. Group the two shapes and make sure that the workspace shape is in the bottom.
5. Name your group to "first".
6. Inside your group create yet another shape and a rectangle then group these two.
7. Name the rectangle in your new group "translation"
8. Name the shape "myshape$(anyvariable).anyotherclass"
9. Save your file and choose Format SVG (NOTE: Do **NOT** choose compressed SVG). In SVG Options I use the following settings:
  * SVG Profiles: SVG 1.1
  * Fonts - Type: SVG
  * Fonts - Subsetting: None
  * Images: Link
  * Preserve Illustrator Editing Capabilities: **FALSE**
  * CSS Properties: Presentation attributes
  * Decimal places: 3
  * Encoding: UTF-8
  * Output fewer <tspan> elements: **TRUE**
  * Everything else: **FALSE**
10. Run svg2jadepartial on your file and out comes two jade files.
11. More to come..

Future
======
I have a small client side library that acts on default css class names such as clickable, foldable, visible and so forth. Setting the correct class name on an object would implement default behavior. However that library is still not subject for public display ;-)
