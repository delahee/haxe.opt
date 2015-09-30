
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

js result :
```
console.log("Haxe is great!");
var a = new A();
a.foo();
var b = new B();
b.foo();
b.fob = "a";
```
cpp result : 
```
Void Test_obj::main()
{
{
		HX_STACK_FRAME("Test","main",0x9c6c95b5,"Test.main","Main.hx",32,0x087e5c05)
		HX_STACK_LINE(33)
		Dynamic tmp = hx::SourceInfo(HX_HCSTRING("Main.hx","\x05","\x5c","\x7e","\x08"),33,HX_HCSTRING("Test","\x72","\xf4","\xd2","\x37"),HX_HCSTRING("main","\x39","\x38","\x56","\x48"));		HX_STACK_VAR(tmp,"tmp");
		HX_STACK_LINE(33)
		::haxe::Log_obj::trace(HX_HCSTRING("Haxe is great!","\x50","\xb5","\x52","\x60"),tmp);
		HX_STACK_LINE(35)
		::A a = ::A_obj::__new();		HX_STACK_VAR(a,"a");
		HX_STACK_LINE(36)
		a->foo();
		HX_STACK_LINE(39)
		::B b = ::B_obj::__new();		HX_STACK_VAR(b,"b");
		HX_STACK_LINE(40)
		b->foo();
		HX_STACK_LINE(41)
		b->__FieldRef(HX_HCSTRING("fob","\xf9","\xc6","\x4d","\x00")) = HX_HCSTRING("a","\x61","\x00","\x00","\x00");
	}
	return null();
}
```

so everything is fine, if you really want you users to be able to add data dynamically to classes, "implements Dynamic" can be used safely.

