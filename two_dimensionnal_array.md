
Abstract :

There are some issues processing bi dimensionnal array under Array<Array<T>>. In a more general way, nearly all languages have problems because this structure of data generates nested arrays.

```
var a :Array<Array<Float>> = [[],[]]

will generate one array comprising two arrays.

```
Severity:
- Very variable depending on the target cache size, branch predictor effectiveness etc... sometime the optimising will not produce results. On desktop the gain will be negligible, on mobile, it _can_ be a game changer. 

Problem:
- Each array will generate a new pointer leading in cache misses at array accesss time
- Arrays are resizable leading in branch tests and bracnh misses, on mobile the branch predictor is bad so slow down will ensue
- As of today, Haxe will generate Array<Dynamic> at least on cpp so slowdown will ensue.
- Natural storage would be to store arr[x][y] but sadly for cache coherency, it is often way faster to store a[y][x]...

Solution:
- Use haxe.ds.Vector of one dimension whenever possible.
- If one dimension resizing is necessary you can fallback to linear Array of dimension * (scaling dimension )
- If Both dimension resizing is needed, you are screwed and should engineer something :)

Requirement:
- All target except js, where haxe.ds.vector is backed by array
 
Benefits :
- Better cache coherency
- Less allocations


Snippet :

In the snippet, the vector usually performs faster, it could perform another magnitude faster by caching the indexes computation when you do your computations.
The difference in speed will usually depend on the performance of the cache and branch prediction, so "usually" it performs better on mobile.

http://try.haxe.org/#7eA51
