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

So, to summarize: Every object in javascript comes from the Object class (this is already made for you by default). This means that no matter what kind of object you are dealing with, they all have the same methods and properties that belong to the Object class. So which are those methods and properties? There are lots, but one of them is `hasOwnProperty`, which we have seen before. So given that, is it possible to call `hasOwnProperty` on a class we have never seen before?

The answer is: yes. 

