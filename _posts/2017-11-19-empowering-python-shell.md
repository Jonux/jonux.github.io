---
layout: post
title: Empowering Python Shell
tags:
- Python Shell
- IPython
- Tutorial
keywords:
- python shell,ipython,python3,debugging
---

<h1>Empowering Python Shell</h1>

<p>Many developers underestimate the power of Python shell. The iPython shell provides interactive tools for code debugging. I haven't found good articles about Python's shell usage, so I decided to gather up some helpful commands while working with Python shell.
</p>

<p>
My background is from C++ programming, so getting familiar with Python shell for more complex debugging tasks took me a while. I hope that this article enlightens and inspires you to use Python shell for challenging tasks. You will see that Python shell provides an amazingly easy way to test multiple edge cases.
</p>

<img src="/assets/images/python_unboxed.png" alt="http://resources.arcgis.com/de/communities/python/" />

<blockquote>
  <h4>So, let's open IPython's toolbox and utilize its interactive tools!</h4>
</blockquote>


<h3 id="table">1. Moving in Python Shell</h3>

<p>
The most handy commands to me have been basic shell commands, like jumping word by word, autocompleting by <code>tab</code> and browsing though previous commands by using <code>UP</code> and <code>DOWN</code> keys. These are the basics that every shell user knows. Nevertheless, iPython shell provides extensions to these basic functionalities; For example, command <code>pri[press UP]</code> will show you previous commands which start with a prefix string <code>pri</code>. This trick becomes extremely handy when a programmer copy-pastes a code snippet from an editor and remembers that the snippet is intended with x number of spaces. In that case, we could add a space and press up and scroll through all our previous code snippets.
</p>

<table>  
<tbody>  
<tr>  
<th style="width:150px;">Key (Mac)</th>  
<th>Command</th>  
</tr>  
<tr>
<td>Alt+P / Up</td>  
<td>Previous history command</td>  
</tr>  
<tr class="even">  
<td>Alt+N / Down</td>  
<td>Next history command</td>
</tr>  
<tr class="odd">  
<td>Ctrl+A / Home</td>  
<td>Jump to the beginning of the line</td> 
</tr>  
<tr class="even"> 
<td>Ctrl+E / End</td>  
<td>Jump to the end of the line</td> 
</tr>
<tr class="even"> 
<td>Alt+Left</td>  
<td>Jump a word backward</td> 
</tr>
<tr> 
<td>Alt+Right</td>  
<td>Jump a word forward</td> 
</tr>
<tr>  
<td>Ctrl+R</td>  
<td>Search from command history</td> 
</tr>  
<tr>  
<td>Tab</td>  
<td>Autocomplete command</td> 
</tr>  
</tbody>  
</table>
<br />

<p>
Another problem that I have noticed developers have trouble with is moving code snippets between the code editor and python shell. A common solution is to remove leftern extra spaces before moving the snippet to the shell if the first copy-pasting didn't work out. This approach is extra work and blocks our previous whitespace history trick. The IPython shell is checking the intention level from the first line and it also ignores python shell's characters. Hence, we should select an area which does not contain any extra whitespace lines before the relevant code. The selected area should include all whitespace chars from the first line to keep the snippet's internal intention attach (see example below).
</p>

<img width="480px" src="/assets/images/ipython_select1.png" alt="Code snippet copy-pasting" />


<h3 id="table">2. IPython Magic Commands</h3>

<p>
Interactive Python Shell provides a few extra key features on top of the basic Python compiler. I think the most relevant ones are <code>%magic</code> commands that helps many frequently needed simple tasks, such as measuring time of a function, profiling code's performance, and reloading Python objects.
</p>


{% highlight python %}
# iPython reference
>>> %quickref

# Print type of a variable
>>> a=1; a?
Type:        int
String form: 1
Docstring: ...

# Show object's source code
>>> import enum
>>> %psource enum.IntEnum
class IntEnum(int, Enum):
    """Enum where members are also (and must be) ints"""

# Running shell commands in Python shell (start command with !):
>>> !echo "hello"
hello

# Capturing the shell command's result into a python variable:
# Example, load 'data.txt' into a python variable
>>> !!cat data.txt
>>> data = _

# Save a variable over a python session?
>>> %store data
>>> %store  # list stored variables
# Restart python shell
>>> %store -r  # restore variables

# Fetch from history, same as Ctrl+R
>>> %hist -g test

# Modified the file after opening the shell? No worries, just reload it:
>>> %loadpy tests/test.py

# The python module has recursive dependencies? Reload it recursively with all dependencies:
>>> from IPython.lib.deepreload import reload as dreload
>>> import itertools  # example module
>>> dreload(itertools)
<module 'itertools' (built-in)>

# Measure time of a command
>>> %timeit x=10**50
100000000 loops, best of 3: 14.4 ns per loop

# Run code with cProfiler to find out bottlenecks:
>>> import re
>>> %prun re.search(r'python', 'python shell')
6 function calls in 0.000 seconds

Ordered by: internal time

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
    1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
    1    0.000    0.000    0.000    0.000 <string>:1(<module>)
    1    0.000    0.000    0.000    0.000 re.py:179(search)
    1    0.000    0.000    0.000    0.000 {method 'search' of '_sre.SRE_Pattern' objects}
    1    0.000    0.000    0.000    0.000 re.py:286(_compile)
    1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
{% endhighlight %}


<h3 id="table">3. Practical Python Debugging Commands</h3>

<p>In addition to interactive Python's magic functions Python itself provides also handy functions for debugging purposes. The most known Python debugging tool is Python's <a href="https://docs.python.org/3.6/library/pdb.html">pdb-library</a>. It provides basic debugging tools and stack trace of the problems. I recommend also trying out <a href="https://pypi.python.org/pypi/pdbpp/">pdb++-library</a> which is a superset of the default debugger. The main benefit of pdb++ is an improved syntax highlighting.</p>

<p>Python also provides other useful libraries for debugging, such as, <a href="https://docs.python.org/3.6/library/unittest.mock-examples.html">Mock</a> for faking objects, <a href="https://docs.python.org/3.6/library/dis.html">Dis</a> for disassembling code, and <a href="https://docs.python.org/3.6/library/inspect.html">Inspect</a> for checking live information from any objects. See examples below:


</p>

{% highlight python %}
# Using Python debugger
>>> import pdb; pdb.Pdb(skip=['django.*']).set_trace()

# Create a test object by using Mock-library
>>> import mock
>>> obj = mock.Mock()
>>> obj.x = 1

# Check object's properties and values
>>> obj.__dict__
{'_mock_call_args': None, '_mock_call_args_list': [], '_mock_call_count': 0, '_mock_called': False, '_mock_children': {}, '_mock_delegate': None, '_mock_methods': None, '_mock_mock_calls': [], '_mock_name': None, '_mock_new_name': '', '_mock_new_parent': None, '_mock_parent': None, '_mock_return_value': sentinel.DEFAULT, '_mock_side_effect': None, '_mock_unsafe': False, '_mock_wraps': None, '_spec_class': None, '_spec_set': None, '_spec_signature': None, 'method_calls': [], 'x': 1}

# Object's public properties
 >>> dir(obj)
['assert_any_call', 'assert_called_once_with', 'assert_called_with', 'assert_has_calls', 'assert_not_called', 'attach_mock', 'call_args', 'call_args_list', 'call_count', 'called', 'configure_mock', 'method_calls', 'mock_add_spec', 'mock_calls', 'reset_mock', 'return_value', 'side_effect', 'x']

# Disassemble functions
>>> import dis
>>> from typing import Any
>>> def func(obj: Any):
        return obj.x ** 2

>>> dis.dis(func)
	0 LOAD_FAST                0 (obj)
	2 LOAD_ATTR                0 (x)
	4 LOAD_CONST               1 (2)
	6 BINARY_POWER
	8 RETURN_VALUE

# Inspect function's arguments
>>> from inspect import signature
>>> sig = signature(func)
>>> sig.parameters['obj'].annotation
typing.Any
{% endhighlight %}

<h2> Final words </h2>
<p>
Hopefully this blog inspired you to use Python shell more in your projects and help you to test possible edge cases in early stage. Early edge case testing improves code quality and feels rewarding at least to me. I recommend also to document some of these manually checked test cases as doctests in your project because then you have up-to-date documentation and tests about the problem. Overall, I hope that this blog post was helpful and I wish you delightful debugging time!
</p>
<hr/>
Check out IPython's <a href="http://ipython.readthedocs.io/">official documentation</a>!<br/>
Check out how to extend Python shell's <a href="http://algorithmicallyrandom.blogspot.com/2009/09/tab-completion-in-python-shell-how-to.html">autocomplete</a>!<br/>
For more advanced tricks and IPython customization, I recomment to check out 
<a href="https://github.com/ipython/ipython/wiki/Cookbook%3A-Index">IPython cookbook</a>!<br/>

<br/>

