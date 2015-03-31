# scss-color
This makes theming very easy, by giving you the ability to store your colors in seperate namespaces. It also gives you the flexibility of getting different variations from a base color. But it's simple enough to get a variation of a color value without needing to define a namespace.


### `define-color-set($namespace: default, $settings: false, $merge-color-settings: true)`
This is used to set the color settings for the default namespace. The main reason this is a seperate mixin is for the simple fact that it increases the performance. Only having to merge the namespace settings with the necessary defaults once is a huge performance increase.

##### $settings options
These are the default settings, so if you like these you don't have to define anything.
```scss
$settings: (
 base: 0, // defines the base color to look for in the maps(not reccommended to change) this is ment for if you want o define out color names instead of using integers.
 increment: 5%, // this is a fall back increment if the color functions that are defined are lists and not maps
 colors: (), // placeholder for the colors of this namespace
 map: "integers", // determins if you're using integers to get your light and dark values or words.(@feature - this hasn't been added yet)
 smart-color: .25, // determins how far you go back if the color is #fff or #000 (example if you called color(#ccc, 25) that will be black but if smart color is set then it will be #060606)
 functions: ( // holds the light and dark functions to use for the different variations. (only light, and dark can be the keys of this map)
  light: ( // holds the light functions that will run if the variation is less than 0
   lighten: 5% // this is where you specify different sass color functions, or your own if you have any
  ),
  dark: ( // holds the dark functions that will run if the variation is less than 0
   darken: 5% // this is where you specify different sass color functions, or your own if you have any
  )
 )
);
```

##### Example: How to define default color settings

``` scss
$color-settings: ( // must be `$color-settings`
 smart-color: .45,
 colors: (
  primary: (
   0: #ccc, // base color
   name: gray // color name of primary(for debug purposes only)
  ),
  pink: #c82754 // base color for pink
 )
);
@import "_colors.scss";
// define-color-set doesn't need to be called if you define $color-settings (defaults) before you import `_colors.scss` it will automatically merge them.
```

##### Example: How to define default color settings after you import `_colors.scss`
``` scss
$color-settings: (
 smart-color: .45,
 colors: (
  a: (
   0: #ccc,
   name: gray
  )
 )
);
@include define-color-set; // this merges your preferences with the defaults, these will be used for every namespace that you define.
```

##### Example: How to define a different namespace of colors
```scss
@include define-color-set(messaging, (
  smart-color: .33,
  colors: (
   info: (
    0: #cbe8f5,
    name: light blue // the name of `#cbe8f5`, it's for debug purposes only
   )
  )
 )
);
```


### `color($this: #000, $variation: 0, $force: false, $save: true, $namespace: "default")`


##### Example: Get a different variation of a color value
```scss
$turquoise: #1abc9c;
color($turquoise, -2.5); // lighter - #33e3c0
color($turquoise, 2.5); // darker - #12846e
```

##### Example: Using the default theming option of this function
```scss
$color-settings: ( 
 colors: ( 
  a: ( 0: #ccc, name: gray), 
  b: ( 0: #c82754, name: pink),
  ...
 )
); // in your variables file
 
// in some other file
color(a, -1) // #d9d9d9
color(a) // #ccc
color(a, 1) // #bfbfbf
```

##### Example: Getting colors from a specific namespace
```
// define the namespace for the messaging colors
@include define-color-set(messaging, (
 colors: (
  info: #cbe8f5,
  error: #b02430,
  success: #4f6447,
  warning: #755e32
 )
));
@function messaging-color($type, $variation: 0){ // just a helper function to make it easier
 @return color($type, $variation, $namespace: messaging);
}
messaging-color(info, -1) // #e0f1f9
messaging-color(info, 0) // #cbe8f5
messaging-color(info) // #cbe8f5
messaging-color(info, 2.25) // #9bd3ec
```

@todo add more examples


### `console-colors($namespace: "default", $to-console: "colors")`
@todo add more examples
