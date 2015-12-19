
"I read your long postmortem about your #ld34 game. It'd be awesome to find out more how you managed to keep the GC at bay"
"I'm mainly working with @MonoGameTeam lately, but I used Haxe and hxcpp's GC has been a big problem."
"many haxe based frameworks do a lot of allocations during their rendering cycles, and the lack of struct doesn't help as well"

Hi Valeria !

You are totally right, most haxe frameworks are not production ready, especially for mobile, their allocation policy and some compiler bugs do combine for creepy situations.
They are usually "very good at rendering bunnies but not at real large scale game" management ( quote form Hugh Sanderson himself )

Still, with the motion twin team, we had this game ( https://play.google.com/store/apps/details?id=com.motiontwin.monsterhotel&hl=fr ) out on mobile.
It manages tons of sprites with many layers without major gc stalls. 
Same goes for Double Kick Heroes, the game exhibit very few GC stalls

So how did we get to here ? 

1- We used the "inline function new" construct a lot, it allows whole classes to go as local variables. This make these classes vent faster than C structs as locals are register rather than stack vars.
BUT there was a very major Haxe bug, that waits to be solved that impeach this construct to function properly during a long time. 
https://github.com/HaxeFoundation/haxe/pull/4558

There is an article about how to use it here : https://github.com/delahee/haxe.opt/blob/master/inline_function_new.md

2- We took weeks to profile the haxe compiler output properly, and forked the ancestor of heaps, h3d, i spent time tuning it and removing allocation, 
adding stack objects and better allocation policies. 
We also spent time converting ALL dynamic access to inline classes.
Dynamic acceses are the root of all haxe evil.

3- We profiled a lot with scout and cpp profilers, and i am used to the profiling jobs, i did it on consoles for years, so I developed a sense on 
how to detect bottleneck and cache misses. We mostly use micro benching (see https://github.com/delahee/h3d/blob/master/hxd/Profiler.hx and https://github.com/delahee/h3d/blob/master/hxd/DrawProfiler.hx) rather than stack benching because eliminating what takes overall time usually hints for big problems.

4- We tracked all allocations with scout and eliminated systemic allocations ie allocations that would occur each frame without render graph changes.

5- Very sadly, we spent a lot of time with the team to have various allocations removed and inline calls optimised. 
It was really a very painful and stressful and frustrating process since most of the time, we would call on an issue and it would only be solved when one of the compiler team would make use of the solution.
In the end we end up making fixes on our sit and not just sitting for the team to fix.

6- You can read the whole https://github.com/delahee/haxe.opt articles, maybe you'll learn something useful.

For mobile, so far the only production framework I used is h3d, heaps is really desktop/gles3 and I suspect Kha could be good but it's very low level.
I suspect monogame + haxe with "inline function new" to be a real blessing :)

In the end, I totally get that an indie team that do not want to get into profiling could skip haxe frameworks. 
They are really powerful but really require you to get engage with the team and be ready to hack bits here and there even in the compiler to get the job done.




