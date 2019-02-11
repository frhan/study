# Inheritance and the prototype chain

JavaScript only has one construct: `objects`. 

Each object has a private property which holds a link to another object called its _**prototype**_.

Nearly all objects in JavaScript are instances of `Object` which sits on the top of a prototype chain.