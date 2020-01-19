# Javascript Garbage Collection

Memory lifecycle
Irrespective of any language (high-level or low-level), Memory life cycle will be almost similar as the below.
- **Allocate Memory** ---> **Use Memory** ---> **Release Memory**

#### Memory Allocation in Javascript:
When declaring the variable, Javascript will automatically allocate the memory for the variables.

```javascript
var numberVar = 100;
var stringVar = 'node simplified';
var objectVar = {a: 1};
var a = [1, null, 'abra'];
function f(a) { return a + 2; }
```
When the memory is no longer needed anymore, then the allocated memory will be released. Memory leak and most of the memory related issue come while releasing the memory. The hardest part is finding the memory which is no longer needed and it is tracked down by the garbage collector.

**Garbage collection:** It is the process of finding memory which is no longer used by the application and releasing it. To find the memory which is no longer used, few algorithms will be used by the garbage collector and in this section we will look into main garbage collection algorithms and its limitations. we will look into following algorithms.
- Reference-counting garbage collection
- Mark-and-sweep algorithm

#### Reference-counting garbage collection
It is most important garbage collection algorithm and in which when there is no reference to an object then it will be automatically garbage collected. This algorithm considers zero referencing object as an object that is no longer used by the application.

```javascript
var o = { a: { b: 2 } }; o = 1;
function func() { 
    var o = {}; 
    var o2 = {}; 
    o.a = o2; 
    o2.a = o; 
    return 'true'; 
} 
func();
```
Consider the above code snippet in which o is referenced to o2 and o2 is referenced to o and it creates a cycle. When the scope goes out of the method, then these two objects are useless however garbage collector unable to free the memory since those two still got the reference to each other. It leads to memory leaks in the application.

#### Mark-and-sweep algorithm:
This algorithm reduces the definition of "an object is no longer needed" to "an object is unreachable".

This algorithm assumes the knowledge of a set of objects called roots. In JavaScript, the root is the global object. Periodically, the garbage collector will start from these roots, find all objects that are referenced from these roots, then all objects referenced from these, etc. Starting from the roots, the garbage collector will thus find all reachable objects and collect all non-reachable objects.

This algorithm is an improvement over the previous one since an object having zero references is effectively unreachable. The opposite does not hold true as we have seen with circular references.

As of 2012, all modern browsers ship a mark-and-sweep garbage-collector. All improvements made in the field of JavaScript garbage collection (generational/incremental/concurrent/parallel garbage collection) over the last few years are implementation improvements of this algorithm, but not improvements over the garbage collection algorithm itself nor its reduction of the definition of when "an object is no longer needed".

##### Example 1

```javascript
data = "Dummy data"
console.log(data)
delete data
console.log(data)

// Dummy data
// Uncaught ReferenceError: data is not defined
```

```javascript
var data = "Dummy data"
console.log(data)
delete data
console.log(data)

// Dummy data
// Dummy data
```

##### Reference
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management
https://medium.com/nodesimplified/javascript-memory-management-and-garbage-collection-in-javascript-ebe7a97d7143
https://www.youtube.com/watch?v=jkHsO0N3NGs
