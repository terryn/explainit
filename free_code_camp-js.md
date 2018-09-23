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

The iPhone6 class does not define `hasOwnProperty` anywhere, and neither does the iPhone class. So the final place that Javascript looks is in the `Object` class itself, where it does find that method. But if `hasOwnProperty` was defined on either the iPhone6 or the iPhone class, then *that* version of the method would be called instead. This is what is meant by `overriding`. We "override" the original method with one that is more specific.


```javascript

```


