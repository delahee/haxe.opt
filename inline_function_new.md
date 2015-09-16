
TODO: contact the hf about the problems leveraged by this content

Abstract : 

For small objects, without static analyzer the compiler can actually optimize whole classes instances so that they get inlined into local variables.

Requirements : 
- None

Gains :
- Code will not generate small allocations
- Instances are stack variables which are usually stored in registers of very fast data cache. The code may execute way faster.
 
Example :
```class Vec{
    var x : Float;
    var y : Float;
    var z : Float;
    var w : Float;

    public inline function new(?x=0,?y=0,?z=0,?w=1){
    }
}
```

Proof : 
http://try.haxe.org/#42EBa


