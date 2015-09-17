## Swithing Lists to Arrays ?

##TODO : add something about the cpp target

### Abstract
There are some discussions here and there about how crappy are lists, somehow Simn said he never get why lists (need exact quote). There seems to be convergence over how crappy they are. BUT there are some misconceptions and old history carried back, as lists have been modernized a lot in a few years.
Let's sumup how they can impact your code. The topic of this note is the : should we switch from list to arrays for every time critical tasks.

### Requirements
All targets. Lists are fully cross platforms.

### Facts
- List were generating allocations through traversal. This is no longer the case.
- Most list Lambda traversal are not inlined.
- Every list move create a new array nesting the previous one.
- List are iteration removal safe

### Pros :
- Less small array allocations
- Array have a wider palette of uses

### Cons :
- Array resizing may imply array regrow and copy which is very costy in theory. In practice, as its backed by native functions, there cost are barely to be seen.
- Lose the ergonomic simplicity of lists.

### Benchmark :
- Benching traversal : http://try.haxe.org/#63c01
  js or swf, At low data size, results are indistinguishable, one has to go around 500k items to really have numera differences. Good to know that each run show different numbers, this is simply because those structures are so lightweight, that cache misses, and GC hit will just break timing. Around 500k hit, array become earnestly faster but calling this a "real world use case" would be preposterous.

- Insertions : 
  - Random insert before /after : http://try.haxe.org/#35242
  On js and swf, array clearly loses ( by a 10 factor ). Js is much slower than swf on my chrome.
- Insert at "designed position": http://try.haxe.org/#903dd
  Once again, array becomes consistently faster ( by a 3 factor ) at very high numbers.

### Conclusions
Well List and Arrays have clearly defined semantics and very different use cases. Latest efforts to inline List properly have bear fruits and they are now very competitive. This is a good lesson. List are now okay to work with and could be changed to array if the need is real. 
Congrat to the team to fixing this historical problem !

