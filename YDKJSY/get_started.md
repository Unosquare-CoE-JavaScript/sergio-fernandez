# Get Started


JS is a compiled language, it's formatted into an AST, which then is compiled into binary
IC39 every year defines what will be part of the next ECMA version.
### Backwards & Forwards
Once something is accepted as valid JS, there will not be a future change tothe language that causes that code to become invalid JS.Code written in 1995—however primitive or limited it may have been!—should still work today.
### Transpile
If the syntax is not compatible then we have to transpile it. Those problems appear to be part of the forward compatibility issue. The most used tool for this is babel which converts from a newer syntax to an older one
### Interpretation
The comon definition for JS is that it is an interpretated (script) languaje.JSs ourcecode is parsed before it is executed,parsed JS is converted to an optimized (binary) form, and that “code” is subse-quently executed ,the engine does not commonly switch back into line-by-line execution mode after it has finished all the hard work of parsing—most languages/engines wouldn’t, because that would be highly inefficient.
### Web Assembly (WASM)
WASM is well known as the fastest version of JavaScript and can be processed by a JS engine and skip the parsing (which is nomally done by JS).

### Values  
Values come in two forms in JS: primitive and object.  
- Primitives: 
  * strings
  * numbers
  * booleans
  * null
  * undefined
  * symbol

- Objects: objects and arrays.

### Declaring and Using Variables
Variables have to be declared to be used, there are various syntax forms that declare variables and each form has different implied behaviors.  
```javascript
    var name = "Kyle";
    var age;
```
Let allows a more limited access to the variable than var this is called “block scoping”
```javascript
    var adult = true;
    if (adult) {
        var name = "Kyle";
        let age = 39;
        console.log("Shhh, this is a secret!");
    }
    console.log(name);
    // Kyle
    console.log(age);
    // Error!
```
Const must be given a value at the moment it’s declared, and cannot be re-assigned a different value later.
```javascript
    const myBirthday = true;
    let age = 39;
    if (myBirthday) {
        age = age + 1; // OK!
        myBirthday = false; // Error!
    }
```

## Functions
Collection of statements that can be invoked one or more times, may be provided some inputs, and may give back one or more outputs.  
Function declaration:
```javascript
    function awesomeFunction(coolThings) {
        // ..
        return amazingStuff;
    }
``` 

Function expression:
```javascript
    // let awesomeFunction = ..
    // const awesomeFunction = ..
    var awesomeFunction = function(coolThings) {
        // ..
        return amazingStuff;
    };
```
This function is an expression that is assigned to the variable awesomeFunction, function expression is not associated with its identifier
until that statement during runtime.  

You can only return a single value, but if you have more values to return, you can wrap them up into a single object/array.  

Since functions are values, they can be assigned as properties on objects:
```javascript
    var whatToSay = {
        greeting() {
            console.log("Hello!");
        },
        question() {
            console.log("What's your name?");
        },
        answer() {
            console.log("My name is Kyle.");
        }
    };

    whatToSay.greeting();
    // Hello!
```

## Comparisons  
Differences between an equality comparison and an equivalence comparison.  

== vs ===  

```javascript
    3 === 3.0; // true
    "yes" === "yes"; // true
    null === null; // true
    false === false; // true
    42 === "42"; // false
    "hello" === "Hello"; // false
    true === 1; // false
    0 === null; // false
    "" === null; // false
    null === undefined; // false
```
“Triple-equals” disallows any sort of type coercion. 

The === operator is designed to lie in two cases of special values: NaN and -0.
```javascript
    NaN === NaN; // false
    0 === -0; // true
```

In JS, all object values are held by reference, are assigned and passed by reference-copy, and are compared by reference (identity) equality.
Coercion means a value of one type being converted to its respective representation in another type (to whatever extent possible).
```javascript
    [ 1, 2, 3 ] === [ 1, 2, 3 ]; // false
    { a: 42 } === { a: 42 } // false
    (x => x * 2) === (x => x * 2) // false
    
    var x = [ 1, 2, 3 ];
    // assignment is by reference-copy, so
    // y references the *same* array as x,
    // not another copy of it.

    var y = x;
    y === x; // true
    y === [ 1, 2, 3 ]; // false
    x === [ 1, 2, 3 ]; // false
```



## How We Organize in JS

### Classes
A class in a program is a definition of a “type” of custom data structure that includes both data and behaviors that operate on that data. Classes define how such a data structure works, but classes are not themselves concrete values.  
```javascript
    class Page {
        constructor(text) {
            this.text = text;
        }
        print() {
            console.log(this.text);
        }
    }

    class Notebook {
        constructor() {
            this.pages = [];
        }
        addPage(text) {
            var page = new Page(text);
            this.pages.push(page);
        }
        print() {
            for (let page of this.pages) {
            page.print();
            }
        }
    }

    var mathNotes = new Notebook();
    mathNotes.addPage("Arithmetic: + - * / ...");
    mathNotes.addPage("Trigonometry: sin cos tan ...");
    mathNotes.print();
    // ..
```

### Class Inheritance

Aspect inherent to traditional “class-oriented” design
```javascript
    class Publication {
        constructor(title,author,pubDate) {
            this.title = title;
            this.author = author;
            this.pubDate = pubDate;
        }

        print() {
            console.log(`
                Title: ${ this.title }
                By: ${ this.author }
                ${ this.pubDate }
            `);
        }
    }

    class Book extends Publication {
        constructor(bookDetails) {
            super(
                bookDetails.title,
                bookDetails.author,
                bookDetails.publishedOn
            );
            this.publisher = bookDetails.publisher;
            this.ISBN = bookDetails.ISBN;
        }

        print() {
            super.print();
            console.log(`
                Publisher: ${ this.publisher }
                ISBN: ${ this.ISBN }
            `);
        }
    }

    class BlogPost extends Publication {
        constructor(title,author,pubDate,URL) {
            super(title,author,pubDate);
            this.URL=URL;
        }
        print() {
            super.print();
            console.log(this.URL);
            }
    }

    var YDKJS = new Book({
        title: "You Don't Know JS",
        author: "Kyle Simpson",
        publishedOn: "June 2014",
        publisher: "O'Reilly",
        ISBN: "123456-789"
    });

    YDKJS.print();
    // Title: You Don't Know JS
    // By: Kyle Simpson
    // June 2014
    // Publisher: O'Reilly
    // ISBN: 123456-789

    var forAgainstLet=newBlogPost(
        "For and against let",
        "Kyle Simpson",
        "October 27, 2014",
        "https://davidwalsh.name/for-and-against-let"
        )
    forAgainstLet.print();

```

### Modules
The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. Also like classes, modules can “include” or “access” the data and behaviors of other modules, for cooperation sake.
- Classic Modules
    - The key hallmarks of a classic module are an outer function (that runs at least once), which returns an “instance” of the module with one or more functions exposed that can operate on the module instance’s internal (hidden) data.
    ```javascript
        function Publication(title,author,pubDate) {
            var publicAPI = {
                print() {
                    console.log(`
                        Title: ${ title }
                        By: ${ author }
                        ${ pubDate }
                    `);
                }
            };
            return publicAPI;
        }
        
        function Book(bookDetails) {
            var pub = Publication(
                bookDetails.title,
                bookDetails.author,
                bookDetails.publishedOn
            );

            var publicAPI = {
                print() {
                    pub.print();
                    console.log(`
                        Publisher: ${ bookDetails.publisher }
                        ISBN: ${ bookDetails.ISBN }
                    `);
                }
            };

            return publicAPI;
        }

        function BlogPost(title,author,pubDate,URL) {
            var pub = Publication(title,author,pubDate);
            var publicAPI = {
                print() {
                    pub.print();
                    console.log(URL);
                }
            };

            return publicAPI;
        }

        var YDKJS = Book({
            title: "You Don't Know JS",
            author: "Kyle Simpson",
            publishedOn: "June 2014",
            publisher: "O'Reilly",
            ISBN: "123456-789"
        });

        YDKJS.print();
        // Title: You Don't Know JS
        // By: Kyle Simpson
        // June 2014
        // Publisher: O'Reilly
        // ISBN: 123456-789

        var forAgainstLet = BlogPost(
            "For and against let",
            "Kyle Simpson",
            "October 27, 2014",
            "https://davidwalsh.name/for-and-against-let"
        );

        forAgainstLet.print();
        // Title: For and against let
        // By: Kyle Simpson
        // October 27, 2014
        // https://davidwalsh.name/for-and-against-let
    ```

- ES Modules
    - ES modules (ESM), introduced to the JS language in ES6, are meant to serve much the same spirit and purpose as the existing classic modules just described, especially taking into account important variations and use cases from AMD, UMD, and CommonJS.
        ```javascript
            function printDetails(title,author,pubDate) {
                console.log(`
                    Title: ${ title }
                    By: ${ author }
                    ${ pubDate }
                `);
            }

            export function create(title,author,pubDate) {
                var publicAPI = {
                    print() {
                        printDetails(title,author,pubDate);
                    }
                };

                return publicAPI;
            }
        ```
    - To import and use this module, from another ES module like blogpost.js:
        ```javascript
            import { create as createPub } from "publication.js";

            function printDetails(pub, URL) {
                pub.print();
                console.log(URL);
            }

            export function create(title, author, pubDate, URL) {
                var pub = createPub(title,author,pubDate);
                var publicAPI = {
                    print() {
                        printDetails(pub,URL);
                    }
                };

                return publicAPI;
            }
        ```
    - And finally, to use this module, we import into another ES module like main.js:
        ```javascript
            import { create as newBlogPost } from "blogpost.js";

            var forAgainstLet = newBlogPost(
                "For and against let",
                "Kyle Simpson",
                "October 27, 2014",
                "https://davidwalsh.name/for-and-against-let"
            );

            forAgainstLet.print();
            // Title: For and against let
            // By: Kyle Simpson
            // October 27, 2014
            // https://davidwalsh.name/for-and-against-let
        ```

## Iteration

- for..ofloop
```javascript
// given an iterator of some data source:
var it=/* .. */;
// loop over its results one at a time
for(let val of it) {
    console.log(`Iterator value:${val}`);
    }
    // Iterator value: ..
    // Iterator value: ..
    // ..
```

- Spread iterator
Spread iterator actually has two symmetri-cal forms:spreadandrest(orgather, as I prefer). The spread form is an iterator-consumer. 
```javascript
    // An array spread:
    var vals = [ ...it ];

    // A function call spread:
    doSomethingUseful( ...it );
```

- Iterables  
The iterator-consumption protocol is technically defined for
consuming iterables; an iterable is a value that can be iterated
over.
```javascript
    // an array is an iterable
    var arr = [ 10, 20, 30 ];
    for (let val of arr) {
    console.log(`Array value: ${ val }`);

    var arrCopy = [ ...arr ];

    var greeting = "Hello world!";
    var chars = [ ...greeting ];
    chars;
    // [ "H", "e", "l", "l", "o", " ",
    // "w", "o", "r", "l", "d", "!" ]
}
```

## Closure
Closure is when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope.
```javascript
    function greeting(msg) {
        return function who(name) {
            console.log(`${ msg }, ${ name }!`);
        };
    }

    var hello = greeting("Hello");
    var howdy = greeting("Howdy");

    hello("Kyle");
    // Hello, Kyle!

    hello("Sarah");
    // Hello, Sarah!

    howdy("Grant");
    // Howdy, Grant!
```

Closures are not a snapshot of the msg variable’s value; they are a direct link and preservation of the variable itself. That means closure can actually observe (or make!) updates to these variables over time.
```javascript
    function counter(step = 1) {
        var count = 0;
        return function increaseCount(){
            count = count + step;
            return count;
        };
    }

    var incBy1 = counter(1);
    var incBy3 = counter(3);

    incBy1(); // 1
    incBy1(); // 2

    incBy3(); // 3
    incBy3(); // 6
    incBy3(); // 9
```

## **this** Keyword

Functions also have another characteristic besides their scope that influences what they can access. This characteristic is best described as an execution context, and it’s exposed to the function via its **this** keyword.  

Scope is static and contains a fixed set of variables available at the moment and location you define a function, but a function’s execution context is dynamic, entirely dependent on how it is called (regardless of where it is defined or even called from).  

**this** is not a fixed characteristic of a function based on the function’s definition, but rather a dynamic characteristic that’s determined each time the function is called.

```javascript
    function classroom(teacher) {
        return function study() {
            console.log(
                `${ teacher } says to study ${ this.topic }`
            );
        };
    }

    var assignment = classroom("Kyle");
```



## Prototypes
Where this is a characteristic of function execution, a prototype is a characteristic of an object, and specifically resolution of a property access.

Think about a prototype as a linkage between two objects; the linkage is hidden behind the scenes, though there are ways to expose and observe it. This prototype linkage occurs when an object is created; it’s linked to another object that already exists.
```javascript
    var homework = {
        topic: "JS"
    };

    homework.toString(); // [object Object]
```
- Object Linkage
    - To define an object prototype linkage, you can create the object using the **Object.create(..)** utility:
    ```javascript
        var homework = {
            topic: "JS"
        };

        var otherHomework = Object.create(homework);

        otherHomework.topic; // "JS"
    ```
    - The first argument to Object.create(..) specifies an object to link the newly created object to, and then returns the newly created (and linked!) object.
    - Delegation through the prototype chain only applies for accesses to lookup the value in a property. If you assign to a property of an object, that will apply directly to the object regardless of where that object is prototype linked to.
    ```javascript
        homework.topic;
        // "JS"

        otherHomework.topic;
        // "JS"

        otherHomework.topic = "Math";
        otherHomework.topic;
        // "Math"
        
        homework.topic;
        // "JS" -- not "Math"
    ```

    ## The Bigger Picture

    JS language can be organizated into three main pillars

    ### Pillar 1 Scope and Closure
    Scopes are like buckets, and variables are like marbles you put into those buckets. The scope model of a language is like the rules that help you determine which color marbles go in which matching-color buckets.
    JS is lexically scoped, though many claim it isn’t, because of two particular characteristics of its model that are not presenti n other lexically scoped languages.
    - Hoisting: when all variables declared any where in a scope are treated as if they’re declared at the beginning of the scope.

    ### Pillar 2 Prototyes

    Think about a prototype as a linkage between two objects; the linkage is hidden behind the scenes, though there are ways to expose and observe it. This prototype linkage occurs when an object is created; it’s linked to another object that already exists.

    ### Pillar 3 Types and Coercion

    This pillar is more important than the other two,in the sense that no JS program will do anything useful if it doesn’t properly leverage JS’s value types, as well as the conversion (coercion) of values between types.Even if you love TypeScript/Flow, you are not going to get the most out of those tools or coding approaches if you aren’t deeply familiar with how the language itself manages valuetypes.