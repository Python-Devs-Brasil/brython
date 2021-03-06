Frequently asked questions
--------------------------

__Q__ : _what is the performance of Brython compared to Javascript, or to other solutions that allow using Python in the browser ?_

__A__ : compared to Javascript, the ratio is naturally very different from one program to another, but it is somewhere between 3 to 5 times slower. A Javascript console is provided in the distribution or on [the Brython site](http://brython.info/tests/js_console.html), it can be used to measure the execution time of a Javascript program compared to its equivalent in Python in the editor (unchecking the "debug" box)

The diffence is due to two factors :

- the time to translate Python into Javascript, on the fly in the browser. To give an idea, the module datetime (2130 lines of Python code) is parsed and converted to Javascript in 0,5 second on an ordinary PC
- JavaScript code generated by Brython must be compliant with the specifications of Python, including the dynamic nature of the search attributes, which leads to unoptimized Javascript code

Compared to other solutions that translate Python to Javascript, some [fanciful comparisons](http://pyppet.blogspot.fr/2013/11/brython-vs-pythonjs.html) mention a ratio of 1 to 7500 against Brython : no information is given, but it's obvious that they don't compare similar situations ; in the same conditions (running a script in a web browser) it's hard to imagine how to be faster than native Javascript...

Moreover, the example presented in this page is :

    N = 100000
    a = 0
    for i in range(N):
        a += 1

that PythonJS naively translates to

    var N=100000;
    var a=0;
    for(var i=0;i<N;i++){
        a += 1
    }

If the code is modified like this :

    def range(N):
        return ['spam', 'eggs']
    
    for i in range(100000):
        print(i)

Brython gives the correct result, while PythonJS still translates the loop as if `range()` was the built-in function of the same name

Needless to say, in the case when `a` is a list instead of an integer, the results produced by PythonJS are not exactly what a Python developer would expect...

Brython tries to be as fast as possible and, as the examples in the gallery show, is _fast enough_ for most of the use cases of its target - web front end developement. But it also aims at covering 100% of Python syntax, which includes producing the same error messages as CPython, even if this leads to a slower Javascript.

I haven't found serious comparisons between the solutions that can be found in [a list](http://stromberg.dnsalias.org/~strombrg/pybrowser/python-browser.html) maintained by Dan Stromberg. Nothing proves that the Javascript code generated by solutions written in Python are faster than the one generated by Brython. And the development cycle with solutions written in Python such as Pyjamas / pyjs is obviously longer than with Brython

__Q__ : _I see a lot of 404 errors in the browser console when I run Brython scripts, why is that ?_

__A__ : this is because of the way Brython implements the "import" mechanism. When a script has to import module X, Brython searches for a file or a package in different directories : the standard library (directory libs for Javascript modules, Lib for Python modules), the directory Lib/site-packages, the directory of current page. For that, Ajax calls are sent to the matching urls ; if the file is not found, the 404 error message is written in the browser console, but the error is caught by Brython which goes on searching until it finds the module, or raises `ImportError` if all paths have been tried with no result

__Q__ : _why use the operator `<=` to build the tree of DOM elements ? This is not pythonic !_

__A__ : Python has no built-in structure to manipulate trees, ie to add "child" or "sibling" nodes to a tree node. For these operations, functions can be used ; the syntax proposed by Brython is to use operators : this is easier to type (no parenthesis) and more readable

To add a sibling node, the operator `+` is used

To add a child, the operator `<=` was chosen for these reasons :

- it has the shape of a left arrow ; note that Python function annotations use a new operator `->` that was chosen for its arrow shape
- it looks like an augmented assignment because of the equal sign
- it can't be confused with "lesser or equal" because a line with `document <= elt` would be a no-op if it was "lesser or equal", which is always used in a condition or as the return value of a function
- we are so used to interpret the 2 signs `<` and `=` as "lesser or equal" that we forget that they are a convention for programming languages, to replace the real sign `≤`
- in Python, `<=` is used as an operator for sets with a different meaning than "lesser or equal"
- the sign `<` is often used in computer science to mean something else than "lesser than" : in Python and many other languages, `<<` means left shift ; in HTML tags are enclosed with `<` and `>`
- Python uses the same operator `%` for very different operations : modulo and string formatting

__Q__ : What browsers are compatible with Brython?

__A__ : Below is a list of browsers we support:

// todo: get images from http://www.paulirish.com/2010/high-res-browser-icons/

<table border="1">
  <tr><td>Chrome</td><td>FireFox</td><td>IE</td><td>Opera</td><td>Safari</td><td>Android Browser</td><td>Chrome for Android</td><td>iOS Safari</td><td>Opera Mini</td></tr>
  <tr><td>36+</td><td>31+</td><td>9+</td><td>26+</td><td>5.1+</td><td>4.1+</td><td>39+</td><td>7.1+</td><td>8+</td></tr>
</table>
