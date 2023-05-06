# Grid Simplified

This repository summarizes the most important concepts of CSS Grid. It is based on multiple tutorials and articles, attempting to condense the most practical use cases of Grid for future reference.

## Table of Contents

- [Grid Simplified](#grid-simplified)
  - [Table of Contents](#table-of-contents)
  - [Cheat Sheets](#cheat-sheets)
    - [Reel](#reel)
  - [What is CSS Grid?](#what-is-css-grid)
  - [Grid Properties](#grid-properties)
    - [Parent Properties](#parent-properties)
      - [`display`](#display)
      - [`grid-template-columns` and `grid-template-rows`](#grid-template-columns-and-grid-template-rows)
      - [`grid-template-areas`](#grid-template-areas)
      - [`gap`](#gap)
      - [`justify-items`](#justify-items)
      - [`align-items`](#align-items)
      - [`place-items`](#place-items)
      - [`justify-content`](#justify-content)
      - [`align-content`](#align-content)
      - [`place-content`](#place-content)
      - [`grid-auto-columns` and `grid-auto-rows`](#grid-auto-columns-and-grid-auto-rows)
      - [`grid-auto-flow`](#grid-auto-flow)
    - [Child Properties](#child-properties)
      - [`grid-column-start` and `grid-column-end`](#grid-column-start-and-grid-column-end)
      - [`grid-row-start` and `grid-row-end`](#grid-row-start-and-grid-row-end)
      - [`grid-column` and `grid-row`](#grid-column-and-grid-row)
      - [`grid-area`](#grid-area)
      - [`justify-self`](#justify-self)
      - [`align-self`](#align-self)
      - [`place-self`](#place-self)
  - [Special Units \& Functions](#special-units--functions)
    - [`fr`](#fr)
    - [Sizing Keywords](#sizing-keywords)
    - [Sizing Functions](#sizing-functions)
    - [The `repeat()` Function](#the-repeat-function)
      - [`auto-fill`](#auto-fill)
      - [`auto-fit`](#auto-fit)
  - [Even Columns](#even-columns)
  - [Don't Declare Grid Rows](#dont-declare-grid-rows)
  - [Don't Use Percentages](#dont-use-percentages)
  - [Spanning Grid Items](#spanning-grid-items)
  - [Grid Areas](#grid-areas)
  - [`place-content` vs `place-items`](#place-content-vs-place-items)
  - [Stacking Content](#stacking-content)
  - [Responsive Grids](#responsive-grids)
    - [Overflow](#overflow)
    - [Max Column Count](#max-column-count)
  - [Sticky Footers](#sticky-footers)

## Cheat Sheets

### Reel

To create a reel (sliding gallery) with CSS Grid, we can use the following code:

```css
.parent {
	--gap: 1rem;

	display: grid;
	gap: var(--gap);
	grid-auto-flow: column;
	grid-auto-columns: calc(50% - (var(--gap) / 2)); /* fit two at a time */
	overflow-x: scroll;
	scroll-snap-type: x mandatory;
	scroll-padding: var(--gap);
}

.parent > * {
	scroll-snap-align: start;
}
```

## What is CSS Grid?

CSS Grid is a two-dimensional layout model that allows you to create grid-based layouts in modern browsers.

It's main purpose in modern CSS is to provide an efficient way to lay out, align, and distribute space among items in a container, even when their size is unknown and/or dynamic.

To use CSS Grid, you must first declare a parent element as a grid container. This is done by setting the `display` property to `grid` or `inline-grid`.

Consider the following HTML:

```html
<div class="grid">
	<div class="item">Item 1</div>
	<div class="item">Item 2</div>
	<div class="item">Item 3</div>
</div>
```

By setting `display: grid` on the parent element, we can turn it into a grid container. This will cause the child elements to become grid items.

```css
.grid {
	display: grid;
}
```

Unlike Flexbox, setting `display: grid` doesn't actually do anything. To define our grid layout, it's typical to use the `grid-template-columns` and `grid-template-rows` properties.

The dividing lines that make up the structure of the grid are called **_grid lines_**, and can be either vertical (column grid lines) or horizontal (row grid lines).

**Note: All images are credited to [CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)**

See the image below:

![Grid Lines](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-line.svg)

A single "unit" of the grid is called a \***\*grid cell\*\***.

The following image depicts a grid cell between row grid lines 1 and 2, and column grid lines 2 and 3:

![Grid Cell](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-cell.svg)

Finally, the columns or rows of the grid are called \***\*grid tracks\*\***.

Here's an image of the grid track between the second and third row grid lines:

![Grid Track](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-track.svg)

To get started with CSS Grid, let's look at the following CSS:

```css
.parent {
	display: grid;
	grid-template-columns: 33% 33% 33%;
}
```

Here, we are saying we want to create a grid with three columns, each of which is 33% of the width of the parent element.

We can also use the `fr` unit to define our grid tracks. The `fr` unit is a grid-only unit that represents a fraction of the available space in the grid container. For example, if we have three columns, each with a width of `1fr`, they will each take up one-third of the available space.

```css
.parent {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
}
```

This can be further simplified by utilizing the `repeat()` function. The `repeat()` function takes two arguments: the number of times to repeat, and the value to repeat.

```css
.parent {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
}
```

But this still "hard-codes" the number of columns. What if we don't want to "hard-code" this? If we only have two columns, we want the columns to be 50% each. If we have four columns, we want them to be 25% each. And so on.

This can be achieved using the `grid-auto-flow` property.

```css
.parent {
	display: grid;
	grid-auto-flow: column;
}
```

Now, it will work similar to Flexbox and adjust the number of columns based on the number of items.

## Grid Properties

CSS Grid has a number of properties that can be used to control the layout of grid items within a grid container. Since it's a two-dimensional layout model, there are properties for the grid container or parent, and properties for the grid items or children.

### Parent Properties

#### `display`

The `display` property is used to define a grid container. It can be set to one of two values:

- `grid`
- `inline-grid`

As the names sound, `grid` generates a block-level grid, while `inline-grid` generates an inline-level grid.

Most of the time, you'll want to use `grid` to create a grid container.

Remember that `display: grid`, on its own, doesn't actually do anything. It simply turns the element into a grid container, unlike Flexbox which will change the layout immediately.

#### `grid-template-columns` and `grid-template-rows`

The `grid-template-columns` and `grid-template-rows` properties are used to define the columns and rows of the grid with a space-separated list of values.

The values represent the track size, and can be a length, a percentage, an intrinsic size (`min-content`, `max-content`, `fit-content`) or a fraction (`fr`). The number of values provided represents the number of columns or rows in the grid.

Let's look at a very simple example. Consider the following CSS:

```css
.parent {
	display: grid;
	grid-template-columns: 33% 33% 33%;
}
```

Here, we are saying that each column should be 33% of the width of the parent element. This will create a grid with three equal columns.

Alternatively, we could make one column larger like so:

```css
.parent {
	display: grid;
	grid-template-columns: 50% 25% 25%;
}
```

Now, the first column will be twice as wide as the other two.

With the grid-only unit `fr`, we can set the size as a fraction of the \***\*free space\*\*** in the grid container. For example, if we have three columns, each with a width of `1fr`, they will each take up one-third of the available space.

```css
.parent {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
}
```

The free space is calculated after any non-flexible items. For example, `grid-template-columns: 1fr 50px 1fr 1fr` will calculate the free space after the `50px` column.

Immediate things to take notice here are, compared to Flexbox, grid allows you to define your layout configuring only the parent element.

CSS Grid is also easier if you want specific sizes for columns.

You can also define explicit line names using the `[]` syntax, but it's very rarely used. For example:

```css
.parent {
	display: grid;
	grid-template-columns: [col1-start] 1fr [col2-start] 1fr [col3-start] 1fr [col3-end];
	grid-template-rows: [row1-start] 1fr [row2-start] 1fr [row3-start] 1fr [row3-end];
}
```

This will create a grid with three columns and three rows, with the first column and row named `col1-start` and `row1-start`, respectively.

#### `grid-template-areas`

The `grid-template-areas` property is used to define a grid template by referencing the names of the grid areas which are specified with the `grid-area` property.

Repeating the name of a grid area causes the content to span those cells. A period signifies an empty cell. Its syntax is easier to understand with an example.

Consider the following HTML:

```html
<div class="parent">
	<header></header>
	<main></main>
	<aside></aside>
	<footer></footer>
</div>
```

We can define `grid-template-areas` like so:

```css
.parent {
	display: grid;
	grid-template-areas:
		'header header header header'
		'main main . aside'
		'footer footer footer footer';
}
```

\***\*Each string represents a row in the grid.\*\***

\***\*Each space in the string represents a column.\*\***

Notice that we can use the `.` character to signify an empty cell.

So, this will create a grid that's four columns wide by three rows tall. The entire top row will be the header, the entire bottom row will be the footer, and the middle row will be split into four columns, with the main content spanning two columns, and the aside spanning one column.

On its own, this won't have any immediate effect. We need to assign the grid areas to the grid items using the `grid-area` property.

```css
header {
	grid-area: header;
}

main {
	grid-area: main;
}

aside {
	grid-area: aside;
}

footer {
	grid-area: footer;
}
```

Now, the grid will appear like so (credit: [CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)):

![CSS Grid Template Areas](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-template-areas.svg)

Now, on larger screens or smaller screens, all we have to do is redefine the `grid-template-areas` property on the parent, and the grid will automatically adjust.

#### `gap`

The `gap` or `grid-gap` (deprecated) property is a shorthand for `row-gap` and `column-gap`. However, due to the symmetrical layouts used in modern websites, it's the go-to property for spacing between grid items.

It will only control the spacing between grid items, not including outer edges.

Its syntax is as follows:

- `gap: <row-gap> <column-gap>`

If you want to set the gap individually, you can use `row-gap` and `column-gap` instead.

These are also known as `grid-row-gap` and `grid-column-gap`, now deprecated.

#### `justify-items`

The `justify-items` property controls the alignment of grid items along the inline (row) axis. It applies to all grid items inside the container and can be set to one of four values:

- `start`
- `end`
- `center`
- `stretch` (default)

`justify-items: start` will align the items with the start edge of their cell.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-start.svg)

`justify-items: end` will align the items with the end edge of their cell.

![End](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-end.svg)

`justify-items: center` will align the items in the center of their cell.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-center.svg)

`justify-items: stretch` will have the items fill the entire width of their cell.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-stretch.svg)

This behavior can also be specified for individual grid items using the `justify-self` property.

#### `align-items`

The `align-items` property controls the alignment of grid items along the block (column) axis. It applies to all grid items inside the container and can be set to one of five values:

- `start`
- `end`
- `center`
- `stretch` (default)
- `baseline`

`align-items: start` will align the items with the start edge of their cell.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/align-items-start.svg)

`align-items: end` will align the items with the end edge of their cell.

![End](https://css-tricks.com/wp-content/uploads/2018/11/align-items-end.svg)

`align-items: center` will align the items in the center of their cell.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/align-items-center.svg)

`align-items: stretch` will have the items fill the entire height of their cell.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/align-items-stretch.svg)

`align-items: baseline` will align the items along the text baseline.

- `align-items: first baseline` will align the items along the first line of text.
- `align-items: last baseline` will align the items along the last line of text.

This behavior can also be specified for individual grid items using the `align-self` property.

#### `place-items`

The `place-items` property is a shorthand for `align-items` and `justify-items`.

The first value is the `align-items` value, and the second value is the `justify-items` value.

Its syntax is as follows:

- `place-items: <align-items> / <justify-items>`

If the second value is omitted, the first value is assigned to both properties. It's useful for super quick multi-directional centering, i.e. `place-items: center;` will center both horizontally and vertically.

#### `justify-content`

The `justify-content` property controls the alignment of the entire grid itself along the inline (row) axis. It will only have an effect if there's room in the grid container for the grid to move around.

It can be set to one of six values:

- `start`
- `end`
- `center`
- `stretch`
- `space-around`
- `space-between`
- `space-evenly`

`justify-content: start` will align the grid to the start edge of the grid container.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-start.svg)

`justify-content: end` will align the grid to the end edge of the grid container.

![End](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-end.svg)

`justify-content: center` will align the grid in the center of the grid container.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-center.svg)

`justify-content: stretch` will stretch the grid to fill the grid container.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-stretch.svg)

`justify-content: space-around` will place an even amount of space between each grid column, with half-sized spaces on the edges.

![Space Around](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-around.svg)

`justify-content: space-between` will place an even amount of space between each grid column, with no space at the edges.

![Space Between](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-between.svg)

`justify-content: space-evenly` will place an even amount of space between each grid column, including the edges.

![Space Evenly](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-evenly.svg)

#### `align-content`

The `align-content` property controls the alignment of the entire grid itself along the block (column) axis. It will only have an effect if there's room in the grid container for the grid to move around.

It can be set to one of six values:

- `start`
- `end`
- `center`
- `stretch`
- `space-around`
- `space-between`
- `space-evenly`

`align-content: start` will align the grid to the start edge (row) of the grid container.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/align-content-start.svg)

`align-content: end` will align the grid to the end edge (row) of the grid container.

![End](https://css-tricks.com/wp-content/uploads/2018/11/align-content-end.svg)

`align-content: center` will align the grid in the center of the grid container.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/align-content-center.svg)

`align-content: stretch` will stretch the grid to fill the grid container.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/align-content-stretch.svg)

`align-content: space-around` will place an even amount of space between each grid row, with half-sized spaces on the edges.

![Space Around](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-around.svg)

`align-content: space-between` will place an even amount of space between each grid row, with no space at the edges.

![Space Between](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-between.svg)

`align-content: space-evenly` will place an even amount of space between each grid row, including the edges.

![Space Evenly](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-evenly.svg)

Basically, `align-content` will align the content of the grid itself, while `align-self` will align individual grid items within the grid.

#### `place-content`

The `place-content` property is a shorthand for `align-content` and `justify-content`.

The first value is the `align-content` value, and the second value is the `justify-content` value.

Its syntax is as follows:

- `place-content: <align-content> / <justify-content>`

If the second value is omitted, the first value is assigned to both properties.

It's important to understand the differences between `place-items` and `place-content`.

`place-items` controls the alignment of grid items within the grid cells
`place-content` controls the alignment of the entire grid within the grid container.

So, something like `place-items: center` will center the grid items within the grid cells, while `place-content: center` will center the entire grid within the grid container.

If you're doing something like the entire body or section on a page, you'll probably want to use `place-content`.

If you're doing something like a gallery of images, you'll probably want to use `place-items`.

#### `grid-auto-columns` and `grid-auto-rows`

The `grid-auto-columns` and `grid-auto-rows` properties control the size of columns and rows that are created implicitly on the grid.

Implicit tracks get created when there are more grid items than cells in the grid or when a grid item is placed outside of the explicit grid.

In other words, if you have 4 grid items, but only 3 columns, the 4th grid item will be placed in a new row that is created implicitly.

This is particularly useful when you have a dynamic number of columns. Rather than using `grid-template-columns` to define a fixed number of columns, you can use `grid-auto-columns` to define the size of columns that are created implicitly.

```css
.parent {
	display: grid;
	grid-auto-columns: 1fr; /* even columns */
}
```

#### `grid-auto-flow`

The `grid-auto-flow` property controls how the auto-placement algorithm works, specifying exactly how auto-placed items, or ones not placed explicitly, get flowed into the grid.

It can be set to one of two values:

- `row` (default)
- `column`

`grid-auto-flow: row` will place items by filling each row in turn, adding new rows as necessary.

This is why a new row gets created when there are more grid items to be placed than there are number of columns.

`grid-auto-flow: column` will place items by filling each column in turn, adding new columns as necessary.

Make sure you understand the logic here. For example, to create equal, implicit columns, very similar to Flexbox, you can do the following:

```css
.parent {
	display: grid;
	grid-auto-flow: column;
}
```

Now, it will create columns depending on the number of grid items. If there are 4 grid items, it will create 4 columns.

However, depending on the content, the columns may not be equal. If you want equal columns, just use the `grid-auto-columns` property.

```css
.parent {
	display: grid;
	grid-auto-flow: column;
	grid-auto-columns: 1fr;
}
```

Let's look at a more complicated example to solifidy the concept.

Consider the following HTML and CSS:

```html
<section class="container">
	<div class="item-a">item-a</div>
	<div class="item-b">item-b</div>
	<div class="item-c">item-c</div>
	<div class="item-d">item-d</div>
	<div class="item-e">item-e</div>
</section>
```

```css
.container {
	display: grid;
	grid-template-columns: repeat(5, 60px);
	grid-template-rows: repeat(2, 30px);
	grid-auto-flow: row; /* default */
}
```

If we specify the placing of items on the grid like so:

```css
.item-a {
	grid-column: 1;
	grid-row: 1 / 3;
}

.item-b {
	grid-column: 5;
	grid-row: 1 / 3;
}
```

But we don't specify the placement of the other items, they will be placed implicitly.

Look at the image below:

![Auto Flow Row](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-flow-01.svg)

But if we change the `grid-auto-flow` property to `column`, the way implicit items are placed will change and flow down the columns.

![Auto Flow Column](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-flow-02.svg)

### Child Properties

#### `grid-column-start` and `grid-column-end`

The `grid-column-start` and `grid-column-end` properties specify the columns (or more accurately, column line numbers) where a grid item should start and end.

The syntax is as follows:

- `grid-column-start: <number> | span <number> | auto`
- `grid-column-end: <number> | span <number> | auto`

For example, if we want to place an item starting at the second column line and ending at the fourth column line, we can do the following:

```css
.item {
	grid-column-start: 2;
	grid-column-end: 4;
}
```

Equivalently, we can use the `span` keyword:

```css
.item {
	grid-column-start: 2;
	grid-column-end: span 2;
}
```

If no value is specified for `grid-column-end`, it will default to `auto`, which means the item will span one column.

Items can also overlap each other. You can use `z-index` to control the stacking order.

It's important to note that these properties aren't used often. Instead, we use the `grid-column` shorthand property.

#### `grid-row-start` and `grid-row-end`

The `grid-row-start` and `grid-row-end` properties specify the rows (or more accurately, row line numbers) where a grid item should start and end.

The syntax is as follows:

- `grid-row-start: <number> | span <number> | auto`
- `grid-row-end: <number> | span <number> | auto`

It's essentially equivalent to the `grid-column-start` and `grid-column-end` properties, but for rows.

As said, it's not used often. Instead, we use the `grid-row` shorthand property.

#### `grid-column` and `grid-row`

The `grid-column` and `grid-row` properties are shorthand properties for `grid-column-start` and `grid-column-end`, and `grid-row-start` and `grid-row-end` respectively.

They are used to specify a grid item's size and location within the grid by referencing the grid lines.

The syntax is as follows:

- `grid-column: <start-line> / <end-line> | <start-line> / span <value>`
- `grid-row: <start-line> / <end-line> | <start-line> / span <value>`

For example, if we want to place an item starting at the second column line and span 2 columns, we can do either of the following:

```css
.item {
	grid-column: 2 / span 2;
}
```

```css
.item {
	grid-column: 2 / 4;
}
```

Same goes for `grid-row`, but for rows. If we omit the `end-line`, it will default to `auto`, which means the item will span one column or row.

For example, the two following code snippets are equivalent:

```css
.item {
	grid-column: 4 / 5;
}
```

```css
.item {
	grid-column: 4;
}
```

Also, if we omit the `start-line`, it will default to wherever the item currently is in the grid. So commonly, if you want a grid item to span two columns from wherever it currently is, you can just do `grid-column: span 2`.

Finally, we can use the line number `-1` to refer to the last line of the grid. This is because grid line numbers can also be used in reverse.

So, for example, if we want an item to span the entire row, we can do the following with line numbers:

```css
.item {
	grid-column: 1 / -1;
}
```

This is because this tells the item to start at the first column line, and end at the last column line, or in other words, span the entire row.

#### `grid-area`

The `grid-area` property gives an item a name so that it can be referenced by a template created with the `grid-template-areas` property.

The syntax is as follows:

- `grid-area: <name>`

For example, if we want to give an item the name `header`, we can do the following:

```css
.item {
	grid-area: header;
}
```

Now, we can reference it using the `grid-template-areas` property. For further information, see [`grid-template-areas`](#grid-template-areas) for syntax, or [Grid Areas](#grid-areas) for practical use.

#### `justify-self`

The `justify-self` property aligns a grid item inside a cell along the inline (row) axis. It applies to a grid item inside a single cell. It is like the `justify-content` property, but for individual grid items, and takes the same four values:

- `start`
- `end`
- `center`
- `stretch` (default)

`justify-self: start` will align the item with the start edge of its cell.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-start.svg)

`justify-self: end` will align the item with the end edge of its cell.

![End](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-end.svg)

`justify-self: center` will align the item in the center of its cell.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-center.svg)

`justify-self: stretch` will have the item fill the entire width of its cell.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-stretch.svg)

#### `align-self`

The `align-self` property aligns a grid item inside a cell along the block (column) axis. It applies to a grid item inside a single cell. It is like the `align-items` property, but for individual grid items, and takes the same four values:

- `start`
- `end`
- `center`
- `stretch` (default)

`align-self: start` will align the item with the start edge (row) of its cell.

![Start](https://css-tricks.com/wp-content/uploads/2018/11/align-self-start.svg)

`align-self: end` will align the item with the end edge (row) of its cell.

![End](https://css-tricks.com/wp-content/uploads/2018/11/align-self-end.svg)

`align-self: center` will align the item in the center of its cell.

![Center](https://css-tricks.com/wp-content/uploads/2018/11/align-self-center.svg)

`align-self: stretch` will have the item fill the entire height of its cell.

![Stretch](https://css-tricks.com/wp-content/uploads/2018/11/align-self-stretch.svg)

#### `place-self`

The `place-self` property is a shorthand for `align-self` and `justify-self`.

The first value is the `align-self` value, and the second value is the `justify-self` value.

Its syntax is as follows:

- `place-self: <align-self> <justify-self>`

If the second value is omitted, the first value is assigned to both properties. It's useful for super quick multi-directional alignment:

`place-self: center` for centering the item in its cell

![Center](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center.svg)

`place-self: center stretch` for centering the item in its cell and stretching it to fill the entire width of the cell

![Center Stretch](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center-stretch.svg)

## Special Units & Functions

### `fr`

The `fr` unit is a fractional unit and it allows us to distribute space in the grid container according to the fraction specified. It esentially means "free space".

So, a declaration like `grid-template-columns: 1fr 3fr` loosely means 25% of the space for the first column and 75% for the second column.

For even columns, we can use `grid-template-columns: 1fr 1fr 1fr` to distribute the space evenly between three columns.

### Sizing Keywords

As mentioned, when sizing columns and rows, you can use all the usual CSS units like `px`, `em`, `rem`, `%`, etc. However, there are also some handy keywords that can be used:

- `min-content`
- `max-content`
- `auto`

`min-content` will size the column or row to fit the longest word or item.
`max-content` will size the column or row to fit the entire content.
`auto` will size the column or row to fit the content automatically.

### Sizing Functions

There are also some handy functions that can be used to size columns and rows:

- `fit-content()`
- `minmax()`

`fit-content()` will size the column or row using the space available, but never less than `min-content` and never more than `max-content`.

`minmax()` sets a minimum and maximum size for a column or row.

This is very useful when combined with the `fr` unit. Consider the following CSS:

```css
grid-template-columns: minmax(100px, 1fr) 3fr;
```

What this does is specify the smallest the first column can be is `100px` and the largest it can be is `1fr`. So, CSS Grid will only shrink this column as much as you specify. The second column will take up the remaining space.

You can also use the `min()` and `max()` functions to achieve the same thing.

### The `repeat()` Function

The `repeat()` function is a function that repeats the same pattern for a specified number of times. It can be used to repeat a pattern of columns or rows.

Consider the following CSS:

```css
grid-template-columns: minmax(10px, 1fr) minmax(10px, 1fr) minmax(10px, 1fr);
```

This will create four columns, each with a minimum width of `10px` and a maximum width of `1fr`. We can use the `repeat()` function to achieve the same thing:

```css
grid-template-columns: repeat(4, minmax(10px, 1fr));
```

But its greater power comes when combining it with `auto-fill` or `auto-fit`.

#### `auto-fill`

The `auto-fill` keyword tells the browser to fill as many possible columns as possible on a row, even if they are empty.

Consider the following CSS:

```css
grid-template-columns: repeat(auto-fill, 100px);
```

This will create as many columns of `100px` as possible on a row.

#### `auto-fit`

The `auto-fit` keyword tells the browser to fit whatever columns there are into the space and to prefer expanding columns to fill space rather than have any empty columns.

\***\*This is revolutionary for creating responsive grids.\*\***

As explained with the `grid-auto-flow` property, we can set the following CSS to create dynamic columns depending on the number of children:

```css
.container {
	display: grid;
	grid-auto-flow: column;
}
```

But what we've lost here is wrapping. It would be nice if the number of columns were based on how many elements could fit without breaking the width of the container.

We can achieve this with the `auto-fit` keyword!

```css
.container {
	display: grid;
	grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

Now, the columns will expand to fill the space and wrap when the space is no longer available, i.e. if it hits the `200px` minimum.

This can be even more powerful when combined with `minmax()`

```css
grid-template-columns: repeat(auto-fill, minmax(min(10rem, 100%), 1fr));
```

This is saying that if `100%` width is less than `10rem`, then use `10rem`, otherwise use `1fr`, which makes it safe to use on smaller screens.

## Even Columns

To achieve even columns with CSS Grid, we can use the `grid-template-columns` property along with the `fr` unit.

The `fr` unit is a fractional unit that divides up the available space. For example, if we have two columns with `grid-template-columns: 1fr 1fr`, they will each take up half of the available space.

If we have three columns with `grid-template-columns: 1fr 1fr 1fr`, they will each take up a third of the available space. And so on.

Consider the following HTML:

```html
<div class="parent">
	<div class="child">Child 1</div>
	<div class="child">Child 2</div>
	<div class="child">Child 3</div>
</div>
```

We can create three equal columns, one for each child with the following CSS:

```css
.parent {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
}
```

Notice how we used the `repeat()` function to repeat the `1fr` value three times.

But there's actually a better way we can do this using `grid-auto-flow`.

What if we are working with a dynamic number of columns? Or what if we are working with `grid-template-areas` and don't want to define the specific number of columns?

We can change our CSS to the following:

```css
.parent {
	display: grid;
	grid-auto-flow: column;
	grid-auto-columns: 1fr;
}
```

Now, all our items will automatically be placed into columns, and each column will be equal width.

`grid-auto-flow` implicitly creates the columns.
`grid-auto-columns` sets the width of the columns to be `1fr`, or equal width.

One thing to keep in mind is there is no wrapping with this method. If there are more items than can fit in the container, they will overflow.

So, make sure you're using this within a media query, or see [Responsive Grids](#responsive-grids) for more information.

## Don't Declare Grid Rows

With CSS Grid, there is what is known as explicit and implicit grid tracks. Basically, when we create columns using `grid-template-columns`, we are creating explicit grid tracks.

But what happens if we have 4 items in a grid container with 3 columns?

Consider the following HTML:

```html
<div class="parent">
	<div class="child">Child 1</div>
	<div class="child">Child 2</div>
	<div class="child">Child 3</div>
	<div class="child">Child 4</div>
</div>
```

And the following CSS:

```css
.parent {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
}
```

In this case, Child 4 will actually be placed in the first column of a new row, or second row. This is because CSS Grid uses an implicit grid, and notices that there's more content but no room for it to live.

Due to this behavior, except for edge cases and complex layouts, it's better to not declare grid rows, as doing so incorrectly can lead to unexpected behavior.

One more thing to keep in mind is \***\*implicit grid rows will match the height of the tallest item in the row\*\***. This is usually what we want but it can be adjusted with the `grid-auto-rows` property.

## Don't Use Percentages

When creating a grid with CSS Grid, it's best to avoid using percentages for column widths.

This is because, more often, the `fr` unit is what you really want. Percentages are based on the width of the container, whereas `fr` is based on the available space.

Consider the following CSS:

```css
.grid {
	display: grid;
	grid-template-columns: 50% 50%;
	gap: 1rem;
}
```

This is going to cause horizontal overflow or scrolling because the `50%` width is based on the width of the container, not the available space.

In other words, you're giving each item a width of `50%` of the container, plus `1rem` for the gap, which is going to be more than `100%` of the container.

But if we use `fr` instead, we can avoid this issue because `fr` is based on the available space at any given time.

```css
.grid {
	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 1rem;
}
```

Things like `grid-template-columns: 75% 25%` for a sidebar layout should usually be `grid-template-columns: 3fr 1fr` instead.

If you really want to use a percentage, combine it with the `fr` unit.

```css
grid-template-columns: 75% 1fr;
```

Now, the first column will be 75% of the width of the container, and the second column will be the remaining space, regardless of any gaps.

## Spanning Grid Items

With CSS Grid, we can span grid items across multiple columns and rows.

Consider the following HTML:

```html
<div class="parent">
	<div class="child">Child 1</div>
	<div class="child">Child 2</div>
	<div class="child">Child 3</div>
	<div class="child">Child 4</div>
</div>
```

We can create a grid with 3 columns with the following CSS:

```css
.parent {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
}
```

From here, what if we want Child 1 to span across all three columns? We can achieve this with the `grid-column` property.

The `grid-column` property is a shorthand for `grid-column-start` and `grid-column-end`.

Its syntax is as follows:

- `grid-column: <start-line> / <end-line>`
- `grid-column: <start-line> / span <number>`

The `start-line` and `end-line` values can be line numbers or named lines. For example:

`grid-column: 1 / 3` would span from the first line to the third line.
`grid-column: 1 / span 2` would start from the first line and span two lines.

You can also omit the `start-line` value and it will default to wherever the grid item currently is.

So, `grid-column: span 2` would span two lines from the column the grid item is currently in.

Also, you can use negative values to count backwards from the end of the grid. This is useful for spanning across an entire row. For example:

`grid-column: 1 / -1` would span from the first line to the last line, or the entire row.

When you want a very specific position, it's best to combine `grid-column` with `grid-row` to specify the exact position of the grid item.

For example, to position a grid item from the 3rd to 4th column, and span both rows, we could use the following:

```css
.child {
	grid-column: 4 / 5;
	grid-row: 1 / 3;
}
```

Notice that the `grid-column` value is `4 / 5` because we want it to start on the 4th line number and end on the 5th line number.

We can also simply omit the `end-line` value since it's only spanning one column.

```css
.child {
	grid-column: 4;
	grid-row: 1 / 3;
}
```

Make sure you understand the syntax and concept here. For example, we could also change the `grid-row` value to `1 / span 2` and it would have the same effect.

## Grid Areas

With CSS Grid, we can also position grid items using grid areas. It takes a little more setup, but is very powerful, and can be easier to understand.

Consider the following HTML:

```html
<div class="parent">
	<div class="child">Child 1</div>
	<div class="child">Child 2</div>
	<div class="child">Child 3</div>
	<div class="child">Child 4</div>
	<div class="child">Child 5</div>
</div>
```

To begin defining grid areas, we need to create a grid container as usual.

```css
.parent {
	display: grid;
	gap: 1.5rem;
}
```

From there, assuming we are doing mobile-first, we can define our grid areas for mobile.

```css
.parent {
	display: grid;
	gap: 1.5rem;
	grid-template-areas:
		'one'
		'two'
		'three'
		'four'
		'five';
}
```

This just says that, on mobile, we want all our children stacking on top of each other.

But for this to work, we need to assign each child a grid area. We can do this with the `grid-area` property.

```css
.parent:nth-child(1) {
	grid-area: one;
}

.parent:nth-child(2) {
	grid-area: two;
}

.parent:nth-child(3) {
	grid-area: three;
}

.parent:nth-child(4) {
	grid-area: four;
}

.parent:nth-child(5) {
	grid-area: five;
}
```

Notice that when you define the areas on the parent, you use quotes, but when you assign the areas to the children, you do not use quotes.

Now, it's as simple as using media queries to change the grid areas for larger screens.

```css
@media (width >= 30em) {
	.testimonial-grid {
		grid-template-areas:
			'one one'
			'two five'
			'three five'
			'four four';
	}
}

@media (width >= 50em) {
	.testimonial-grid {
		grid-template-areas:
			'one one two five'
			'three four four five';
	}
}
```

Each string represents a row, and each space represents a column.

## `place-content` vs `place-items`

The `place-content` and `place-items` properties are two of the most useful properties in CSS Grid, specifically for centering or aligning content.

The `place-content` property is a shorthand for `align-content` and `justify-content`.
The `place-items` property is a shorthand for `align-items` and `justify-items`.

But there's a key difference between the two. First thing to understand is that if you only have a single grid item, `place-content` and `place-items` will have the exact same effect.

However, if you have multiple grid items, the properties do different things.

`place-content` will align the entire grid itself within the grid container. `place-items` will align the grid items within their corresponding grid cells.

So, one thing to remember is that `place-content: center` will only work if their is space in the grid container for the grid to be centered.

## Stacking Content

One of the most powerful features of CSS Grid is the ability to stack content.

Let's consider the following HTML:

```html
<div class="hero">
	<div class="hero__content">
		<!-- hero content stuff -->
	</div>
	<img src="img" alt="img" class="hero__img" />
</div>
```

We can utilize CSS Grid to make the image act as if it's a background image, but still have the image be accessible to screen readers.

```css
.hero {
	display: grid;
	text-align: center;
	place-items: center; /* center content */
}

.hero > * {
	grid-column: 1 / 2;
	grid-row: 1 / 2;
}

.hero__img {
	aspect-ratio: 16 / 9;
	object-fit: cover;
	z-index: -1; /* make sure image is behind the content */
}
```

Notice how we don't need a `position` property to use `z-index`. This is because CSS Grid automatically creates a new stacking context.

An alternative solution to this is using `grid-template-areas`.

```css
.hero {
	display: grid;
	grid-template-areas: 'stack';
	text-align: center;
	place-items: center; /* center content */
}

.hero > * {
	grid-area: stack;
}
```

This isn't as flexible if we want to change the layout for larger screens, but it's a good solution if you know your layout won't change.

## Responsive Grids

One of the most powerful features of CSS Grid is how easy it is to create responsive grids.

To do so, we want to utilize the implicit grid. In other words, we don't want to define the number of columns (or rows, but that's obvious).

We want to use the `repeat()` function, combined with `auto-fit` or `auto-fill`, as well as the `minmax()` function.

The `repeat()` function takes two arguments: the number of times to repeat, and the size of each item.

But we don't want to define the number of times to repeat, so we can use `auto-fit` or `auto-fill` as the first argument.

`auto-fit` will fit the columns you've defined into the available space.
`auto-fill` will fill the available space with as many columns as possible, even if they are empty.

\***\*Almost always, you'll want to use `auto-fit`.\*\***

From there, we define the minimum and maximum size of each column using the `minmax()` function.

For example, if we want our columns to be at least 15rem wide, but no more than 1fr, we could use the following:

```css
grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
```

Typically, the maximum size will be `1fr`, but you can use any unit you want.

By doing this, the grid will automatically adjust the number of columns based on the available space, creating a responsive grid.

### Overflow

One thing that's a downside with `minmax()` is that it will cause overflow at smaller screen sizes.

You may not know this because as long as your minimum size is smaller than the screen size, it will work fine.

But let's consider the following CSS:

```css
grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
```

Here, our minimum size is a very large number. So, at screen sizes smaller than 500px, we will have overflow or horizontal scrolling.

To fix this, we can utilize the `min()` function.

```css
grid-template-columns: repeat(auto-fit, minmax(min(500px, 100%), 1fr));
```

Now, if the screen is ever smaller than our minimum size, it will just use 100% of the available space.

### Max Column Count

One thing to note is that, by default, responsive grids will add as many columns as possible. This can be a problem if you want to limit the number of columns.

To combat this, we can use a hacky solution involving `minmax()`, `min()`, `max()`, and `auto-fit`.

```css
.grid {
	--idealSize: 6.25rem;
	--gap: 0.5rem;
	--columns: 3;

	display: grid;
	grid-template-columns: repeat(
		auto-fit,
		minmax(
			min(max(100% / var(--columns) - var(--gap), var(--idealSize)), 100%),
			1fr
		)
	);
	gap: var(--gap);
}
```

This will be responsive like before, and stretch items to fill the space if there is extra space, but will never add more than 3 columns. Granted it's a bit hacky, so feel free to refer to this code snippet if you need it.

## Sticky Footers

One of the most common problems in web development is creating a sticky footer. This is a footer that sticks to the bottom of the page, even if there isn't enough content to fill the page.

Typically, this is achieved using `position: fixed` or `position: sticky`, but this can cause problems with overlapping content.

Instead, we can use CSS Grid to create a sticky footer.

```html
<body>
	<header></header>
	<main>...</main>
	<footer></footer>
</body>
```

Now, we can use the following CSS:

```css
body {
	display: grid;
	grid-template-rows: auto 1fr auto;
	min-height: 100vh;
}
```

Now, the footer will always be at the bottom of the page, even if there isn't enough content to fill the page.

And, even with lots of content and vertical scrolling, the footer will still be at the bottom of the page.

If you don't want to do this on the `body` element, you can do it on a wrapper element that contains all of the content.

One more thing to keep in mind is `100vh` can have issues on mobile devices. So, you may want to use `100dvh` instead, but this isn't supported in all browsers.
