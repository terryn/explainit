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
