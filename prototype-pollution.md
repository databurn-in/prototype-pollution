# JavaScript Prototypes

These notes are made while reading this documentation: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain

JavaScript is a prototype-based language and NOT "class-based" like other languages such as Java or C++.

The `class` keyword was added in ES2015 but it is just syntactical sugarcoating and JavaScript remains prototype-based.

The only 'Inheritance' JavaScript objects have is the chain of prototypes.

Each object in JS has a link to it's prototype. And those prototypes have their own links to other prototypes and so on. The final prototype will be the `Object` object. This `Object` has `null` as it's prototype indicating that it's the last in the chain. This chain is called the **Prototype Chain**.

When trying to access a property of an object, the JavaScript "compiler" tries to walk through the prototype chain to find that property. If the property is found on the first object in the chain, the compiler stops at that. This concept is very important.

Prototype of an object in JavaScript can be accessed through `__proto__` property. Please be aware that `__proto__` is non-standard but is always implemented by almost all browsers.

Apart from `__proto__` , there are also `func.prototype` (for a function) and `Object.prototype`. Note that `__proto__` of an object and `prototype` of a function are two different *concepts* but essentially both are the same things.

Also important to note that `prototype` of an `Object` has a special meaning.   `Object.__proto__` is `null` but `Object.prototype` is `Object`. (You can think of `Object.prototype` as `Object.this` in some other languages I guess!?!?!)
   
### Function "objects" and their `prototype`

You can have function "objects" in JavaScript. That is, you can assign functions to variables and pass those variables around.

When we create a function, it has it's own properties and it's prototype will be the `Object`. And we can create new *instances* of a function definition using the `new` operator.

```javascript
function doSomething(){}
doSomething.prototype.foo = "bar";
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; 
console.log( doSomeInstancing );
```

Output will be:

```javascript
{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
```

JavaScript has this special `prototype` property only for functions because whenever we create instances of a function using `new`, those new *function variables* will be created on based on this function `prototype`. It's just a concept. Essentially, `prototype` and `__proto__` for a function are the same.

But regular objects don't have this prototype property and only have `__proto__` because, you can create new objects from existing objects using the `new` operator.

For example:
```javascript
a = {name: 'nandan'}
b = new a;
VM1050:1 Uncaught TypeError: a is not a constructor
    at <anonymous>:1:5
(anonymous) @ VM1050:1
```

**Then how do you create a new object by taking an existing object as a prototype?**

Using Object.create() method.

"The `Object.create()` method creates a new object, using an existing object as the prototype of the newly created object."

More: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create


# Prototype Pollution

"Prototype pollution" attack can be used when the developers are trying to access some property of an object that doesn't exists.

For example, consider the following logic:

```javascript
if(user.admin){
	//admin related stuff
}else{
	//do normal user stuff
}
```

In that logic, the developer is check for the existence of *admin* property on the *user* object. If the admin property exists, they're trying to execute critical admin-related stuff. That's where we can pollute the prototype. 

We can say something like, `Object.prototype.admin = true`.

And now, when that logic is being executed, the browser tries to get the *admin* property of the *user* object. As this property doesn't exist on the *user* object, it's prototype will be checked (and it's prototype is the `Object`). As we had created this *admin* property on the `Object`, it will be considered and used! And we get inside that if condition and take control of the admin related tasks! That's basically what Prototype Pollution is!




