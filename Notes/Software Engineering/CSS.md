2025-01-13 17:55

Status: #Leaf

Tags:

# CSS
Cascading Style Sheets (CSS) is used with [[HTML]] to style components with custom layouts, colors, and fonts. You can write CSS in the HTML file, using inline or block styling, but it's best to create a dedicated CSS file for each HTML file. 

CSS is based on two ideas:
- **Selectors** define an HTML element to be styled
- **Attributes** define how to style the selected HTML element
## Selectors
To define attributes for an HTML element we must first specify the HTML element to be styled, which we use **selectors** for. Selectors come in three flavors.
### Selector Type
**HTML element type**:  this "h1" selector will style all header elements
```HTML
<h1>
	Hello
</h1
```
```css
h1 {
	// Style here
}
```

**Class**: this ".secondary" selector will style all elements with the "secondary" class
```HTML
<h2 class="secondary">
	Welcome to my site
</h2>
```
```CSS
.secondary {
	// Style here
}
```

**ID**: This "main-text" selector will style all elements with the "main-text" id
```HTML
<p id="main-text">
	Here's the text
<\p>
```
```CSS
#main-text {
	// Style Here
}
```

### Chaining Selectors
We can "chain" selectors together to achieve more complex styling directions. It's best practice to keep selectors as simple as possible

**Multiple Selectors**: If we use a comma between selectors we will style both selectors
```CSS
h1, h2 {
	/* Style both h1 and h2 */*
}
```

**Nested Selectors**: If we use a space between selectors we will style the final element if it is inside the other selectors listed
``` css
h1 h2 {
	/* Style all h2's inside h1 */
}
```

Similarly we can use a carrot > to search one level deep for an element inside another element
```css
.hero > h2 {
	/* Style all h2's one level deep inside .hero class */*
}
```
## Attributes
Attributes are the actual styles applied to the selected elements. They are applied in the order listed within their CSS file. There are hundreds of different attributes, all with their own uses. A general list of CSS properties can be seen [here](https://www.w3schools.com/cssref/index.php) but they fall into two main categories: **color** and **layout**

In this example, the h1 elements will be styled first, then h2, etc. 
```CSS
h1 {
	color: black
}

h2 {
	color: blue
}

h3 {
	color: red
}
```
### Specificity
If two attributes conflict, then the selector with the greatest "specificity" is used. The more "specific" a selector is, then the higher priority its styling will be.  

The order of specificity is:
1) ID
2) Class
3) Element Type

In this example, we have a styling for "h1" and  ".main-header" in our CSS file. They both contain a color attribute, so we apply the color with the greatest specificity. class is of greater specificity than element type, so the element is colored purple
```HTML
<h1 class="main-header">
	Hello!
</h1>
```
```CSS
h1 {
	color: orange
}

.main-header {
	color: purple
}
```

### Color
The first main attribute type is color. We can either use a [color word](https://www.w3.org/wiki/CSS/Properties/color/keywords) or a more specific hex code (#FFFFFF for white). 
- Color: Set the text color
- Background-color: Set the background color

```CSS
.banner {
	color: white
	background-color: blue
}
```
## Layout - Box Model
HTML and CSS uses the **Box Model**, wherein all HTML elements are treated as a rectangular box consisting of four layers:
- **Content**: The innermost section where text/images and other content is displayed
	- This is where the text in a `<div>` elem would be displayed
- **Padding**: An empty space (non-styled) between the content and the border to prevent them merging together
	- The background color of the element also applies to the padding
- **Border**: Layer meant to surround the padding, or content if there is no padding
	- Can be styled with width, color, etc. 
- **Margin**: The outermost layer, in between this element and its neighbors. It is not styled and does not affect the box itself but rather determines the distance between this element and its neighbors
	- Similar to padding, but the space between this element and its neighbors instead of the content and the border

![[Pasted image 20250113184019.png]]

A basic box model would look like the following: 
```css
.banner {
	background-color: blue;

	width: 100px;
	height: 50%;
	padding: 50%;
	border: 1px solid black;
	margin: 10px;
} 
```

We can even view this inside chrome element inspector: 
![[Pasted image 20250113184254.png]]
#### Width and Height Syntax
The width and height properties determine the size of the content area. It is best practice to use percentage units rather than pixels as it provides more flexibility for different screen sizes. 

Percentage units adapt to the size of the parent container while pixel units are static. This means that percentage units can adapt to the size of the screen, and will be displayed the same no matter what. The static nature of pixels can result in odd behavior where the website can appear so large it doesn't fit on the screen for a mobile device, but look zoomed out and small when viewed on a large desktop. 
#### Padding and Margin Syntax
Both padding and margin have between one and four possible inputs. Depending on the number of inputs, the behavior changes:
- One value (`padding: 10px`): Set the padding to this value on all sides
- Two values (`padding: 10px 15px`): Set the padding to 10px on top and bottom, then 15px on left and right
- Four values (`padding: 10px, 15px, 20px, 25px`): Set padding for all sides
	- `padding: top, right, bottom, left`

You can also set the padding for each side explicitly using properties like `padding-top`, `padding-right` etc.
##### REM Units
While pixels are typical for padding, we can also use REM (Relative MEasurement) units. REM scales to the base font size of the element, so if the font size increases our spacing will scale proportionally. This is useful for more responsive designs
#### Border Syntax
Border has three parameters:
- Size: The size of the border (typically in pixels)
- Type: The type of border, almost always `solid` but sometimes `dashed`, `dotted`, etc. 
- Color: The color of the border

```css
.hero {
	border: 1px solid black
}
```
### Layout - Display
We can use the `display` property to define the layout of our elements relative to each other. It determines the flow of the document and how elements are positioned relative to each other. For example, how will these two `<div>` elements be displayed relative to eachother?
```HTML
<div>
	<div class="first">
		Hello
	<\div>
	<div class="second">
		There
	</div>
</div
```

**block**: The element will start on a new line and stretch to fill the width of the container. Other elements will be pushed downwards to a new line
- `display: block;`
- ![[Pasted image 20250113190725.png]]

**inline**: The element will be displayed on the same line, with its width set only as wide as its content
- `display: inline;`
- ![[Pasted image 20250113194006.png]]

**inline-block**: The element will be displayed inline, but we can use block properties like `margin`
```css
.first, .second {
	display: inline-block;
	margin: 10px 0;
}
```


## Layout - Grid


## References
CSS in 5 minutes: https://www.youtube.com/watch?v=Z4pCqK-V_Wo