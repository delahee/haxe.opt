
Status : Ok for cpp and swf, discussion with Haxe Foundation about how to proceed for js.

Abstract : 

For small objects, without static analyzer the compiler can actually optimize whole classes instances so that they get inlined into local variables.

Requirements : 
- target cpp
- target swf

Gains :
- Code will not generate small allocations
- Instances are stack variables which are usually stored in registers of very fast data cache. The code may execute way faster.
 
Example :
```

class VecInl{
    public var x : Float;
    public var y : Float;
    public var z : Float;
    public var w : Float;

    public inline function new(x:Float,y:Float,z:Float,w:Float){
        this.x=x;this.y=y;this.z=z;this.w=w;
    }
    
    public inline function add(v:VecInl):VecInl{
        return new VecInl(x+v.x,y+v.y,z+v.z,w+v.w);
    }
    
    public inline function mul(s:Float){
        return new VecInl(x*s,y*s,z*s,w*s);
    }
    
    public inline function mad(s:Float,v:VecInl){
        return new VecInl(x*s+v.x,y*s+v.y,z*s+v.z,w*s+v.w);
    }
    
    public inline function dot(v:VecInl):Float{
        return x * v.x + y * v.y + z * v.z + w * v.w;
    }
    
    public inline function cross(v:VecInl){
        return new VecInl(y * v.z - z * v.y, z * v.x - x * v.z,  x * v.y - y * v.x, 1);
    }
    
    public inline function toString(){
        return '$x $y $z $w';
    }
    
    public static inline function random(){
        return new VecInl(Math.random(),Math.random(),Math.random(), 1);
    }
}


class Test {
    static function main() {
        var v0 = new VecInl(Math.random(),2,3,1);
        var v1 = new VecInl(1,2,3,1);
        var v2 = VecInl.random();
        var v3 = VecInl.random();
        
        var p0 = v1.dot( v0 );
        var p1 = v2.dot( v3 );
        var str = Std.string(p0+p1);
        trace(str);
    }
}
```

Snippet : 
http://try.haxe.org/#42EBa


