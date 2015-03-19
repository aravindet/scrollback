# Introduction
The purpose of this guide is to provide a guidelines to follow when making artwork and design for Scrollback. Following the guidelines are important to provide a consistent brand image.

## Fonts
We use the Open Sans font for our designs. The weight of the font is 400 for body text. The base font size is 14px.

[Open Sans in Google fonts](https://www.google.com/fonts/specimen/Open+Sans)

### Letter case: 
Scrollback is a single word and a proper noun. The ‘S’ is always uppercase. The ‘b’ is always lowercase.

## Colors
We use a *#33ccaa* as our accent color.

    Hex triplet - #3ca
    RGB Decimal - rgb(51, 204, 170)
    RGB Percent - rgb(20%, 80%, 66.7%)
    CMYK - cmyk(75, 0, 17, 20)
    HSL - hsl(166.7°, 60%, 50%)
    HSV - hsv(166.7°, 75%, 80%)

## Logo

The Scrollback logos can be found [here](https://drive.google.com/folderview?id=0BzKyJbUtt_IaM0U0RjJDZ2paUkU&usp=sharing).

The logo usage will depend on the following:

| Background 	|           	| Characteristics 	|                	|
|------------	|-----------	|-----------------	|----------------	|
| Brightness 	| Colorful? 	| Text color      	| Up arrow color 	|
| Very light 	| No        	| Black           	| Green          	|
| Very light 	| Yes       	| Black           	| Black          	|
| Light      	| Any       	| Black           	| Black          	|
| Dark       	| Any       	| White           	| White          	|
| Very dark  	| No        	| White           	| Green          	|
| Very dark  	| Yes       	| White           	| White             	|

## Icons and other imagery
`// TODO`

## Copy-writing guidelines
`// TODO`


## User Interface
The UI widgets are building blocks of the application. They have a consistent look and feel across the application.

### Goals
1. Provide a consistent look and feel
2. Take advantage of user's previous experience by familiar looking elements

### Size and spacing
1. The widgets should be touch friendly, with a touch target of minimum **49px**.
2. The widgets on a page should have a consistent spacing, possibly **1em**.

## Working with stylesheets
We use SASS preprocessor for writing our stylesheets. While working with the stylesheets, following conventions should be followed,

### Coding style
* Use 4 spaces indent as spaces are the only way to guarantee code renders the same in any environment.
* Put spaces before { in rule declarations.
* Put line breaks between rulesets.
* Place closing braces of declaration blocks on a new line.
* All property declarations should be followed by a semicolon (;).
* Each declaration should appear on its own line.
* In case of a selector with a  single property, the whole block should be written in one line.

### Formatting
* Use hex color codes (e.g. - #000) unless using rgba().
* Use `/* */` for comment blocks since SASS strips out the comments with `//`. If the comments are SASS specific, use `//`.
* Avoid specifying units for zero values, e.g., `margin: 0;` instead of `margin: 0px;`.
* Avoid unnecessary nesting in SASS. Follow the selector guidelines for a better approach.

### Selector conventions
* Class names should preferably be used for selectors.
* Avoid use of IDs as CSS selector as it has a very high specificity and cannot be overridden.
* For things like `header`, `footer` etc, use the HTML5 tags instead of ID.

### Classname conventions
* Classnames should always be lowercase.
* The segments in the classname should be separated by a hyphen (-), don't use camelcase. e.g. (use `chat-item` instead of `chatItem`).
* The classname should be determined by it's role and position in the document. The following approach is preferred - `appname-component-subcomponent`.
* In case of modifiers, no prefix is needed, e.g. `.settings-label.current`, `.room-item.unread`.