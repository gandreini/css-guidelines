#CSS Style Guide


## 1. Introduction

To keep the CSS code written for the project consistent and maintainable we provide CSS guidelines to follow during implementation and maintenance. 
The aim of these guidelines is to speed up development, make it easy for new developers to start working on the project, keep the code as it was written by a single developer, keep files short and readable.

### 1.1	Preprocessor

The CSS code will be written using <a href="https://sass-lang.com/" target="_blank">**SASS preprocessor**</a> with SCSS syntax. These guidelines also applies for projects based on **LESS** or **vanilla CSS**.

### 1.2	Gulp

SASS will be compiled with a **Gulp task**. This allows all developers working on the same project, to compile SASS using the same tool and avoid having developers using different compilers that could have some pre-built mixins not available to others. <a href="https://blog.teamtreehouse.com/use-npm-task-runner" target="_blank">NPM task runner</a> or <a href="https://gruntjs.com/" target="_blank">Grunt</a> can also be used. Gulp allows to perform all the other tasks needed to manage the project.

More info about how to use Gulp.js to automate CSS tasks <a href="https://www.sitepoint.com/automate-css-tasks-gulp/" target="_blank">here</a>.

### 1.3	Tabs and spaces

For indentation and formatting of the code **we don’t use tabs but spaces**.

Please set up your code editor with these settings:

 - **no tabs**;
 - **tab size = 4**;
 - **indent size = 4**.

Spaces are the only way to guarantee code renders the same in any person's environment.

### 1.4	Anatomy of a Ruleset

Before we discuss how we write our rulesets, let’s first familiarise ourselves with the relevant terminology:

    [selector] {
        [<--declaration--->]
        [property]: [value];
    }

### 1.5	HTML inline styles

**Never** write inline style in the HTML code.
Inline styles generate critical **<a href="http://www.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/" target="_blank">specificity problems</a>**
that might be very difficult to fix and impose the usage of **!important**.


### 1.6	Temporary css

Sometimes is necessary to write a CSS declaration very fast and it might not be possible to compile the SASS files.
**It is mandatory not to edit the style.css compiled by SASS: these modifications will be lost the next time a developer compiles the CSS**.

For this reason we create a static **temporary.css** file that is linked in the HTML right after the style.css: in this file is possible to write CSS declarations when it is not possible to compile the SASS file. This must be used only for critical situations and urgent bug fixings. Then the declarations in this file must be moved to the SASS files: the temporary CSS file must be empty most of the time.

### 1.7 Avoid undoing/overriding

**You're undoing CSS when you are writing declarations that override other declarations written somewhere else in the code.** This means that HTML elements are affected by too many conflicting CSS declarations and you'll need to rely on specificity to style correctly the element.

Undoing is a bad way of coding and we must always minimise undoing from above levels. This typically happens in large projects where we start with a CSS framework (i.e. Bootstrap) and then we realize we are writing a lot of declarations to overwite the default framework's style (see 2.6 External frameworks (like Bootstrap)).

### 1.8	80 characters wide

Where possible, limit CSS files width to 80 characters. Reasons for this include:

  - the ability to have multiple files open side by side;
  - viewing CSS on sites like GitHub, or in terminal windows;
  - providing a comfortable line length for comments.

### 1.9 Style Lint

SASS files should be verified with the linting tool <a href="http://stylelint.io/" target="_blank">Style Lint</a>. Style linting must be set up as a task in the Gulp file and run whenever a SASS file is modified. More info on how to set up style linting with Gulp <a href="http://www.creativenightly.com/2016/02/How-to-lint-your-css-with-stylelint/" target="_blank">here</a>.

Our stylelint file is available for download <a href="http://www.giulioandreini.it/resources/stylelintrc" target="_blank">here</a>.

### 1.10 spacing-unit

All margins and paddings must be expressed using the variable:

`$spacing-unit: 10px;`

The base value is `10px` but you can also set the variable to your own base value (e.g. `8px`). If you need different values you can multiply the variable:

`margin-bottom: $spacing-unit*2;`

It is recommended to multiply `$spacing-unit` by integers or multiples of 0.5.

The benefits of using this variable are:

- faster CSS coding;
- a more coherent spacing of elements;
- changing the value of `$spacing-unit` of a few pixel, you can change the overall look and feel of you UI in seconds.

### 1.11	HTML 5

HTML 5 tags must be used when writing HTML markup. These semantic elements clearly describe their meaning to both the browser and the developer.

---

## 2. SASS file organization

File organization, file naming and class naming are strictly related and very important to easily manage the style of the project.
Our general approach to manage CSS and the whole project is based on Modular CSS on which you can find more informations <a href="https://spaceninja.com/2018/09/17/what-is-modular-css/?utm_source=CSS-Weekly&utm_campaign=Issue-332&utm_medium=email" target="_blank">here</a>. We also adopt the Inverted Triangle CSS (ITCSS) philosophy, in a slightly simplified version. More info about ITCSS <a href="https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/" target="_blank">here</a>.

SASS files are divided in 5 main folders that organize them in a logical structure:

**Generic, Global, Components, Layouts, Utilities**

### 2.1 Generic

Files that don't directly style elements in the HTML: **color-scheme**, **variables**, **mixins**.

Examples of **mixins** include:

A mixin to keep floating elements consistent, without using *overflow: hidden;*:

	@mixin consistent {
	    &:after {
	        content: ' ';
	        clear: both;
	        display: block;
	    }
	}

A mixin to use icon fonts:

	@mixin icon-font($icon) {
	    content: $icon;
	    font-family: 'icon-font';
	    speak: none;
	    font-style: normal;
	    font-weight: normal;
	    font-variant: normal;
	    text-transform: none;
	    line-height: 1;
	    -webkit-font-smoothing: antialiased;
	    -moz-osx-font-smoothing: grayscale;
	}

### 2.2 Global

Common elements that style HTML tags. **No use of classes in here**, only HTML tags. Some examples: **normalize**, **typography**, **forms**, **tables**.

### 2.3 Components

Styles for components. What is a component? **A component is a distinct, independent unit**, that can be combined with other components to form a more complex structure. **Components are built to be reusable** in different contexts, can be nested one into the other or directly inserted into layouts. Components' style is also **indipendent** of the parent elements that wraps them.

Sometimes components are referred as **modules**.

### 2.4 Layouts

Layouts are the strucuture of each page and create the main areas where components will be placed. Due to the fact that layouts are just structures, we expect HTML and CSS to be very limited.

### 2.5 Utilities

Utilities and helper classes with ability to override the styles defined in Global, Components and Layouts. Utilities includes classes for **hidden elements**, **label and value** elements to show metedata, classes to set up a **sticky footer**.

### 2.6 External frameworks (like Bootstrap)

In some projects it could be useful to use a framework like Bootstrap of Foundation. You're highly discouraged to use this type of frameworks for complex projects since you'll spend a lot of time undoing styles (see 1.7 Avoid undoing/overriding).

If you use a framework, import it using a package manager (like NPM or Bower) and never modify the original files. Import the SASS files in the main **style.scss** file where you import all your files: select only the files you are really using (don't import the whole CSS).

Be aware that using the theming options of the framework (like bootstrap-theme of Bootstrap) will lead to having one more layer of styling for each element and you'll have to undo two levels of CSS style.

### 2.7 Color scheme

In any UI is a good habit to use a limited set of colors. Using too many colors can lead to have a very incoherent interface that confuses the user.

For this reason we have a **color-scheme.scss** file that contains the variables **of all the colors used in the project**. This is useful to control and limit the number of colors used: these should be less than 15, including all the possible shades of gray used for borders and backgrounds.

The file should be imported before the **variables.scss**.

Using other colors not declared into **color-scheme.scss** is not allowed.

### 2.8 File organization, file naming and class naming

TODO

### 2.9 Style.scss file

Outside the folders **Generic, Global, Components, Layouts, Utilities** we create the **style.scss** file which is responsible for importing all the needed scss files (that will be compiled by SASS).

The **style.scss** file starts with a comment where we write the name of the project and we explain what this file is responsible for:

	//
	// <NAME_OF_THE_PROJECT>
	//
	// Sass styles import for whole project
	//

The next section is where we import external libraries (e.g. Susy). This is how it looks like:

	// ------------------------------------ //
	// #LIBRARIES
	// ------------------------------------ //
	@import "jspm_packages/npm/hint.css@2.2.1/src/hint.scss";
	@import "jspm_packages/npm/susy@2.2.9/sass/susy";

Then we have 5 sections for **Generic, Global, Components, Layouts, Utilities** which are opened by a comment and followed by all the **import** instructions. **Imports are ordered in alphabetical order to make it easier to check if a file is imported or not**.

Here we provide a partial example of the **components** import section:

	// ------------------------------------ //
	// #COMPONENTS
	// ------------------------------------ //
	@import "components/boxview";
	@import "components/boxview-item";
	@import "components/buttons";
	@import "components/carousel";
	@import "components/document-reader";

The other sections (**Generic, Global, Layouts, Utilities**) will look the same.

---


## 3. Naming conventions

Naming conventions in CSS are useful in making your code consistent and more informative. Name something based on what it is, not how it looks or behaves.
Never use a class like **.red**: this will cause you problems the day you want that element to be green. Never use a class like **.left-bar**: this won't make sense the day you decide to move that bar on the right.

**Camel case must not be used for classes**.

### 3.1 Components and layouts

We use a simplified version of the <a href="http://getbem.com/" target="_blank">BEM guidelines</a>.

Components and layout classes are bulit using simple **hyphen `-` delimited strings**; their child elements have classes that are composed by the parent's class plus a custom part delimited using a **double underscore `__`**.

E.g. you'll have a component that has the class **item-preview** and a child element that has the class **item-preview__metadata**.

Example code:

	<article class="item-preview">

    	<div class="item-preview__thumb-caption-wrapper" >
        	<a class="item-preview__thumb" href="...">
            	<img class="item-preview__image">
		        <p class="item-preview__caption" >
		            Caption
        		</p>
		</div>
    
		<div class="item-preview__metadata">
			<a class="item-preview__title-link">
            	<h1 class="item-preview__title">
            		Title
            	</h1>
	        </a>
		    <h2 class="item-preview__subtitle">
        		Subtitle
        	</h2>
			<p class="item-preview__description">
        		Description
	        </p>
		</div>
		
	</article>

When you have many nested elements, always create a two levels class, using the root components class as the first part:

**component-root-class__child-or-grandchild-element-class**

We **never** rely on HTML tags as CSS selectors: this guarantees that if we change an HTML tag, the style of the page is not affected. For this reason, each HTML element must have a class.

### 3.2 Class prefix

In some projects, our CSS selectors could conflict with other stylesheets: this happens when we use a CSS framework or we are developing an application that must be embedded in other HTML pages. Let's say you have a **pagination component** and the root class is `pagination`. If this component is used in a page where Bootstrap or Foundation are also used, the framework's style could affect your component and viceversa (your CSS could affect the style of the framework's component).

In these cases it's a good habit to add a short **prefix** to all our classes to avoid any kind of conflict. Let's say we are working on a project called **MY PROJECT**, we will add a prefix **"mp-** to all the classes and the pagination component will be:

```
<div class="mp-pagination">
	...
</div>
```

Class prefixes should be short, we suggest the syntax: `xx-`.

### 3.3 Variables

For variables names we use the **hyphen `-` as string separator**. The string will be composed by parts ordered **hierarchically, starting from higher level and going into more detail**.

A couple of examples:

	@footer-margin-bottom: 20px;
	@pagination-border-top-width: 1px;

The name of the variable starts from a higher level and each following part of the string defines a more detailed level.

Variables that defines colors will start with **"color-"**:

    @color-text-link-hover: @color-main;
    @color-header-border-top: @color-border-light;

Variables must be grouped into logical groups that must be introduced by a comment like:

    // ------------------------------------ //
    //  COLORS
    // ------------------------------------ //

and variables whose name is not completely explaining its meaning must be preceded by a comment like this:

    // Sets the margin between the content and the footer
    @content-margin-bottom: $spacing-unit;

Sometimes it's be useful to define variables with values relative to other variables. This is clear with colors where we can have a main color for links and a hover color which is the **same color but lighter**:

    @color-text-link: #0071bc;
    @color-text-link-hover: lighten(@color-text-link, 25);

if we change the color of link we must **change only one value** and the second one is changed automatically.

---

## 4. States
States are used to style components that are in special momentary condition like **active**, **disabled** or **loading**.
In this case we don't follow the BEM guidelines but we **add** a standalone state class at the root element of the component. State classes start with the prefix **"is-"** or **"has-"**.

Some examples:

- `is-active`
- `has-loaded`
- `is-loading`
- `is-visible`
- `is-disabled`
- `is-expanded`
- `is-collapsed`

These classes must not have a style for theirselves but are always styled in the context of the component where they are used.

---

## 5. Grids
Grid styles must not be hardcoded as classes in HTML, like some frameworks do (e.g. Bootstrap). This can be fast to implement, but if you want to change the layout you'll have to modify the HTML. Moreover, with this solution, you don't have full flexibility when implementing grids for responsive layouts.

Instead, we use <a href="http://susy.oddbird.net/" target="_blank">Suzy</a> which is a minimal framework only for grids that relies a lot on native CSS properties and helps the transition to native CSS Grid Layout.

### 5.1 Responsive columns layout basics

Let's see how we can build a 4 columns layout and a responsive version (comments inline).

	/* 4 columns */
    .list-container { // Parent container
        > .list-container__item { // Child elements to be displayed in 4 columns		

	       float: left; // You can also use Flexbox
           width: span(3 of 12); // Susy
           margin-right: gutter(of 12); // Susy

	        &:nth-child(4n) {
               margin-right: 0; // Sets 0 right margin to each element in the last column
           }
           
           &:nth-child(4n + 1) {
               clear: both; // Clears the float for each first element in a new row
           }
        }
    }

The same layout can then be viewed as a 2 columns layout in a tablet (portrait).

	/* ------------------------------------ *\
	   #MEDIA-QUERIES
	\* ------------------------------------ */
	@media all and (max-width: $breakpoint-ipad-portrait) {
	
		/* 2 columns */
	    .list-container {
	        > .list-container__item { // Child elements to be displayed in 2 columns	
	
		        &:nth-child(1n) { // Needed for specificity
	                float: left;
	                clear: none;
	                width: span(6 of 12); // Susy
	                margin-right: gutter(of 12); // Susy
		        }
	
		        &:nth-child(2n) { // Susy
	               margin-right: 0; // Sets 0 right margin to each element in the last column
	           }
	           
	           &:nth-child(2n + 1) { // Susy
	               clear: both; // Clears the float for each first element in a new row
	           }
	        }
	    }
	
	}










---

## 6. Formatting

    [selector] {
        [property]: [value];
        [<--declaration--->]
    }

Here are some formatting guidelines:

  - a **space before** the opening brace **( { )**;
  - the opening brace **( { )** on the **same line as the last selector**;
  - the first declaration **on a new line after the opening brace** **( { )**;
  - **properties and values always on the same line**;
  - a **space after** the property–value delimiting colon **( : )**;
  - each **declaration on its own new line** (line breaks between declarations);
  - the **closing brace** **( } )** on its own **new line**;
  - each **declaration indented** by four (4) spaces.

Let’s see an example:

    .footer, .footer-bar {
        display: block;
        background-color: @color-page-background;
        color: @color-footer-text;
    }

Avoid specifying **units for zero values**:

    margin: 0; /* Good */

instead of

    margin: 0px; /* No good */

For nested ruleset we also leave a blank line before the nested ruleset. Here’s an example:

    .footer {
        color: @color-footer-text;

        .bar {
            color: @color-footer-bar-text;
        }
    }

Nesting in SASS should be avoided wherever possible and used only when necessary to have the desired specificity.

### 6.1 Whitespaces

As well as indentation, we can provide a lot of information through liberal
and judicious use of whitespace between rulesets.
We usually have one (1) empty line between rulesets and two (2) empty lines at
the end of each main section of code.

### 6.2 Colors

We prefer using hex color codes (`#f0f0f0`).
Short hex colors and color names (e.g. **red**) are not allowed.
Letters in the hex color must be lowercase.


### 6.3 Declarations order

Declarations inside a ruleset will be organized following this order:

1. **Box model:**

        display: block;
        float: right;
        box-sizing; border-box;
        width: 100px;
        height: 100px;
        margin: 0;
        padding: 0;

2. **Positioning:**

        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        z-index: 100;

3. **Visual** (Borders & Background):

        background-color: #f5f5f5;
        border: 1px solid #e5e5e5;
        border-radius: 3px;

4. **Typography:**

        font-weight: 100;
        font-size: 13px;
        font-family: Helvetica;
        line-height: 1.5;
        color: #333;
        text-align: center;
        text-transform: uppercase;

5. **Misc:**

        animations, transforms, etc.

This is not a strict rule (and won't be checked by StyleLint) but you're encouraged to follow this guideline.

---


## 7. Comments and code sections

CSS is not telling its own story very well and **it is a language that benefit from being heavily commented**.

As a rule, you should comment anything that isn’t immediately obvious from the code alone: there is no need to tell someone that **display: none;** will hide an element, but if you’re using **overflow: hidden;** to clear floats — as opposed to clipping an element’s overflow — this is probably something worth documenting.

In SASS we can use comments with two different ways:

    /* */

and

    //

The main difference is that the **first will be processed and included in the resulting .css file** while the second **will be ignored by the preprocessor**.
For this reason we must use the `/* */` syntax for comments we want to include in the .css file while we use `//` for comments we don’t want to be compiled.
This will make no difference when we compile a **minified** and no-comments version of the file for production, but while we’re developing we’ll probably have a not-minified CSS file and comments can be very useful.

A clear example is the **variables.scss**: this will be transparent in the compiled CSS file so it’s a good practice to insert all comments using `//` otherwise there will be comments floating around with no related code.

We identify **four** different types of commenting.

### 7.1 Document level comments

These comments are **mandatory** and must explain what the current SASS document is about: we place them at the beginning of each SASS file.

    /**
     * DOCUMENT TITLE
     *
     * Text describing in detail what this file is doing...
     */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    //
    // DOCUMENT TITLE (COMMENT HIDDEN FROM COMPILER, LIKE VARIABLES)
    //
    // Text describing in detail what this file is doing...
    //
        

### 7.2 Section level comments

The CSS declarations inside each file must be divided and organized in logical sections.
These sections must be introduced by a **mandatory** comment with a title of the section.
Only if the content is extremely hard to understand reading the code please provide some additional lines of text.

    /* ------------------------------------ *\
       #SECTION-TITLE
    \* ------------------------------------ */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    // ------------------------------------ //
    // #SECTION-TITLE
    // ------------------------------------ //

### 7.3 Selector level comments

This is not mandatory but needed when writing CSS code that perform
some strange or unexpected action. We need to use this comments when writing CSS related to the entire CSS selector that will be hard to understand week or months after.

This is a simple single line comment:

    /* This set of declarations is doing this and that */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    // This set of declarations is doing this and that

### 7.4 Inline comments

These are comments written at the level of each CSS declaration that we
need when writing hard-to-understand code. We write it like this:

    color: blue; /* The text color id blue */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    color: blue; // The text color id blue


---


## 8. Media Queries

Media queries **shouldn’t be written in a single file** but divided in each SASS file to define media queries behaviour for each specific component, section or layout.


---


## 9. Specificity

Keep **specificity low**.

There will always be times you need to override the style of an element, so the lower the specificity is on a selector, the easier it is to override. Not only that, but override in such a way you might even be able to override it again without going crazy with an **ID selector** or **!important**.

To keep specificity low:

- **limit nesting** in SCSS files. The rule of thumb for components and layouts is to have a two-level nesting.
- never use **!important**;
- never use **ID selectors**.

---

## 10. Sizing

Units and sizes must be set using pixels.

**Font-size** will be experessed in pixels too (why? <a href="https://hackernoon.com/rems-and-ems-and-why-you-probably-dont-need-them-664b9ce1e09f" target="_blank">read here</a>).

**Line-height**: unit-less **line-height** is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.


---


## 11. Resources


  - <a href="http://cssguidelin.es" target="_blank">High-level advice and guidelines for writing sane, manageable, scalable CSS</a>
  - <a href="http://codepen.io/chriscoyier/blog/codepens-css" target="_blank">CodePen's CSS</a>
  - <a href="http://blog.trello.com/refining-the-way-we-structure-our-css-at-trello/?utm_source=CSS-Weekly&utm_campaign=Issue-128&utm_medium=email" target="_blank">Refining The Way We Structure Our CSS At Trello</a>
  - <a href="http://geek-rocket.de/frontend-development/scss-styleguide-with-bem-oocss-smacss/" target="_blank">Scss-Styleguide with BEM, OOCSS & SMACSS</a>
  - <a href="https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06" target="_blank">Medium’s CSS is actually pretty f***ing good.</a>
  - <a href="https://github.com/evernote/sass-build-structure" target="_blank">Evernote SASS build structure</a>
  - <a href="http://maintainablecss.com/" target="_blank">Mantainable CSS</a>
