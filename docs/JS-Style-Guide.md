# JSHint Errors #
Never commit with JSHint warnings. All variables should be defined, unused declarations should be deleted and globals should be avoided where possible and declared using `/* global foo */` otherwise. If the use of things like `document.write` or `new Function(…)` cannot be avoided, wrap them in `/*jshint -Wxx*/ … /*jshint +Wxx*/` to suppress the warning.

# Indentation #
Use one tab for indentation. When breaking a statement into multiple lines, prefer to break immediately after `(`, `{` or `[`:
```javascript
var bar = foo(
    first, second, thirdArgument
);
````

This is to avoid situations like this:
```javascript
var bar = foo(first, second,
              thirdArgument);
```
where you must align a line to a specific point on the previous line (using spaces or tabs). When that is unavoidable, use [Smart tabs](http://www.emacswiki.org/SmartTabs), where you first put the same number of tabs as the previous line, then add spaces to align.

# Punctuation #
* Use single quotes for string literals.
* Always end statements with semicolons.
* Never put spaces or line breaks before `,`, `:`, `?` or `;`
* Never put spaces on either side of `.`.
* Always add spaces on either side for `=` and comparison operators.
* Prefer adding spaces on either side of arithmetic operators when space is available. Modulate the use of spaces to show grouping.
* Add a space before `(` and `[` in function or array declarations, but not in function calls, array/object lookups or conditional and looping blocks.
* Don’t add spaces after `(`, `[` or `{`, or before `)`, `]` or `}`.
* When breaking long lines, always break after an operator.

# Conditionals and Loops #
* The basic principle is that it should be possible to insert a statement on any line without changing the behavior of existing statements of 
* Single-line conditionals and loops that entirely fit on a single line are okay.
 * Except when it is an `if` with an `else` clause.
* Otherwise, use braces.

Examples:
```javascript
if(err) throw err; // Okay

if(err)
    throw err; // Not okay.

if(connected) socket.send(data);
else queue(data);  // Not okay.

if(connected) {
  socket.send(data);
} else queue(data); // Okay
```

# Naming #
* Be consistent, predictable and boring.
* Be as short as possible but no shorter.
* Ordinary variable names and property names should be camelCased singular nouns.
* Treat abbreviations as normal words, e.g. `escapedHtml`, not `escapedHTML`.
* Prefer variable names that indicate the specific type or class of the expected value:
 * `user` or `oldUser` for a User object
 * `userId` for an ID sting
 * `userIdent` for an Identity string
 * `userAction` for a User Action object
* Integer-indexed arrays should be named with a plural, e.g. `users` for an array of User objects.
* Hashmaps should be named like `usersById` or `connectionsByRoom`.
* Ordinary function names should be camelCased verbs, e.g. `getTexts` or `render`.
* Constructor names should be PascalCased singular nouns, e.g. `WebSocket`.
 * When the class is a collection object, use the type of collection (List, Set, Map) rather than a plural. e.g. `UserList` rather than `Users`.
* Constants should be UPPERCASE_WITH_UNDERSCORES.
* File and module names should be lower-cased-with-hyphens.

# Objects #
* For singletons, use an object literal.
* For object classes that do not have methods (i.e. simple JSON objects or structs), prefer direct object literals or builder functions like `makeTextAction()`.
* For object classes that have methods, use constructors (`new IrcConnection()`) and add methods to it’s `prototype` property.

# Functions #
* Prefer writing named functions as `function foo () {}` rather than `foo = function () {};`, except when they are defined inside an object or array literal.
* Declare all local variables in a single `var` at the top of the function.
* This should be followed by any inner functions, and then the function body.
* Functions that are immediately executed (to create a closure) should be wrapped in parentheses.
* If the entire file is wrapped in an immediately executing function, the body should not be indented.