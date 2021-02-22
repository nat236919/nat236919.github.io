---
title: "When Null is An Object"
date: 2020-09-10T09:50:26+08:00
draft: false
categories: ["blog"]
tags: ["blog", "javascript"]
---

{{< image src="https://i.ibb.co/FHcpxH3/js-meme.jpg" alt="JS is weird" position="center">}}

**JavaScript is weird**; that is all I can say about it. If I were asked to compare JS with an animal, it would definitely be a cat. Despite its overloaded cuteness, it always seems that it is plotting against you. However, at the end of the day, we still love them.

The fun started when I was working on a function that returns an object. The object was meant to be used for displaying data on a webpage. No hay problema, I already wrote a utility function to check an object type and it had worked like a charm. Little did I know that the function would cause an issue later on.

```javascript
function common_util_value_is_object(arg) {
    return (typeof arg === "object");
}
common_util_value_is_object({"data": "Hello There"}); // true
```

One day, while I was refactoring a back-end service (PHP), I encountered a weird behaviour causing a UI incorrect presentation. So I checked, since my refactored back-end service now can return a null value instead of an incomplete model object, I suspected it must be something about type checking.

By rechecking the function, I discovered that **typeof**, somehow, yields **true** for a null value. **How?? I mean Why????**

```javascript
console.log(typeof(null)); //object
console.log(typeof(null) === "object"); // true
```

Of course, this is not new and I am not the only one who was wondering what going on here. After some digging, the answer was just so simple; that is just how JS works as can be seen from primitive data types.

{{< image src="https://i.ibb.co/6ygwXFW/primitives.jpg" alt="primitives" position="center">}}

It had become clear that there was nothing I could do about it; therefore, I simply modified the functions as follow:

```javascript
function common_util_value_is_null(arg) {
    return ((typeof arg === 'undefined') || arg === null || arg == undefined);
}

function common_util_value_is_object(arg) {
    if (common_util_value_is_null(arg))
        return false;
    return (typeof arg === "object");
}

common_util_value_is_object({"data": "Hello There"}); // true
common_util_value_is_object(null); // false
```

## Summary

I do not know if this is a bug or merely a true nature. But it is something we all need to bear in mind when using JS. There is still a lot of weird behaviours that JS has to offer. So, be careful adventurers...

### References

* [JavaScript - typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#null)
* [Why is typeof null “object”?](https://stackoverflow.com/questions/18808226/why-is-typeof-null-object)
* [Why is null an object and what's the difference between null and undefined?](https://stackoverflow.com/questions/801032/why-is-null-an-object-and-whats-the-difference-between-null-and-undefined)
