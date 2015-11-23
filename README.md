# ELR JavaScript Style Guide

Adapted from:

+ [http://javascript.crockford.com/code.html](http://javascript.crockford.com/code.html)
+ [https://github.com/styleguide/javascript](https://github.com/styleguide/javascript)

The long-term value of software to an organization is in direct proportion to the quality of the codebase. Over its lifetime, a program will be handled by many pairs of hands and eyes. If a program is able to clearly communicate its structure and characteristics, it is less likely that it will break when modified in the never-too-distant future.

Code conventions can help in reducing the brittleness of programs.

Neatness counts.

+ Use soft-tabs with a four space indent.
+ Always use camelCase, never underscores.
+ Use implicit parentheses when possible.
+ Any top level objects should be namespaced under the ELR namespace.
+ Don't ever use $.get or $.post. Instead use $.ajax and provide both a success handler and an error handler.
+ Use
        $.fn.on
+ instead of
        $.fn.bind
        $.fn.delegate
        $.fn.live.

### JavaScript Files

+ JavaScript programs should be stored in and delivered as .js files.
+ JavaScript code should not be embedded in HTML files unless the code is specific to a single session. Code in HTML adds significantly to pageweight with no opportunity for mitigation by caching and compression.
+ script tags should be placed as late in the body as possible. This reduces the effects of delays imposed by script loading on other page components. There is no need to use the language or type attributes. It is the server, not the script tag, that determines the MIME type.
+ Live websites should have minified, concatenated js files only
+ Use cdn for third party js whenever possible
+ Filenames
    + Filenames should be all lowercase in order to avoid confusion on case-sensitive platforms. Filenames should end in .js, and should contain no punctuation except for -.

### File Structure
+ js (compiled js)
    + assets (authored js)
    + vendor (all third party js)
    + *final compiled, and concatenated js file goes here (project-name.min.js)

### Indentation

+ The unit of indentation is four spaces. Use of tabs should be avoided because (as of this writing in the 21st Century) there still is not a standard for the placement of tabstops. The use of spaces can produce a larger filesize, but the size is not significant over local networks, and the difference is eliminated by minification.

### Line Length

+ Avoid lines longer than 80 characters. When a statement will not fit on a single line, it may be necessary to break it.
+ Place the break after an operator, ideally after a comma.
+ A break after an operator decreases the likelihood that a copy-paste error will be masked by semicolon insertion. The next line should be indented 8 spaces.

### Comments

+ Be generous with comments. It is useful to leave information that will be read at a later time by people (possibly yourself) who will need to understand what you have done.
+ The comments should be well-written and clear, just like the code they are annotating. An occasional nugget of humor might be appreciated. Frustrations and resentments will not.
+ It is important that comments be kept up-to-date. Erroneous comments can make programs even harder to read and understand.
+ Make comments meaningful. Focus on what is not immediately visible. Don't waste the reader's time with stuff like
        i = 0; // Set i to zero.
+ Generally use line comments. Save block comments for formal documentation and for commenting out.

### Selectors

+ Try to prefix all javascript-based selectors with js-.
+ This is taken from slightly obtrusive javascript.
+ The idea is that you should be able to tell a presentational class from a functional class.
+ Most of the codebase doesn't do this, let's try and move toward it.

### Naming

+ In general, use functionNamesLikeThis, variableNamesLikeThis, ClassNamesLikeThis, EnumNamesLikeThis, methodNamesLikeThis, CONSTANT_VALUES_LIKE_THIS, foo.namespaceNamesLikeThis.bar, and filenames-like-this.js.

+ Namespaces
    + JavaScript has no inherent packaging or namespacing support.
    + Global name conflicts are difficult to debug, and can cause intractable problems when two projects try to integrate. In order to make it possible to share common JavaScript code, we've adopted conventions to prevent collisions.
    + Use namespaces for global code
    + ALWAYS prefix identifiers in the global scope with a unique pseudo namespace related to the project or library. If you are working on "Project Sloth", a reasonable pseudo namespace would be sloth.*.
            var sloth = {};

            sloth.sleep = function() {
              ...
            };

### Variable Declarations

+ All variables should be declared before used. JavaScript does not require this, but doing so makes the program easier to read and makes it easier to detect undeclared variables that may become implied globals.
+ The var statements should be the first statements in the function body.
+ It is preferred that each variable be given its own line.
+ If necessary define them when they are declared. These should be listed first.
+ Undefined variables should be listed last.

        var currentEntry = 0;
        var level = 1;
        var size;

+ JavaScript does not have block scope, so defining variables in blocks can confuse programmers who are experienced with other C family languages.
+ Define all variables at the top of the function.
+ Use of global variables should be minimized. Implied global variables should never be used.

### Always Use Semicolons

+ JavaScript requires statements to end with a semicolon, except when it thinks it can safely infer their existence. In each of these examples, a function declaration or object or array literal is used inside a statement. The closing brackets are not enough to signal the end of the statement. Javascript never ends a statement if the next token is an infix or bracket operator.

+ Clarification: Semicolons and functions

    + Semicolons should be included at the end of function expressions, but not at the end of function declarations. The distinction is best illustrated with an example:

            var foo = function() {
              return true;
            };  // semicolon here.

            function foo() {
              return true;
            }  // no semicolon here.

### Function Declarations

+ All functions should be declared before they are used. Inner functions should follow the var statement. This helps make it clear what variables are included in its scope.
+ There should be no space between the name of a function and the ( (left parenthesis) of its parameter list.
+ There should be one space between the ) (right parenthesis) and the { (left curly brace) that begins the statement body.
+ The body itself is indented four spaces.
+ The } (right curly brace) is aligned with the line containing the beginning of the declaration of the function.

        function outer(c, d) {
            var e = c * d;

            function inner(a, b) {
                return (e * a) + b;
            }

            return inner(0, 1);
        }

+ This convention works well with JavaScript because in JavaScript, functions and object literals can be placed anywhere that an expression is allowed. It provides the best readability with inline functions and complex structures.

        function getElementsByClassName(className) {
            var results = [];
            walkTheDOM(document.body, function (node) {
                var a;                  // array of class names
                var c = node.className; // the node's classname
                var i;                  // loop counter

            // If the node has a class name, then split it into a list of simple names.
            // If any of them match the requested name, then append the node to the set of results.

                if (c) {
                    a = c.split(' ');
                    for (i = 0; i < a.length; i += 1) {
                        if (a[i] === className) {
                            results.push(node);
                            break;
                        }
                    }
                }
            });

            return results;
        }

+ Use of global functions should be minimized.

+ When a function is to be invoked immediately, the entire invocation expression should be wrapped in parens so that it is clear that the value being produced is the result of the function and not the function itself.

        var collection = (function () {
            var keys = [], values = [];

            return {
                get: function (key) {
                    var at = keys.indexOf(key);
                    if (at >= 0) {
                        return values[at];
                    }
                },
                set: function (key, value) {
                    var at = keys.indexOf(key);
                    if (at < 0) {
                        at = keys.length;
                    }
                    keys[at] = key;
                    values[at] = value;
                },
                remove: function (key) {
                    var at = keys.indexOf(key);
                    if (at >= 0) {
                        keys.splice(at, 1);
                        values.splice(at, 1);
                    }
                }
            };
        })();

+ Nested Funtions

    + Nested functions can be very useful, for example in the creation of continuations and for the task of hiding helper functions. Feel free to use them.

+ Function Declarations Within Blocks
    + Do not do this ever:

            if (x) {
              function foo() {}
            }

### for-in loop

+ Only for iterating over keys in an object/map/hash

### Associative Arrays

+ Never use Array as a map/hash/associative array

### Exceptions

+ You basically can't avoid exceptions if you're doing something non-trivial (using an application development framework, etc.). Go for it.

### Standards Features

+ Always preferred over non-standards features

### Multiline string literals

+ No
+ Do not do this:

        var myString = 'A rather long string of English text, an error message \
                        actually that just keeps going and going -- an error \
                        message to make the Energizer bunny blush (right through \
                        those Schwarzenegger shades)! Where was I? Oh yes, \
                        you\'ve got an error and all the extraneous whitespace is \
                        just gravy.  Have a nice day.';

+ The whitespace at the beginning of each line can't be safely stripped at compile time; whitespace after the slash will result in tricky errors; and while most script engines support this, it is not part of ECMAScript.

+ Use string concatenation instead:

        var myString = 'A rather long string of English text, an error message ' +
            'actually that just keeps going and going -- an error ' +
            'message to make the Energizer bunny blush (right through ' +
            'those Schwarzenegger shades)! Where was I? Oh yes, ' +
            'you\'ve got an error and all the extraneous whitespace is ' +
            'just gravy.  Have a nice day.';

### Array and Object literals

+ Use Array and Object literals instead of Array and Object constructors.
+ Array constructors are error-prone due to their arguments.
+ Object constructors don't have the same problems, but for readability and consistency object literals should be used.

### Modifying prototypes of builtin objects
+ No, never ever
+ Modifying builtins like Object.prototype and Array.prototype are strictly forbidden. Modifying other builtins like Function.prototype is less dangerous but still leads to hard to debug issues in production and should be avoided.

### Internet Explorer's Conditional Comments
+ No
+ Conditional Comments hinder automated tools as they can vary the JavaScript syntax tree at runtime.

### Wrapper Objects for Primitive Types
+ No
    + There's no reason to use wrapper objects for primitive types, plus they're dangerous.
    + Don't do it!

### Method and property definitions

        /** @constructor */ function SomeConstructor() { this.someProperty = 1; } Foo.prototype.someMethod = function() { ... };

+ While there are several ways to attach methods and properties to an object created via "new", the preferred style for methods is:

        Foo.prototype.bar = function() {
          /* ... */
        };

+ The preferred style for other properties is to initialize the field in the constructor:

        /** @constructor */
        function Foo() {
          this.bar = value;
        }

+ Why?
+ Current JavaScript engines optimize based on the "shape" of an object, adding a property to an object (including overriding a value set on the prototype) changes the shape and can degrade performance.

### delete

        Prefer this.foo = null.
         Foo.prototype.dispose = function() {
          this.property_ = null;
        };

+ Instead of:

        Foo.prototype.dispose = function() {
          delete this.property_;
        };

+ In modern JavaScript engines, changing the number of properties on an object is much slower than reassigning the values.
+ The delete keyword should be avoided except when it is necessary to remove a property from an object's iterated list of keys, or to change the result of if (key in obj).

### Closures

+ Yes, but be careful.
+ The ability to create closures is perhaps the most useful and often overlooked feature of JS.
+ One thing to keep in mind, however, is that a closure keeps a pointer to its enclosing scope.
+ As a result, attaching a closure to a DOM element can create a circular reference and thus, a memory leak.
+ For example, in the following code:

        function foo(element, a, b) {
          element.onclick = function() { /* uses a and b */ };
        }

+ the function closure keeps a reference to element, a, and b even if it never uses element.
+ Since element also keeps a reference to the closure, we have a cycle that won't be cleaned up by garbage collection.
+ In these situations, the code can be structured as follows:

        function foo(element, a, b) {
          element.onclick = bar(a, b);
        }

        function bar(a, b) {
          return function() { /* uses a and b */ };

### Names

+ Names should be formed from the 26 upper and lower case letters (A .. Z, a .. z), the 10 digits (0 .. 9).
+ Avoid use of international characters because they may not read well or be understood everywhere.
+ Do not use \ (backslash) in names.
+ Never start names with $ unless you are naming a jQuery object.
+ All jQuery object names should begin with $ to indicate that jQuery methods are available
+ Do not use _ (underscore) as the first character of a name.
    + It is sometimes used to indicate privacy, but it does not actually provide privacy.
    + Avoid conventions that demonstrate a lack of competence.
+ Most variables and functions should start with a lower case letter.
+ Constructor functions which must be used with the new prefix should start with a capital letter. JavaScript issues neither a compile-time warning nor a run-time warning if a required new is omitted. Bad things can happen if new is not used, so the capitalization convention is the only defense we have.
+ Avoid constructor functions that must be used with the new prefix if possible

### Statements

+ Simple Statements
    + Each line should contain at most one statement.
    + Put a ; (semicolon) at the end of every simple statement. Note that an assignment statement which is assigning a function literal or object literal is still an assignment statement and must end with a semicolon.
    + JavaScript allows any expression to be used as a statement. This can mask some errors, particularly in the presence of semicolon insertion. The only expressions that should be used as statements are assignments and invocations.

+ Compound Statements
    + Compound statements are statements that contain lists of statements enclosed in { } (curly braces).
    + The enclosed statements should be indented four more spaces.
    + The { (left curly brace) should be at the end of the line that begins the compound statement.
    + The } (right curly brace) should begin a line and be indented to align with the beginning of the line containing the matching { (left curly brace).
    + Braces should be used around all statements, even single statements, when they are part of a control structure, such as an if or for statement. This makes it easier to add statements without accidentally introducing bugs.

+ Labels
    + Statement labels are optional.
    + Only these statements should be labeled: while, do, for, switch.
    + return Statement
    + A return statement with a value should not use ( ) (parentheses) around the value. The return value expression must start on the same line as the return keyword in order to avoid semicolon insertion.

+ if Statement
    + The if class of statements should have the following form:

            if ( condition ) {
                statements
            }

            if ( condition ) {
                statements
            } else {
                statements
            }

            if ( condition ) {
                statements
            } else if ( condition ) {
                statements
            } else {
                statements
            }

+ for Statement
    + A for class of statements should have the following form:

            for (initialization; condition; update) {
                statements
            }

            for (variable in object) {
                if ( filter ) {
                    statements
                }
            }

    + The first form should be used with arrays and with loops of a predeterminable number of iterations.
    + The second form should be used with objects. Be aware that members that are added to the prototype of the object will be included in the enumeration. It is wise to program defensively by using the hasOwnProperty method to distinguish the true members of the object:

            for (variable in object) {
                if (object.hasOwnProperty(variable)) {
                    statements
                }
            }

+ while Statement
    + A while statement should have the following form:

            while (condition) {
                statements
            }

+ do Statement
    + A do statement should have the following form:

            do {
                statements
            } while (condition);

    + Unlike the other compound statements, the do statement always ends with a ; (semicolon).

+ switch Statement
    + don't use switch statements the are bug prone

+ try Statement
    + The try class of statements should have the following form:

            try {
                statements
            } catch (variable) {
                statements
            }

            try {
                statements
            } catch (variable) {
                statements
            } finally {
                statements
            }

+ continue Statement
    + Avoid use of the continue statement. It tends to obscure the control flow of the function.

+ with Statement
    + The with statement should not be used.

    + Using with clouds the semantics of your program. Because the object of the with can have properties that collide with local variables, it can drastically change the meaning of your program. For example, what does this do?

            with (foo) {
              var x = 3;
              return x;
            }

    + Answer: anything. The local variable x could be clobbered by a property of foo and perhaps it even has a setter, in which case assigning 3 could cause lots of other code to execute.

    + Don't use with.

### Whitespace

+ Blank lines improve readability by setting off sections of code that are logically related.
+ Blank spaces should be used in the following circumstances:
    + A keyword followed by ( (left parenthesis) should be separated by a space.

            while (true) {

    + A blank space should not be used between a function value and its ( (left parenthesis). This helps to distinguish between keywords and function invocations.
    + All binary operators except . (period) and ( (left parenthesis) and [ (left bracket) should be separated from their operands by a space.
    + No space should separate a unary operator and its operand except when the operator is a word such as typeof.
    + Each ; (semicolon) in the control part of a for statement should be followed with a space.
    + Whitespace should follow every , (comma).

### Bonus Suggestions

+ {} and []
    + Use {} instead of new Object().
    + Use [] instead of new Array().
    + Use arrays when the member names would be sequential integers. Use objects when the member names are arbitrary strings or names.

+ , (comma) Operator

    + Avoid the use of the comma operator except for very disciplined use in the control part of for statements. (This does not apply to the comma separator, which is used in object literals, array literals, var statements, and parameter lists.)

### Block Scope

+ In JavaScript blocks do not have scope. Only functions have scope. Do not use blocks except as required by the compound statements.

### Assignment Expressions
+ Avoid doing assignments in the condition part of if and while statements.
+ Is

        if (a = b) {

    a correct statement? Or was

        if (a == b) {

    intended? Avoid constructs that cannot easily be determined to be correct.

+ === and !== Operators.
    + It is almost always better to use the === and !== operators.
    + The == and != operators do type coercion.
    + In particular, do not use == to compare against falsy values.

### Confusing Pluses and Minuses
+ Be careful to not follow a + with + or ++.
    + This pattern can be confusing.
    + Insert parens between them to make your intention clear.

        total = subtotal + +myInput.value;

    + is better written as

            total = subtotal + (+myInput.value);

    + so that the + + is not misread as ++.

### eval is Evil
+ The eval function is the most misused feature of JavaScript. Avoid it.
+ eval has aliases.
    + Do not use the Function constructor.
    + Do not pass strings to setTimeout or setInterval.
+ eval() makes for confusing semantics and is dangerous to use if the string being eval()'d contains user input.
+ There's usually a better, clearer, and safer way to write your code, so its use is generally not permitted.

### Custom toString() methods
+ Must always succeed without side effects.

### Deferred initialization
+ OK
+ It isn't always possible to initialize variables at the point of declaration, so deferred initialization is fine.

### Explicit scope
+ Always
+ Always use explicit scope - doing so increases portability and clarity.
    + For example, don't rely on window being in the scope chain.
    + You might want to use your function in another application for which window is not the content window.

### Code formatting
+ We follow the C++ formatting rules in spirit, with the following additional clarifications.
+ Curly Braces
    + Because of implicit semicolon insertion, always start your curly braces on the same line as whatever they're opening. For example:

            if (something) {
                // ...
            } else {
                // ...
            }

+ Array and Object Initializers
    + Single-line array initializers are allowed when they fit on a line:

            var arr = [1, 2, 3];  // No space after [ or before ].

            // Object initializer.
            var inset = {
                top: 10,
                right: 20,
                bottom: 15,
                left: 12
            };

            // Array initializer.
            this.rows_ = [
                '"Slartibartfast" <fjordmaster@magrathea.com>',
                '"Zaphod Beeblebrox" <theprez@universe.gov>',
                '"Ford Prefect" <ford@theguide.com>',
                '"Arthur Dent" <has.no.tea@gmail.com>',
                'the.mice@magrathea.com'
            ];

            // Used in a method call.
            elr.dom.createDom(elr.dom.TagName.DIV, {
                id: 'foo',
                className: 'some-css-class',
                style: 'display:none'
            }, 'Hello, world!');

    + Long identifiers or values present problems for aligned initialization lists, so always prefer non-aligned initialization. For example:
        + Like this:

            CORRECT_Object.prototype = {
                a: 0,
                b: 1,
                lengthyName: 2
            };

        + Not like this:

            WRONG_Object.prototype = {
                a          : 0,
                b          : 1,
                lengthyName: 2
            };

+ Function Arguments
    + When possible, all function arguments should be listed on the same line.
    + If doing so would exceed the 80-column limit, the arguments must be line-wrapped in a readable way.
    + Put each argument on its own line to enhance readability.
    + The indentation should be Four-space, one argument per line.
    + Works with long function names,
    + survives renaming, and emphasizes each argument.

            elr.foo.bar.doThingThatIsVeryDifficultToExplain = function(
                veryDescriptiveArgumentNumberOne,
                veryDescriptiveArgumentTwo,
                tableModelEventHandlerProxy,
                artichokeDescriptorAdapterIterator) {
              // ...
            };

+ Passing Anonymous Functions
    + When declaring an anonymous function in the list of arguments for a function call, the body of the function is indented four spaces from the left edge of the statement, or four spaces from the left edge of the function keyword.

            prefix.something.reallyLongFunctionName('whatever', function(a1, a2) {
                if (a1.equals(a2)) {
                    someOtherLongFunctionName(a1);
                } else {
                    andNowForSomethingCompletelyDifferent(a2.parrot);
                }
            });

+ Blank lines
    + Use newlines to group logically related pieces of code. For example:

            doSomethingTo(x);
            doSomethingElseTo(x);
            andThen(x);

            nowDoSomethingWith(y);

            andNowWith(z);

+ Ternary Statements

    + Always put ternary statements on a single line. If it takes more than 80 columns use an if statement instead.

            var x = a ? b : c;

+ Dot Operator
    + Can be on multiple lines if this increases readability.

            var x = foo.bar().
                doSomething().
                doSomethingElse();

### Parentheses
    + Only where required
    + Use sparingly and in general only where required by the syntax and semantics.
    + Never use parentheses for unary operators such as delete, typeof and void or after keywords such as return, throw as well as others (case, in or new).

### Strings
+ Prefer ' over "
+ For consistency single-quotes (') are preferred to double-quotes ("). This is helpful when creating strings that include HTML:

        var msg = 'This is some HTML';

### Tips and Tricks

+ True and False Boolean Expressions

    + The following are all false in boolean expressions:

            null
            undefined
            '' the empty string
            0 the number
            But be careful, because these are all true:

            '0' the string
            [] the empty array
            {} the empty object

    + This means that instead of this:

            while (x != null) {

    + you can write this shorter code (as long as you don't expect x to be 0, or the empty string, or false):

            while (x) {

    + And if you want to check a string to see if it is null or empty, you could do this:

            if (y != null && y != '') {

    + But this is shorter and nicer:

            if (y) {

    + Caution: There are many unintuitive things about boolean expressions. Here are some of them:

            Boolean('0') == true
            '0' != true
            0 != null
            0 == []
            0 == false
            Boolean(null) == false
            null != true
            null != false
            Boolean(undefined) == false
            undefined != true
            undefined != false
            Boolean([]) == true
            [] != true
            [] == false
            Boolean({}) == true
            {} != true
            {} != false

+ Conditional (Ternary) Operator (?:)
    + Instead of this:

            if (val) {
                return foo();
            } else {
                return bar();
            }

    + you can write this:

            return val ? foo() : bar();

+ && and ||
    + These binary boolean operators are short-circuited, and evaluate to the last evaluated term.
    + "||" has been called the 'default' operator, because instead of writing this:

            function foo(opt_win) {
                var win;
                if (opt_win) {
                    win = opt_win;
                } else {
                    win = window;
                }
                // ...
            }

    + you can write this:

            function foo(opt_win) {
                var win = opt_win || window;
                // ...
            }

    + "&&" is also useful for shortening code. For instance, instead of this:

            if (node) {
                if (node.kids) {
                    if (node.kids[index]) {
                        foo(node.kids[index]);
                    }
                }
            }

    + you could do this:

            if (node && node.kids && node.kids[index]) {
                foo(node.kids[index]);
            }