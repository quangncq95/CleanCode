## Function
### Small and do one thing
The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

Functions should do **one thing**. 
They should do it well.
They should do it only.

If a function does only those **steps** that are **one level below the stated name** of the function, then the function is doing **one thing**.

another way to know that a function is doing more than **one thing** is if you can **extract another function from it** with a **name** that is **not merely a restatement of its implementation**.
### Function arguments
The ideal number of arguments for a function is zero (niladic). Next comes one (monadic), followed closely by two (dyadic). Three arguments (triadic) should be avoided where possible. More than three (polyadic) requires very special justification—and then shouldn’t be used anyway.
#### Common monadic forms ( one argument )
There are two very common reasons to pass a single argument into a function:
- You may be asking a question about that argument, as in `boolean fileExists(“MyFile”)`
- You may be operating on that argument, transforming it into something else and returning it. For example,` InputStream fileOpen(“MyFile”)` transforms a file name `String` into an `InputStream` return value.

A somewhat less common, but still very useful form for a single argument function, is an event. In this form there is an input argument but no output argument. The overall program is meant to interpret the function call as an event and use the argument to alter the state of the system.
#### Flag arguments
Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!
#### Dyadic functions ( two arguments)
A function with two arguments is harder to understand than a monadic function. For example, `writeField(name)` is easier to understand than `writeField(output-Stream, name)`.

Dyads aren’t evil, and you will certainly have to write them. However, you should be aware that they come at a cost and should take advantage of what mechanims may be available to you to convert them into monads. For example:
- you might make the `writeField` method a member of `outputStream` so that you can say `outputStream. writeField(name`). 
- Or you might make the `outputStream` a member variable of the current class so that you don’t have to pass it. 
- Or you might extract a new class like `FieldWriter` that takes the `outputStream` in its constructor and has a write method.
#### Triad
Functions that take three arguments are significantly harder to understand than dyads. The issues of ordering, pausing, and ignoring are more than doubled. I suggest you think very carefully before creating a triad.
#### Argument objects
When a function seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Consider, for example, the difference between the two following declarations: 
Bad:
`Circle makeCircle(double x, double y, double radius); `
Good:
`Circle makeCircle(Point center, double radius);`
#### Verbs and keywords
Choosing good names for a function can go a long way toward explaining the intent of the function and the order and intent of the arguments.
Bad:
`write(name)`
Good:
`writeField(name)`

Bad:
`assertEquals(expected, actual)`

How many times have you put the `actual` where the `expected` should be? The two arguments have no natural ordering.
Good:
`assertExpectedEqualsActual(expected, actual)`
### Have no side effects
Side effects are lies. Your function promises to do one thing, but it also does other _hidden_ things. Sometimes it will make unexpected changes to the variables of its own class. Sometimes it will make them to the parameters passed into the function or to system globals. In either case they are devious and damaging mistruths that often result in strange temporal couplings and order dependencies.
#### Output arguments
In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.
Bad:
`appendFooter(s);`

Does this function append `s` as the footer to something? Or does it append some footer to `s`? Is `s` an input or an output?
Good:
`report.appendFooter();`
### Command query separation
Functions should either do something or answer something, but not both. Either your function should change the state of an object, or it should return some information about that object. Doing both often leads to confusion.
Bad:
```
public boolean set(String attribute, String value);
if (set("username", "unclebob"))...
```
Is it asking whether the “`username`” attribute was previously set to “`unclebob`”? Or is it asking whether the “`username`” attribute was successfully set to “`unclebob`”?
Good:
```
if (attributeExists("username")) { 
	setAttribute("username", "unclebob"); ... 
}
```
[CQRS Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)
### Prefer exceptions to returning error codes
Bad:
```
if (deletePage(page) == E_OK) { 
	if (registry.deleteReference(page.name) == E_OK) { 
		if (configKeys.deleteKey(page.name.makeKey()) == E_OK){ 
			logger.log("page deleted"); 
		} else { 
			logger.log("configKey not deleted"); 
		} 
	} else { 
		logger.log("deleteReference from registry failed"); 
	} 
} else { 
logger.log("delete failed"); return E_ERROR; 
}
```
When you return an error code, you create the problem that the caller must deal with the error immediately.
If you use exceptions instead of returned error codes, then the error processing code can be separated from the happy path code and can be simplified.
```
try { 
	deletePage(page); 
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey()); 
} catch (Exception e) { 
	logger.log(e.getMessage()); 
}
```
#### Extract try/catch blocks
`Try/catch` blocks are ugly in their own right. They confuse the structure of the code and mix error processing with normal processing. So it is better to extract the bodies of the `try` and `catch` blocks out into functions of their own.
```
public void delete(Page page) { 
	try { 
		deletePageAndAllReferences(page); 
	} catch (Exception e) { 
		logError(e); 
	} 
} 
private void deletePageAndAllReferences(Page page) throws Exception {
	deletePage(page); 
	registry.deleteReference(page.name); 
	configKeys.deleteKey(page.name.makeKey()); 
} 
private void logError(Exception e) { 
	logger.log(e.getMessage()); 
}
```
#### Error Handling is one thing
Functions should do one thing. Error handing is one thing. Thus, a function that handles errors should do nothing else.
