> React.js luckily has a mixin called PureRenderMixin which compares the new incoming properties with the previous one and stops rendering when it's the same. It uses the shouldComponentUpdate method internally.
That’s nice, but PureRenderMixin can't compare objects properly. It checks reference equality (===) which will be false for different objects with the same data:
>

PureRenderMixin两处判断
```
var a = { foo: 'bar' };
var b = { foo: 'bar' };
a === b; // false

var a = { foo: 'bar' };
var b = a;
b.foo = 'baz';
a === b; // true
```
1.List

list 结构有点类似于数组,以及es6中的set数据结构
es6 set方法中add,delete,clear都会影响其自身

> the difference for the immutable collections is that methods which would mutate the collection, likepush, set, unshift or splice instead return a new immutable collection. Methods which return new arrays like slice or concat instead return new immutable collections.

static methods:
   List.isList(),
   List.of()
method:
   push(),
   pop(),
   shift(),
   unshift(),
   concat(),
   update(),
   merge(),
   mergeWith(),
   mergeDeep(),
   mergeDeepWith(),
   setSize(),

Deep persistent changes:
     setIn()
     deleteIn()
     updateIn()
     mergeIn()
     mergeDeepIn()

Transient changes:
     withMutations()
     asMutable()
     asImmutable()

Conversion To seq:
     toSeq()
     toKeyedSeq()
     toIndexedSeq()
     toSetSeq()
     fromEntrySeq()

Value Equality
     equals()
     hashCode()

Reading values
     get()
     has()
     includes()
     first()
     last()

Reading Deep values
     getIn()
     hasIn()

Conversion To Javascript Types
     toJs()
     toArray()
     toObject()
Conversion to Collections
     toMap()
     toOrderedMap()
     toSet()
     toOrderedSet()
     toList()
     toStack()
Iterables
     keys()
     values()
     entries()
Iterables(seq)
     keySeq()
     valueSeq()
     entrySeq()
sequence algorithms(序列比对算法)
     map()
     filter()
     fiterNot()
     reverse()
     sort()
     sortBy()
     groupBy()

Side effects
     forEach()

Creating subSets
     slice()
     rest()
     butLast()
     skip()
     skipLast()
     skipWhile()
     skipUntil()

Combination
     concat()
     flatten()
     flatMap()
     interpose()
     interleave()
     splice()
     zip()
     zipWith()

Reduceing a value
     reduce()
     reduceRight()
     every()
     some()
     join()
     isEmpty()
     count()
     countBy()
Search for values
     find()
     findLast()
     findEntry()
     findLastEntry()
     max()
     maxBy()
     min()
     minBy()
     indexOf()
     lastIndexOf()
     findIndexOf()
     findIndex()
     findLastIndex()
comparison
     isSubset()
     isSuperset()

tips:
@mergerWith and mergerDeepWith first argument as a function which paramas (prev, next), eg:

var x = Immutable.fromJS({a: { x: 10, y: 10 }, b: { x: 20, y: 50 } });
var y = Immutable.fromJS({a: { x: 2 }, b: { y: 5 }, c: { z: 3 } });
x.mergeDeepWith((prev, next) => prev / next, y)
// {a: { x: 5, y: 10 }, b: { x: 20, y: 10 }, c: { z: 3 } }

Seq describes a lazy operation, allowing them to efficiently chain use of all the Iterable methods (such as map and filter).

fromJS()

Deeply converts plain JS objects and arrays to Immutable Maps and Lists.








