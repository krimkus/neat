Neat, a framework for Bourbon
================

Neat is an open source fluid grid framework built on top of [Bourbon](http://thoughtbot.com/bourbon), with an aim of being simple enough for developers to pick up and flexible enough for designers to customize. 

Unlike other frameworks that pollute your markup with presentation classes and require extra wrapping `<div>`'s, Neat relies entirely on Sass mixins and doesn’t require row containers, encouraging a cleaner and more semantic markup.

The grid proportions are calculated using `em` units and the golden ratio for more scalability and visual balance.

## Why is Neat not fully responsive?

A fully responsive framework would depend on Sass 3.2, which is yet to be released as a stable version. Breakpoint support will eventually make it to Neat once that happens.

Requirements
===

- Sass 3.1+
- Bourbon 2.1+

Installing Neat
===

Put the Neat folder in your Sass directory and import it right below Bourbon:

    @import "bourbon/bourbon";
    @import "neat/neat";
    

Using the grid
===
The default grid is comes with 12 columns (a setting that can be easily overridden as detailed below).

### Site container
Include the ```outer-container``` mixin in the topmost container (to which the ```max-width``` setting will be applied):
    
    div.container {
        @include outer-container;
    }
    

You can include this mixin in more than one element in the same page.

### Columns:
Use the ```span-columns``` mixin to specify the number of columns an element should span: 

    @include span-columns(columns, container, display type) 

* ```columns``` is the number of columns you wish this element to span.
* ```container``` is the number of columns the container spans, defaults to the total number of columns in the grid.
* ```display type``` changes the display type of the grid. Use ```block```—the default—for floated layout, ```table``` for table-cell layout, and ```inline-block``` for an inline block layout.

eg. Element that spans across 6 columns (out of the total number of columns defaulting to 12):

    div.element {
        @include span-columns(6);
    }


If the element's parent isn't the top-most container, you need to add the number of columns of the parent element to keep the right proportions: 

    div.container {
        @include outer-container;
        
        div.parent-element {
            @include span-columns(8);
            
            div.element {
                @include span-columns(6,8);
            }
        }
    }

To use a table-cell layout, add ```table``` as the ```display type``` argument:

    @include span-columns(6, 8, table)


Likewise for inline-block:

    @include span-columns(6, 8, inline-block)


### Rows
In order to clear floated or table-cell columns, use the ```row``` mixin:

    @include row(display type);

* ```display type``` takes either ```block```—the default—or ```table```.


Pre, post & pad
===

### Pre
To add columns of space before the column, use ```pre```:

    @extend pre(2);

Which shifts the content two columns to the right.

### Post

To add columns of space after the column, use ```post```:

    @extend post(2);

Which shifts the content two columns to the left.

### Pad

To add padding around the entire column use pad. By default it add the same value as the grid's gutter but can take any unit value.

    @extend pad;
    @extend pad(20px);


Omega
===

By default, Neat removes the last element's gutter. However that may not cover every use case. Use the ```omega``` mixin to manually do so.

    @include omega;

Removes right gutter. You can also use ```nth-omega``` to remove the gutter of a specific column:

    @include nth-omega(nth-child);

* ```nth-child``` takes any valid :nth-child number. See [https://developer.mozilla.org/en-US/docs/CSS/:nth-child](Mozilla's :nth-child documentation)

eg. Remove every 3rd gutter using:

    @include nth-omega(3n);

Fill Parent
===

This makes sure that the child fills 100% of its parent:

    @include fill-parent;


Changing the grid
===

### Variables

All Neat's default settings can be overridden. The ones that are most likely to be changed are ```$max-columns``` / ```$max-width```, and the complete list of settings can be found in ```neat/_settings.scss```. In order to override any of these settings, redefine the variable in your site-wide ```_variables.scss``` and make sure to import it *before* Neat:

    @import "bourbon/bourbon";
    @import "variables";
    @import "neat/neat";
    
Failing to do so will cause Neat to use the default values.
