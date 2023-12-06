Sometimes if you create a tag element in javascript and then you attach some even listener to it (for example, click) the event doesn't work.
I faced this issue several times. It turns out that if you really checked your script in an isolated environment (for example, jsfiddle, but can be anything) and it was working, it REALLY means that
your environment affects the overall script exection in your system. It basically means there is another script (somewhere deep in your system, or vedor) that does something to the tag that you were working with.
The debug approach is pretty straight-forward. You need to find the way to disable .js files one by one loaded on the page you have the problem.

Ok, this way we can isolate a problem to a single file, but what is the actual problem causing your tag to behave differently?
Imagine that in another file someone was searching for a tag (your tag) and replacing some text in it. Looks like a pretty common task, right?
It turns out changing content/text of a tag is not trivial and there are specifics you should know.
Usually if you need to replace text precisely in a tag and you know for sure there can only be text in the tag, you can use direct properties to change the text.
However frequently you have a hierarchical strutcture in a tag. There you can only change nodes that are of text type. Read here:
https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType

Example:
Suppose you have a tag selectNew;

1) selectNew.textContent = children[id].text;

If you had options in the selectNew tag all of them will be replaced by the children[id].text.

2) selectNew.firstChild.textContent = children[id].text;

This modification successfully changes text for the first child tag inside of the selectNew tag preserving tags and their behaviour.
