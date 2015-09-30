
From a performance perspective, "implement Dynamic" could be a problem if it was badly implemented as all typed access could get turned into dynamic accesses. 

In practice in js and cpp , typed members are correctly typed and accesses and dynamic ones are effectively accessed by reflection.

```
class A {
    public function new(){}
    public function foo(){
        trace("bar");
    }
}

class B implements Dynamic {
    public function new(){}
    public function foo(){
        trace("bar");
    }
}

class Test {
    static function main() {
        trace("Haxe is great!");
        
        var a = new A();
        a.foo();
        
        
        var b = new B();
        b.foo();
        b.fob = "a";
    }
}
```

http://try.haxe.org/#1e84A

so everything is fine, if you really want you users to be able to add data dynamically to classes, "implements Dynamic" can be used safely.

