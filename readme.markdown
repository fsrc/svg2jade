SVG to Jade partial
===================
Lately I have been working on a SVG intensive Node.js project. So I created a Node.js script that takes a SVG-file straight from Illustrator and converts that to a usable partial for including in other Jade-files.

Installation
============
npm install -g svg2jadepartial

Usage
=====
svg2jadepartial <any_svg_file.svg>

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

Extras
======
I also have a small serverside function that will automatically create a svg-wrapper around the partial based on the size that the partial has. It looks like this. Tastes like Coffee..

    svg_wrap = (id, display, partials) ->
      total_height = 0
      total_width = 0
  
      # If we only got a single partial, put that into an array.
      partials = [partials] if partials.constructor != Array
  
      partials = for partial in partials
        regex_res = /width\s*=\s*["']([\d\.]+)["']\s+height\s*=\s*["']([\d\.]+)["']/i.exec partial
        total_width   += parseFloat(regex_res[1]) if regex_res and regex_res[1]
        total_height  += parseFloat(regex_res[2]) if regex_res and regex_res[2]
        partial

      "<svg version='1.1' id='#{id}' 
            xmlns='http://www.w3.org/2000/svg' 
            xmlns:xlink='http://www.w3.org/1999/xlink' 
            width='#{total_width}px' 
            height='#{total_height}px' 
            viewBox='0 0 #{total_width} #{total_height}' 
            enable-background='0 0 #{total_width} #{total_height}'
            xml:space='preserve'>
            <script src='/javascripts/app.js'></script>
        #{partials.join()}
      </svg>"

Put that somewhere inside your controller code. And push a reference to your view like so; (Still taste Coffee..)

    app.get '/view/welcome', (req, res) =>
      data = {
        svg_wrap:svg_wrap,
        layout:'basic-layout',
        anyothervar:"value"
      }
      
      # IMPORTANT: You will need this line to be able to render SVG
      res.header 'Content-Type', 'text/xml; charset=utf-8'
      # Render the view
      res.render 'welcome-template', data

**IMPORTANT: You will need this line to be able to render SVG.** Otherwise the browser wont recognize the SVG XML.

    res.header 'Content-Type', 'text/xml; charset=utf-8'

Now, in your jade template you would put; (This is javascript in Jade)

    != svg_wrap('welcome', true, partial('svg-welcome', { }))


Happy hacking!
==============