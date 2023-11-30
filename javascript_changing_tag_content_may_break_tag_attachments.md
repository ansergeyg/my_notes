Sometime if you create a tag element in javascript and then you attach some even listener to it (for example, click) the event doesn't work.
I faced this issue several times. It turns out that if you really checked your script in an isolated environment (for example, jsfiddle, but can be anything) and it was working, it REALLY means that
your environment affects the overall script exection in your system. It basically means there is another script (somewhere deep in your system, or vedor) that does something to the tag that you were working with.
The debug approach is pretty straight-forward. You need to find the way to disable .js files one by one loaded on the page you have the problem.
Ok, this way we can isolate a problem to a single file, but what is the actual problem causing your tag to behave differently?
Imagine that in another file someone was searching for a tag (your tag) and replacing some text in it.
