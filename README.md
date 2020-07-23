# javascript-quirks
JavaScript Quirks
```
/* Object and Properties */
if(typeof global !== "undefined") {
    console.log("Node: global, this", global, this); // global, {}
    console.log("Node: this === global", this === global); // false
}

if(typeof window !== "undefined") {
    console.log("Browser: global, this", window, this); // window, window
    console.log("Browser: this === global", this === window); // true
}

const userDefined = { name: "A", age: 40 };

console.log("Object", Object); 
// [Function: Object]

console.log("Object.prototype", Object.prototype); 
// Node = {}, Browser = {constructor, hasOwnProperty...} Properties are non-enumerable in both cases so Node.js console will not print but Chrome will show these properties with lower opacity

console.log("Object.getOwnPropertyNames(Object.prototype)", Object.getOwnPropertyNames(Object.prototype)); 
// ["constructor", "hasOwnProperty"] Object.getOwnPropertyNames return list of all properties only

console.log("Object.keys(Object.prototype)", Object.getOwnPropertyNames(Object.prototype)); 
// [] Object.keys return list of enumerable properties only

console.log("Object.getOwnPropertyDescriptor(Object.prototype, \"hasOwnProperty\")", Object.getOwnPropertyDescriptor(Object.prototype, "hasOwnProperty")); 
// { value: [Function: hasOwnProperty], writable: true, enumerable: false, configurable: true }

console.log("Object.prototype.toString", Object.prototype.toString); 
// [Function: toString]

console.log("userDefined.hasOwnProperty", userDefined.hasOwnProperty("name")); 
// true

console.log("Object.prototype.__proto__", Object.prototype.__proto__); 
// null

console.log("Object.getOwnPropertyDescriptor(userDefined, \"name\")", Object.getOwnPropertyDescriptor(userDefined, "name"));
// { value: "A", writable: true, enumerable: true, configurable: true }


/* Context (this) */
var prop = "nullValue";

this.otherProp = "otherNullValue";

var anObject = {
    prop: "value",
    otherProp: "otherValue",
    method: function() {
        console.log(this.prop);
    },
    otherMethod: function() {
        console.log(this.otherProp);
    },
    arrowMethod: () => console.log(this.prop, this.otherProp)
}

var fn = anObject.method,
    otherFn = anObject.otherMethod,
    arrowFn = anObject.arrowMethod
    boundFn = anObject.method.bind({ prop: "someValue", otherProp: "someOtherValue" }),
    boundArrowFn = anObject.arrowMethod.bind({ prop: "emptyValue", otherProp: "someEmptyValue" });

anObject.method(); // "value"
anObject.otherMethod(); // "otherValue"
fn(); // Node = undefined, Browser = "nullValue"
otherFn(); // Node = undefined, Browser = "otherNullValue"
arrowFn() // Node = (undefined, "otherNullValue"), Browser = ("nullValue", "otherNullValue") // Value of 'this' in an arrow function is retained from its immediately enclosing lexical context
boundFn() // "someValue"
boundArrowFn() // Node = (undefined, "otherNullValue"), Browser = ("nullValue", "otherNullValue") // Arrow function is uneffected with use of 'bind'


/* Closure (And Lexical Scope) */
var variable = 20;

function enclosure() {
    var variable = 10;

    return function returnable(multiplier) {
        console.log(variable * multiplier);
    }
}

var tester = enclosure();

enclosure()(10); // 100
tester(20); // 200

```
