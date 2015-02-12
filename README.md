# Craftsy JavaScript Style Guide (WIP)

*Forked from the [Airbnb Style Guide](https://github.com/airbnb/javascript). Thank you for sharing, Airbnb!*


## Table of Contents

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [Chat With Us About Javascript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)


## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8.
    [More info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    var superman = {
        default: {clark: 'kent'},
        private: true
    };

    // good
    var superman = {
        defaults: {clark: 'kent'},
        hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var superman = {
        class: 'alien'
    };

    // bad
    var superman = {
        klass: 'alien'
    };

    // good
    var superman = {
        type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Arrays

  - Use the literal syntax for array creation.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
        itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
        var args = Array.prototype.slice.call(arguments);
        ...
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 120 characters should be written across multiple lines using string concatenation.

  - Note: If overused, long strings with concatenation could impact performance.
    [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about \
    how Batman had anything to do with this, you would get nowhere fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about ' +
        'how Batman had anything to do with this, you would get nowhere fast.';
    ```

**[⬆ back to top](#table-of-contents)**


## Functions

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
        return true;
    };

    // named function expression
    function named() {
        return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
        console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead.
    Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

  - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement.
    [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
        function test() {
            console.log('Nope.');
        }
    }

    // good
    var test;
    if (currentUser) {
        test = function test() {
            console.log('Yup.');
        };
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every
    function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
        // ...stuff...
    }

    // good
    function yup(name, options, args) {
        // ...stuff...
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Properties

  - Use dot notation when accessing properties.

    ```javascript
    var luke = {
        jedi: true,
        age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
        jedi: true,
        age: 28
    };

    function getProp(prop) {
        return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

  - Also use subscript notation when the property key contains special characters.

    ```javascript
    // bad
    var bestDoctor = doctors.tenth-doctor;

    // good
    var bestDoctor = doctors['tenth-doctor'];
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the
    global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration per variable.
    It's easier to add new variable declarations this way, and you never have
    to worry about swapping out a `;` for a `,` or introducing punctuation-only
    diffs.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables first. This is helpful when later on you might need to assign a variable depending on
    one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var i;
    var length;
    var dragonball;
    var items = getItems();
    var goSportsTeam = true;
    ```

**[⬆ back to top](#table-of-contents)**


## Conditional Expressions & Equality

  - Use `===` and `!==` over `==` and `!=`.

  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple
    rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
        // true
        // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
        // ...stuff...
    }

    // good
    if (name) {
        // ...stuff...
    }

    // bad
    if (collection.length > 0) {
        // ...stuff...
    }

    // good
    if (collection.length) {
        // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

  - To improve readability, use braces for all blocks. Do not omit braces for single-line blocks.

    ```javascript
    // bad
    if (test)
        return false;

    // bad
    if (test) return false;

    // good
    if (test) {
        return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
        return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Comments

  - Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and
    return values.

    ```javascript
    // bad

    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

        // ...stuff...

        return element;
    }

    // good

    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

        // ...stuff...

        return element;
    }
    ```

  - Use `//` for single line comments. For comments on the same line as code, use two spaces to separate them. Put an
    empty line before comments that are on their own line.

    ```javascript
    // bad
    var active = true; // is current tab

    // good
    var active = true;  // is current tab

    // bad
    function getType() {
        console.log('fetching type...');
        // set the default type to 'no type'
        var type = this._type || 'no type';

        return type;
    }

    // good
    function getType() {
        console.log('fetching type...');

        // set the default type to 'no type'
        var type = this._type || 'no type';

        return type;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - Use soft tabs set to 4 spaces.

    ```javascript
    // bad
    function() {
    ∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
        console.log('test');
    }

    // good
    function test() {
        console.log('test');
    }

    // bad
    dog.set('attr',{
        age: '1 year',
        breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
        age: '1 year',
        breed: 'Bernese Mountain Dog'
    });
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
        // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
        // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
        // ...stuff...
    })(this);↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which emphasizes that the line is a method call,
    not a new statement.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
        find('selected').
            highlight().
            end().
        find('.open').
            updateCount();

    // good
    $('#items')
        .find('.selected')
            .highlight()
            .end()
        .find('.open')
            .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
            .data(data)
        .enter().append('svg:svg')
            .class('led', true)
            .attr('width',  (radius + margin) * 2)
        .append('svg:g')
            .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
            .call(tron.led);
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
        return bar;
    }
    return baz;

    // good
    if (foo) {
        return bar;
    }

    return baz

    // bad
    var obj = {
        foo: function() {
          ...
        },
        bar: function() {
          ...
        }
    };
    return obj;

    // good
    var obj = {
        foo: function() {
          ...
        },

        bar: function() {
          ...
        }
    };

    return obj;
    ```

**[⬆ back to top](#table-of-contents)**


## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var story = [
          once
        , upon
        , aTime
    ];

    // good
    var story = [
        once,
        upon,
        aTime
    ];

    // bad
    var hero = {
          firstName: 'Bob'
        , lastName: 'Parr'
        , heroName: 'Mr. Incredible'
        , superPower: 'strength'
    };

    // good
    var hero = {
        firstName: 'Bob',
        lastName: 'Parr',
        heroName: 'Mr. Incredible',
        superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some
    implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in
    ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of
    the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted
    this.

    ```javascript
    // bad
    var hero = {
        firstName: 'Kevin',
        lastName: 'Flynn',
    };

    var heroes = [
        'Batman',
        'Superman',
    ];

    // good
    var hero = {
        firstName: 'Kevin',
        lastName: 'Flynn'
    };

    var heroes = [
        'Batman',
        'Superman'
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
        var name = 'Skywalker'
        return name
    })()

    // good
    (function() {
        var name = 'Skywalker';
        return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
        var name = 'Skywalker';
        return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for
    [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're
    doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as
    [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer
    ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32
    bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
        // ...stuff...
    }

    // good
    function query() {
        // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
        name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
        name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user(options) {
        this.name = options.name;
    }

    var bad = new user({
        name: 'nope'
    });

    // good
    function User(options) {
        this.name = options.name;
    }

    var good = new User({
        name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - Don't save a reference to `this` (such as `self` or `_this`). Use `bind` to explicitly set the context.

    ```javascript
    // bad
    function() {
        var self = this;
        return function() {
            console.log(self);
        };
    }

    // bad
    function() {
        var that = this;
        return function() {
            console.log(that);
        };
    }

    // bad
    function() {
        var _this = this;
        return function() {
            console.log(_this);
        };
    }

    // good
    function() {
        return function() {
            console.log(this);
        }.bind(this);
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Accessors

  - Accessor functions for properties are not required.

  - If you do make accessor functions use getVal() and setVal('hello').

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal().

    ```javascript
    // bad
    if (!dragon.age()) {
        return false;
    }

    // good
    if (!dragon.hasAge()) {
        return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
        options || (options = {});
        var lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
        this[key] = val;
    };

    Jedi.prototype.get = function(key) {
        return this[key];
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
        console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
        fight: function fight() {
            console.log('fighting');
        },

        block: function block() {
            console.log('blocking');
        }
    };

    // good
    Jedi.prototype.fight = function fight() {
        console.log('fighting');
    };

    Jedi.prototype.block = function block() {
        console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
        this.jumping = true;
        return true;
    };

    Jedi.prototype.setHeight = function(height) {
        this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function() {
        this.jumping = true;
        return this;
    };

    Jedi.prototype.setHeight = function(height) {
        this.height = height;
        return this;
    };

    var luke = new Jedi();

    luke.jump()
        .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
        options || (options = {});
        this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
        return this.name;
    };

    Jedi.prototype.toString = function toString() {
        return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass
    a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without
    finding and updating every handler for the event. For example, instead of:

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // good
    $(this).trigger('listingUpdated', {listingId : listing.id});

    ...

    $(this).on('listingUpdated', function(e, data) {
        // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**


## Modules

  - Use [Require.js](http://requirejs.org/) to define and require modules.

  - Use one file per class. The file should be named with PascalCase, and the name should match the class being
    exported.

  - When there are only a few dependencies, include them all on a single line.

    ```javascript
    // bad
    define(
        ['path/to/Dependency'],
        function(Dependency) {
            ...
        }
    );

    // good
    define(['path/to/Dependency'], function(Dependency) {
        ...
    });

  - When there are many dependencies, split them into multiple lines inside the array literal. For the callback
    function, splitting the arguments into multiple lines is optional based on the readability.

    ```javascript
    // bad
    define(
        ['path/to/Dependency',
         'path/to/OtherDependency',
         'path/to/Library',
         'path/to/Tool'],
        function(Dependency, OtherDependency, Library, Tool) {

            ...

        }
    );

    // good
    define([
        'path/to/Dependency',
        'path/to/OtherDependency',
        'path/to/Library',
        'path/to/Tool'
    ], function(Dependency, OtherDependency, Library, Tool) {

        ...

    });

    // good
    define([
        'path/to/Dependency',
        'path/to/OtherDependency',
        'path/to/Library',
        'path/to/OtherLibrary',
        'path/to/Tool',
        'path/to/OtherTool'
    ], function(
        Dependency,
        OtherDependency,
        Library,
        OtherLibrary
        Tool,
        OtherTool
    ) {

        ...

    });


**[⬆ back to top](#table-of-contents)**


## jQuery

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
        $('.sidebar').hide();

        // ...stuff...

        $('.sidebar').css({
            'background-color': 'pink'
        });
    }

    // good
    function setSidebar() {
        var $sidebar = $('.sidebar');
        $sidebar.hide();

        // ...stuff...

        $sidebar.css({
            'background-color': 'pink'
        });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`.
    [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5
    [compatibility table](http://kangax.github.com/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**


## Chat With Us About JavaScript

  - Find Craftsy's JavaScript developers on Freenode at [#denverscript](irc://chat.freenode.net/denverscript).

## Contributors

  - Craftsy's version of this style guide was a joint effort between the members of our Front End guild.

  - [View Airbnb's Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
