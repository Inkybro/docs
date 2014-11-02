# Each binding

**NOTE:** Currently, one is not able to set the value of an HTML ```id``` attribute through Volt. This will be changed in the future. You can learn more, here. 

Below is an an example of what ***will not*** work:

```html
{{ each _items do |item| }}
  <p id="{{ index }}">{{ item }}</p>
{{ end }}
```

## Iteration of objects

For iteration over objects, one can simply use ```.each``` within a given template.

```html
{{ _items.each do |item| }}
  <p>{{ item }}</p>
{{ end }}
```

Above, if ```_items``` is an array, the block will be rendered for each item in the array, where ```item``` is the value of the array element in question.

## Iteration of objects with index values

You can also access the position of an item in the array, using the #index method. Using the #each method in this way will implicitly call the #each_with_index method.

```html
{{ each _items do |item| }}
  <p>{{ index }}. {{ item }}</p>
{{ end }}
```

For the array: ```['one', 'two', 'three']```, this would print:

    0. one
    1. two
    2. three

The zero offset ugliness is easily corrected by doing something like, ```{{ index + 1 }}```.

When items are removed or added to the array, the ```each``` binding automatically and intelligently adds or removes the items from/to the DOM.
