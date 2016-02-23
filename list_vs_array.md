## Swithing Lists to Arrays ?

### Abstract
There are some discussions here and there about how crappy are lists, somehow Simn said he never get why lists (need exact quote). There seems to be convergence over how crappy they are, even for very low volume of data. BUT there are some misconceptions and old history carried back, as lists have been modernized a lot in a few years. On the haxe compiler, iterator wasn't even inlined and triggered allocations...
Let's sumup how they can impact your code. So should we switch from list to arrays for every time critical tasks ?

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
- Array resizing may imply array regrow and copy which is very costy in theory. In practice, as its backed by native functions, these cost are usually bogus BUT some use cases will trigger them ( .unshift ).
- Lose the ergonomic simplicity of lists.

### Benchmark :
- Benching traversal : http://try.haxe.org/#63c01
  { js_browser:{arr:0.087, list:0.148}}, {swf_browser:{arr:0.081, list:0.137}, {cpp:{arr:0.01, list:1.4}} }, the numbers varies very much through executions.
  Js or swf, At low data size, results are indistinguishable, one has to go around 500k items to really have significant differences. Good to know that each run show different numbers, this is simply because those structures are so lightweight, that cache misses, and GC hit will just break timing. Around 500k hit, array become earnestly faster ( by factor 1.5) but calling this a "real world use case" would be preposterous. On Cpp, array is much much faster.

- Insertions : 
  - Random insert before /after : http://try.haxe.org/#35242
  Result { js_browser:{arr:29, list:0.46}}, {swf_browser:{arr:1.97,list:0.021}, {cpp:{arr:3.47, list:0.024}} }
  On js and swf, array clearly loses ( by a 10 factor ). Js is much slower than swf on my chrome. Cpp is also slower.
  This is just conclusive that list have a meaning ( inserting bothways ) and array is not very ok for that. The Js numbers are still a little frightenning though...

  - Insert at "designed position": http://try.haxe.org/#903dd
  On low volumes, results are indistinguishable, around 100k, we start to see differences.
  Result { js_browser:{arr:0.06, list:0.37}}, {swf_browser:{arr:0.074,list:0.387}, {cpp:{arr:0.059, list:1.88}} }
  Once again, array becomes consistently faster ( by a 3 factor ) at very high numbers. This is a classical "pimp my compiler" use case so no surprise here.

### Conclusions
List and Arrays have clearly defined semantics and very different use cases. Latest efforts to inline List properly have bear fruits and they are now very competitive. This is a good lesson. List's are now okay to work with and could be changed to array if the need is real. If the question is should we switch, I can fairly answer that if your use case is not rethinked, then you might waste you time. As always, measuring for your use is the only valid answer. Frankly if you extract some lore from the big numbers, you are wrong, real world often works at low volumes speeds :)
Congrat to the team to fixing this historical problem !

