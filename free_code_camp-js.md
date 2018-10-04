# Free Code Camp - JS

## Prototype and Own Properties
Prototype properties are properties who's values that are the same for all instances. For example, each iPhone has a different value for the background images, or what data is in your contact list, but the maker, or manufacturer, of each iPhone is the same (Apple). The manufacturer property would belong to the prototype since it is the same, no matter what kind of condition or data any iPhone actually has.

Own properties are things that are specific for each instance e.g. the data on given iPhone. Every iPhone will have similar properties, as previously mentioned, the contact list, the background image, etc, but the data stored in each of those properties is individual.

In summary: things that belong to the prototype have common values for *all* instances, and "own" properties can have unique values for each instance.
```javascript
iPhone.prototype.manufacturer = "Apple";
myIPhone.contactList = "Dave";
yourIPhone.contactList = "Skipper";
myIPhone === yourIPhone -> false
myIPhone.prototype.manufacturer === yourIPhone.prototype.manufacturer -> true
```

# Constructor Property
The constructor function that you use to make an instance of a class e.g. produce and iPhone, is a property on every iPhone instance with the name "constructor".

This constructor property is equal to the function used to make the instance e.g.

```javascript
function iPhone (owner) {
    this.owner = owner;
}

let myIPhone = new iPhone("me");
myIPhone.constructor === iPhone => true
```

The constructor property on an object can be used to test if that object, or instance, is of a particular type. However, there is a better method to check instance type, and that is the keyword `instanceof`;

```javascript
myIPhone instanceof iPhone -> true;
```

*Question*

How can you add multiple properties to a class's prototype at one time?

A. Set the prototype property to an object
```javascript
iPhone.prototype = {
    manufacturer: "Apple",
    overPriced: true,
    hasHeadphoneJack: false,
    makeAppleSound: function () {
        //play the tone
        }
    }
}
```

Now that we have set the prototype to a new object, it has blanked out our `constructor` property on all our instances and made it `undefined`. Wat. 

So we have to remember that when you set a prototype to an object, you should always define the `constructor` property and make the value of it the class e.g.

```javascript
iPhone.prototype = {
    **constructor**: iPhone,
    manufacturer: "Apple",
    overPriced: true,
    hasHeadphoneJack: false,
    makeAppleSound: function () {
        //play the tone
        }
    }
}
```

Just like you can test for what class an object belongs to, you can also test what prototype an object belongs to.

```javascript
iPhone.prototype.isPrototypeOf(myIPhone); -> true
```
You many notice that this method is completely ass-backwards. You use the prototype of the *class* to check an instance (object).

As you saw above when we made the `prototype` of an iPhone an object, prototypes themselves are in fact objects. You can check a variable's type by using the `typeof` keyword.

```javascript
typeof(iPhone.prototype); // -> object
```

All javascript objects have a prototype. Since prototype itself is an object, it too has a prototype! Let's take our iPhone class. At its core, all classes are subclasses of the `Object` class. This is a common feature among many different programming languages. So given that all classes are related to the `Object` class, what do you think this statement returns?

```javascript
Object.prototype.isPrototypeOf(iPhone.prototype); // true or false?
```

It evaluates to true. The prototype of the iPhone.prototype is the same as the prototype for `Object`. You are now probably asking "how is this useful?". At this moment, it is only useful because it demonstrates an important concept of `inheritance`.

`inheritance` is the idea that myIPhone and yourIPhone get some default properties from the class iPhone. This is useful because anyone who picks up an iPhone will already know how to use some of its features. This is why we have the same concept for Objects. When objects come from the same class, or they share common properties and methods, we can use them to do things without having to write different code for each iPhone in the world (which you can imagine would be impossible). Instead we write code that will work with *all* iPhones, even though each phone is different.

So, to summarize: Every object in javascript comes from the Object class (this is already made for you by default). This means that no matter what kind of object you are dealing with, they all have the same methods and properties that belong to the Object class. So which are those methods and properties? There are lots, we won't list them all at the moment, but one of them is `hasOwnProperty`, which we have seen before. So given that, is it possible to call `hasOwnProperty` on a class we have never seen before?

The answer is: yes. 

This demonstrates a powerful idea: we can make properties and methods (functions on objects) in one place, but have them be used many different situations. Often it is common in code to have to make a change to an object's class. For example, let's imagine that all iPhones have a `playAppleSound` ability, or function. If all the different iPhone versions play the same sound, we can write one function, instead of writing that function for each iPhone type.

```javascript
function iPhone () {}; //this is the basic class that all iPhones share, regardless of their version.
iPhone.prototype = {
    constructor: iPhone,
    manufacturer: 'Apple',
    playAppleSound: function () {
        // beowww
    },
    version: 'unknown, please define'
}

function iPhone5() {};
let myIPhone5 = Object.create(iPhone.prototype); // we use this instead of the `new` keyword for reasons that will be explained later
myIPhone5.version = '5';

function iPhoneX() {};

let myIPhoneX = Object.create(iPhone.prototype);
myIPhoneX.version = 'X';

myIPhoneX.playAppleSound(); //beowww
myIPhone5.playAppleSound(); //beowww

```
If we did not use `inheritance` then we would instead have to do this:

```javascript
function iPhone5() {
   version: 5,
    playAppleSound: function () {
        //beowww
    }
};

function iPhoneX() {
   version: X,
    playAppleSound: function () {
        //beowww
    }
};
```

Notice the duplication of the `playAppleSound` method? What if you had to change the sound for each iPhone type? In the example directly above, you would have to change it twice and be careful that you didn't make a mistake. But in the example that uses inheritance you only need to change it in the prototype once, and then all your iPhone classes will be affected.

Remember, programming is all about using this reusability principal to our advantage. Inheritance is one way to do it, but there are others that we will encounter later, but underneath it all is this ability to build easy to use pieces that we can put together and change at will.

Above you saw how we can create different iPhones from the basic iPhone class's prototype. 

`let iPhone6 = Object.create(iPhone.prototype);`

That's fine, but you might have noticed that we have no way of giving specific attributes to our iPhone6 type, it only has the basic properties and methods that are common to all iPhones, and nothing that is specific to *only* iPhone 6's.

What we can do instead is still define an iPhone6 class, but set its prototype to the prototype of the basic iPhone class, like this:

```javascript
function iPhone6 (version) {
    this.version = version;
}

iPhone6.prototype = Object.create(iPhone.prototype);

let myIPhone6 = new iPhone6(6);
console.log(myIPhone6.version); //prints '6'
console.log(myIPhone6.playAppleSound()); // prints 'beowww'
```

Javascript has some silly default behavior. In what we just wrote above, the iPhone6 will have its `constructor` property set to that of the iPhone class. I have no idea why it works that way, but this demonstrates that principle.

```javascript
let myIPhone6 = new iPhone6(6);
myIPhone6.constructor === iPhone.prototype.constructor; // returns `true`
```

What we want instead is that when we make a specific iPhone version, we want the constructor to belong to the actual class we made, in this case an iPhone6.

To do that we need to expicitly (as in do it ourselves, it is not automatic), set the constructor of the prototype of the iPhone6 to be equal to the iPhone6 function i.e.

```javascript
iPhone6.prototype.constructor = iPhone6;
```

If we want to add specific methods to our iPhone6, we can do that by adding the to the iPhone6's prototype e.g.
```javascript
iPhone6.prototype.whatVersionAmI = function () {
    return this.version;
}

let myIPhone6 = new iPhone6('6');
console.log(myIPhone6.whatVersionAmI()); // prints '6'
```
So far the iPhone6 has its own `whatVersionAmI` method that only it has. But what if we want *every* iPhone to have that method, but that it doesn't something different for each iPhone version. We can *override* a method by defining it twice. Best seen with an example.

```javascript
function iPhone () {
    whatVersionAmI: function () {
        return 'I have no idea what version I am, I am only the basic iPhone class';
    }
}

function iPhone6 (version) {
    this.version = version,
    whatVersionAmI: function () {
        return this.version;
    }
};
iPhone6.prototype = Object.create(iPhone.prototype);
iPhone6.prototype.constructor = iPhone6;
```

If the iPhone6 class did not duplicate the `whatVersionAmI` method, this would be the result

```javascript
console.log(myIPhone6.whatVersionAmI()); // prints 'I have no idea what version I am, I am only the basic iPhone class'
```

This is because of the way that Javascript looks for methods. It starts with the most specific spot it can find something, but if it is not there, it looks at the next most specific spot, and continues until it either finds the property or method, or, it fails to find it and creates an `Exception` (more on those later).

Pretending that our iPhone6 has no method called `whatVersionAmI`, Javascript will look at the next place, which is the prototype of the iPhone class. This is because an iPhone is *is* an iPhone, and its prototype is nearly identical to the iPhone's prototype. Because that method is defined for an iPhone, that method is called.

Remember the `hasOwnProperty` method that belongs to the Object class, which all classes are a part of? Well, what happens when you call

```javascript
myIPhone6.hasOwnProperty('version');
```
?

The iPhone6 class does not define `hasOwnProperty` anywhere, and neither does the iPhone class. So the final place that Javascript looks is in the `Object` class itself, where it does find that method. But if `hasOwnProperty` was defined on either the iPhone6 or the iPhone class, then *that* version of the method would be called instead. This is what is meant by `overriding`. We "override" the original method with one that is more specific. The most specific definition "wins". If both the iPhone6 and the iPhone class had `hasOwnProperty` methods, then only the iPhone6 version would be called by

```javascript
myIPhone6.hasOwnProperty('version');
```

# Mixins

Mixins stands for "mixed-in" i.e. you added two unrelated things together and made something new e.g. a cake.

So what does this mean? It means that you can combine unrelated classes to get a combination of the two (or more). Let's see an example:

```javascript
function iPhone () {};
iPhone.prototype = {
    constructor: iPhone,
    ...
}

function LightSwitch() {};
LightSwitch.prototype = {
    constructor: LightSwitch,
    ...
}

function addTurnOff(someFunction) {
    someFunction.turnOff = function () {
        console.log('please turn off now');
    }
}
let myIPhone = new iPhone();
let myLightSwitch = new LightSwitch();
addTurnOff(myIPhone);
addTurnOff(myLightSwitch);

// Now both myIPhone and myLightSwitch have a 'turnOff' method.
// We can make a general 'turnOff' method and then add it to whatever we think
// needs the ability to be turned off, just like we did to the two unrelated
// classes, iPhone and LightSwitch.
```

# What the ?&*! Is A Closure?
A "closure" is a function inside a function, like this:

```javascript
function outsideFunction() {
    function insideFunction() {
        // ascii art here
    }
}
```

What happens when we do this:
```javascript
outsideFunction();
```

The answer is "absolutely nothing". This is because when the outsideFunction is called, all it does is define a new function. This is all perfectly valid, but so far hasn't done anything yet.

What if we changed it to this:

```javascript
function outsideFunction() {
    function insideFunction() {
        return "I'm inside";
    }
}

outsideFunction(); // what do I return?
```

The correct answer is "still nothing". What are we missing? We need to add the `return` keyword to the bottom of the `outsideFunction` function. But what are we going to return?

```javascript
function outsideFunction() {
    function insideFunction() {
        return "I'm inside";
    }
    return insideFunction();
}

outsideFunction(); // what do I return?
```

Now `outsideFunction()` will return "I'm inside". So we have now see that you can have a function defined within a function, *and* that you can actually call, or invoke, that inside function. This is a closure. So far it is completely useless, but it is still a closure. So how is this useful?

For one thing, we can take advantage of the way Javascript works and hide variables from others trying to modify them.

```javascript
function outsideFunction() {
    let someSecretVariableThatNobodyCanModify = "Who rocks? You do!";
    function insideFunction() {
        return "I'm inside";
    }
    return insideFunction();
}

outsideFunction(); // same output, but there is a hidden variable that we
                   // don't know about.
```

Ok, so we've got a secret variable that nobody can modify. Variables like this are said to be "private". Still, this doesn't explain why closures are useful. Let's make a private variable for a *class* instead of just a random function. Actually, just because of the way javascript works, we can make an object from the outsideFunction directly, so let's do that. **But** we need to modify the `insideFunction` just a bit to make our point.

```javascript
function outsideFunction() {
    let someSecretVariableThatNobodyCanModify = "Who rocks? You do!";
    function insideFunction() {
        return someSecretVariableThatNobodyCanModify; // this line is different
    }
    return insideFunction();
}
let demonstration = new outsideFunction(); // normally class definitions should be uppercase, this is only for demo purposes.

console.log(demonstration.insideFunction()); // what does this output?

// Who rocks? You do!
```

With the code above we can now make a variable for instances of the `outsideFunction` that can't be changed. It is private. But! We can *read* the value by calling the `insideFunction`. So the attribute is **read-only**.

You might be asking "and?". Now is not the time to explain why this is useful, but it does have a purpose which will become more clear with experience. For now just know that having some private read-only attributes on a class can be useful, and closures, or functions inside of functions, are a very common way in Javascript to do this.

# Immediately Invoke Function Expression (IIFE)
Sometimes we want to write a function definition *and* have that function immediately execute whenever the Javascript engine e.g. a browser, finds it. It is possible to just write all the code inside the function inside the global scope, but you won't be able to get a job writing that, and besides, it will make your life easier to group functionality together.

```javascript
(function iGot99Problems () {
    console.log("but IIFEs aren't one");
})();

// is equivalent to just writing this
console.log("but IIFEs aren't one");
// This example is trivial so the code directly above is simplier, but
// if you were going to do something complicated the IIFE is a better idea.
```

In Javascript 6 there is shorthand syntax for defining functions.

```javascript
let chocolate = function () {
    this.yum = "yes";
}
// Is equivalent to
let chocolate = () => {
    this.yum = "yes";
}
```
Notice the empty () in the second example? It's the same () that surround the beginning and end of our IIFE.

```javscript
(function () {
    console.log("just executed");
})(); // <- see the () here?
```

The final () at the end is just the way of calling, or invoking, the annonymous function we just wrote (anonymous because the function has no name that we can refer to it by).

### IIFE to make a module ###
What do we mean by a module? A module is a grouping of functionality i.e. variables and functions. It's for our own benefit, to make a kind of lego block of code that we can reuse whenever we need it. When I say I need a flat four by four lego block, everybody knows what that means, and we can make the equivalent of those by creating modules.

Immediately Invoking Function Expressions are a way to create modules. Prepare yourself for a ton of curly braces.

```javascript
let legoBricks = (function () {
    return {
        // crap, yet another set of curly braces
        twoByTwoBrick: function () { // yes, there is more
            return "I'm a two by two brick";
        },
        rubberTireThatIsFunToChew: function () {
            return "What?"
        }
    }
})
```

Believe it or not, but I took it easy on you with the curly braces. If we had `mixin`s in our module then we'd shower you with even more braces. You will see this out in the wild.

# Functional Programming
So far the programs we have seen are written with the "imperative" programming style. Imperative programs look very much like an instruction manual or a recipe. We do each line in a specific order and we give instructions *how* to do it. It's like giving orders to the most literal person in the world; you must spell out exactly what you want in the exact correct order. But imperative programming isn't the only way programs can be written, there is a another style called "functional programming".

Functional programmming is a way to write a program that doesn't actually do anything. That is a bit of an exaggeration, but if we were being strict, this is in a way true. What we mean by "functional programming doesn't do anything" is that they do not produce any observable result, also known as a "side-effect". Example time.

```javascript
let weatherForecast = 'partly cloud with a chance of meatballs';
console.log(weatherForecast); // outputs 'partly cloud with a chance of meatballs', so far totally predictable.
```

The above example is very clear: print the value of `weatherForecast` to the screen i.e. console. What if we hadn't printed the value of the variable? Would we have been able to know what the `weatherForecast` variable contained? No, this was only possible because *producing output, e.g. printing something to a screen, is a side-effect*. By printing, we altered the state of the console, we made some I/O (input/output). Printing is only one example of a side-effect, there are many others such as: writing to a file, making an entry into a database, browsing the internet, etc. 

When we do an action that changes something, we produce a side-effect. Another way of putting it is that we have "altered state". It's like opening or closing a door. When you make an action on the door, you can either put it into the open or closed position. Either way, you've altered the door's state.

Functional programming is more like both opening the door, and then before the program finishes, closing it again. So in the end, everything goes back to where it originally was. This raises the question of what the heck the point is. If we can never open a door and keep it open, we can never step through it and you're stuck. Functional programming fixes this by cheating: it can produce side-effects, but it does its hardest to avoid doing so as much as possible. How functional programs produce side-effects will be explained further, but the main idea is that they *avoid* this as much as possible.

One of the main benefits of functional programming is that it **can be easier to reason about the program's structure**. By having this contraint of changing almost nothing and breaking our program into many small functions, we can more clearly see what our program actually does.

```javascript
function TheDude () {
    this.abides = true;
    this.talk = function () {
        if (this.abides) {
            return "The Dude abides.";
        } else {
            return "That rug really tied the room together, man!";
        }
    }
}

function changeTheDude(theDude) {
    theDude.abides = false;
}

let theDude = new TheDude();
console.log(theDude.talk()); // "The Dude abides."
changeTheDude(theDude);      // here we change the state of theDude
console.log(theDude.talk()); // "That rug really tied the room together, man!"
```
Above we have a typical example of bad imperative programming. We have an object and it has its own state represented by the `abides` boolean attribute. The `changeTheDude` function alters the state of `theDude` object i.e. before the function `changeTheDude`, the value of `abides` was `true`, afterwards it was false i.e. theDude has been changed.

Now imagine that you have a complicated object that has many many attributes and methods, and that that this object is used over thousands of lines of code. Any one of those lines could make a change to the internal state of the object, by setting a new field or resetting the value of one it already has. If someone comes to you and asks "on line 14335, what is the value of `abides`?", you would be perfectly in the right to say "I have no clue", because there are simply too many places where this object i.e. theDude, has been changed.

This can get even more complicated when the code has a lot of conditional statements that will *possibly* set *even more* attributes on the object, but only if a statement evaluates to true. And then *those* attributes can be used in further conditional statements, etc. 

Functional programming avoids this problem by not keeping state, and only when it needs to, producing a side-effect. Why isn't functional programming used by everybody all the time? Because pure functional programming has its own complexity, and it can be more difficult to start writing a program. To some people functional programming is more intuitive and easier, but to most it looks like an evil math professor is trying to *?&! with you.

```javascript
function talk(abides) {
    if (abides) {
        return "The Dude abides.";
    } else {
        return "That rug really tied the room together, man!";
    }
}

console.log(talk(true));
console.log(talk(false));
```
Compared to the first example, the one above is always clear on what exactly the `talk` function returns, it doesn't matter if it is called as the first line in your program or if it is on the 20000th line. The princple here is this: functional programming takes the output of the previous function and gives it as input to the next function, which takes that output and gives it to the next function, etc. So what you get is a chain of functions, each providing the input for the next one in the chain.

```javascript
function talk(abides) {
    if (abides) {
        return "The Dude abides.";
    } else {
        return "You peed on my rug, man!";
    }
}

function peedOnRug() {
    return false;
}

function drinkingWhiteRussian() {
    return true;
}

console.log( // What is the output?
  talk(
    peedOnRug() // returns `false`.
  )
);            
console.log( // And here, what is the output?
  talk(
    drinkingWhiteRussian() // returns `true`
  )
); 
```

The above example is functional programming. The output from `drinkingWhiteRussian` and `peedOnRug` is used as *input* for `talk`. `talk` always produces the same output when it is given the same input, so as long as we know what `drinkingWhiteRussian` and `peedOnRug` produce, we can know what `talk` will actually return.

You may notice that even `talk`'s output is passed to another function: console.log. That is the function that produces the side-effect of printing the value returned by `talk`. It is important to notice that the function that is furtherest on the outside is actually executed last. So the very first function to be run in the first example is `peedOnRug`.

peedOnRug() -> false -> talk (false) -> "You peed on my rug, man!" -> console.log("You peed on my rug, man!");

drinkingWhiteRussian() -> true -> talk (true) -> "The Dude abides, man." -> console.log("The Dude abides, man.");

All of the above happened by only calling functions and then using the output as input for the next function. There is no variable that lives outside any of these functions, and there is no "state" to be changed. If we can understand the beginning of the chain, we can follow the output all the way to the end to see our final result.



Fortunately, many programming languages have support for both imperative *and* functional programming styles. Personally, I like to use functional as much as possible, and only use imperative e.g. objects, when it is necessary. Javascript heavily uses functional programming techniques, and you will see it in a lot of the code that is written.

### Side-effects 
Side-effects 

### Callbacks
A `callback` is a function that was given as an argument to another function.

```javascript
function callbackFunction(coffee) {
    console.log("I am being executed by the function that I was given to as a parameter");
    let sugar = "2 lumps";
    return sugar + coffee;
}
function main(callbackFunction) {
    console.log("do all the things.");
    console.log("time to make the callback");
    let someCalculation = ...
    let calculationTransformed = callbackFunction(someCalculation);
}
```
The callbackFunction is a *blocking callback*. This is because the `main` function cannot finish before the `callbackFunction` is finished and returns the `calculationTransformed`. This example, as you may have guessed, is pretty useless. So what are callbacks used for anyway?

Here are two ways callbacks are commonly used: performing an action on each item in a list (we'll give an example), writing "event-driven" programming (more on this later).

##### Performing an action on each item in a list
By default, javascript arrays have a function called `filter`. This function takes a function as one of its arguments, like so.

```javascript
function coffeeFilter(item) {
    return item === 'coffee';
}

let drinks = ["kombucha", "carrot juice", "coffee", "soda"];

console.log(drinks.filter(coffeeFilter)); // outputs "coffee"
```
The function we give `filter` sets the criteria for keeping the item, or *filtering* it out. By returning `true` we keep the item. `coffeeFilter` is a callback.


```javascript
let someOfMyFavoriteThings = ["Raindrops", "roses", "whiskers", "kittens"];

function iLikeKittens(item) {
    if (item === "kittens") { // only interested in kittens
        return true;
    }
    return false;
}

Array.prototype.filter = function(yourCallback) {
    let outputArray = [];
    for (let i=0;i<this.length;i++) {
        if (yourCallback(this[i])) { // if `yourCallback` returns true
            outputArray.push(this[i]);
        }
    }
    return outputArray;
}
console.log(someOfMyFavoriteThings.filter(iLikeKittens)); // Array["kittens"]
```

The other use of callbacks that we've mentioned is for event-driven programming. In plain English it means this: you know when you go to do something else after you have started making a bath and it is not yet full? That's what we're talking about.

The first part is you turn the water on for the bath, and this is our "first" function. Then you go and make a sandwich (another other function). Then, when the bath is full, you turn the water off (that is your callback). 

The *event* of the bath being full is what made you turn the water off. You could have just sat in the bathroom and waited for the bath to fill up, but by doing something inbetween starting the bath and it being ready, you could do another task while you were waiting. This is the power of event-driven programming: you can start a task that will take some time, do some other quick tasks, and then come back to the first task when it is finally ready. This is called "non-blocking" vs the option to just stare at the bath until it is full (blocking). Callbacks are a common way to accomplish this, and in Javascript we will see this used extensively.

So we can say that callbacks are kind of cool, but there is also something called callback-hell, and this is when there are so many asynchronous callbacks being used that no one call tell what the \*bleep\* is going on. Javascript has a lot of that, too, unfortunately. 

### Splice vs Slice
The `splice` and `slice` methods are functions on the Array.prototype i.e. all arrays have these methods. `splice` changes (mutates), the original array.

```javascript
let holidays = ["Kwanza", "Cinco de Mayo", "Easter", "Halloween"];
holidays.splice(1,2); // produces "Cinco de Maya"
console.log(holidays); // ["Kwanza", "Easter", "Halloween"];
console.log(holidays.length); // 3
```

Whereas the `slice` method does not change the original array, but returns a *copy* of the array section you want. 

```javascript
let holidays = ["Kwanza", "Cinco de Mayo", "Easter", "Halloween"];
let shorterHolidayList = holidays.slice(0,1); // ["Kwanza"]
console.log(holidays); // ["Kwanza", "Cinco de Mayo", "Easter", "Halloween"];
console.log(holidays.length); // 4
```

How to remember which one is the copier and which one changes the array? 
  `splice` is nice but `slice` is better.

In general, we want to avoid changing the variables given to us. Instead we want to use those variables to produce a *new* variable that we can then use in our programs. One of the ways this is accomplished is by copying variables that can be mutated and use the new copy rather than changings the variable and continuing to use that one.

### Concatenation aka Concat aka Cat
Concatenation is simply making two things into one by joining them together. This is most common on strings, but we can do it for arrays as well.

```javascript
// string concatenation
let cowName = "Bessie";
let dogName = "Albert";
let cowAndDog = cowName + " and " + dogName;
console.log(cowAndDog); // "Bessie and Albert"

// array concatenation
let cows = ["Bessie"];
let dogs = ["Toffi", "Albert"];
let cowsAndDogsTogether = cows.concat(dogs);
console.log(cowsAndDogsTogether); // ["Bessie", "Toffi", "Albert"];

// Order is important for both strings and arrays!

let dogsAndCowsTogether = dogs.concat(cows);
console.log(dogsAndCowsTogether); // ["Toffi", "Albert", "Bessie"]

console.log(dogs); // ["Toffi", "Albert"]
console.log(cows); // ["Bessie"]
```
We should note that neither the `dogs` nor the `cows` array has been changed by concatenation. When you use the `concat` array method, you get a *new* array that is a combination of both, and it leaves the original two arrays as they were.

##### Why Use Concat?
As just mentioned, concatenation does not change the original arrays, but still produces a new result i.e. the new combined array, that we can use. This is exactly what we want with functional programming, and like `splice` vs `slice` (do you remember which one is better?), the `concat` function is better for the `push` function. `push` modifies the original array, `concat` will give you a new array with the new items on it.

```javascript
let meals = ["breakfast", "second breakfast", "lunch"];
let dinner = "Dinner!!";

let allMeals = meals.concat(dinner); // makes a new array as a copy of meals with the new item
console.log(allMeals)); // ["breakfast", "second breakfast", "lunch", "Dinner!!"];

console.log(meals); // ["breakfast", "second breakfast", "lunch"]
meals === allMeals; // false

meals.push(dinner); // push changes "meals" to add one more element
console.log(meals); // ["breakfast", "second breakfast", "lunch", "Dinner!!"]

meals === allMeals; // true
```




