

<p align="center">
    # Python Performance Profiling <br>
  <img src="https://github.com/akkefa/pycon-python-performance-profiling/blob/master/imgs/1.png?raw=true" alt="Python performance profiling"/>
</p>

### HELLO!

I am **Ikram Ali**

➔ Data Scientist @ Arbisoft
➔ Working on Deep learning projects for Kayak
➔ Github.com/akkefa
➔ Linkedin.com/in/akkefa


## What is Profiling?


#### Profiling Definition?

➔ Measuring the execution time.
➔ Insight of run time performance of a given piece of
code.
➔ Frequently used to optimize execution time.
➔ Used to analyze other characteristics such as
memory consumption.


## What is Python Profiling?

# Measure Performance


### Why Profile?

➔ Why is this program slow?
➔ Why does it slow my computer to a crawl?
➔ What is actually happening when this code executes?
➔ Is there anything I can improve?
➔ How much memory consumed by program?
➔ How much time taken by each function execution?

```
You can use a profiler to answer questions like these:
```

### Why You should care about Performance

➔ “If You Can’t Measure It, You Can’t Manage It.”

➔ Writing efficient code saves money in modern "cloud
economy" (e.g. you need fewer VM instances).

➔ Even if you don't use clouds, a particular problem
domain can have strict performance requirements
(e.g. when you have to process a chunk of data in
time before the next chunk arrives).


## Available options for measuring

## Performance

##### ➔ Command Line

##### ➔ time Module

##### ➔ timeit Module

##### ➔ cProfile Module


##### Command Line

The **time** command is available in *nix systems.

$ **time** python some_program.py

real 0m4.536s
user 0m3.411s
sys 0m0.979s


##### Command Line

```
➔ Easy to use
```
```
PROS
```
➔ Very limited information
➔ Not very deterministic
➔ Not available on Windows

```
CONS
```

##### Python time Module

```
Naive approach: time.time() statements
```
```
import time
initial_time = time.time()
time.sleep(1)
final_time = time.time()
print('Duration: {}'.format(final_time - initial_time))
```
```
Duration: 1.
```

##### Python time Module

```
➔ Easy to use
➔ Simple to understand
```
```
PROS
```
➔ Very limited information
➔ Not very deterministic
➔ Manual code modification and analysis

```
CONS
```

##### Python timeit Module

**import** timeit

print('Plus:', timeit.timeit("['Hello world: ' + str(n) for n in range(100)]", number=1000))
print('Format:', timeit.timeit("['Hello world: {0}'.format(n) for n in range(100)]",
number=1000))
print('Percent:', timeit.timeit("['Hello world: %s' % n for n in range(100)]", number=1000))

Better approach: timeit

Plus: 0.
Format: 0.
Percent: 0.


##### timeit Module

```
➔ Easy to use
➔ Simple to understand
➔ Measure execution time of small code snippets
```
```
PROS
```
➔ Simple code only
➔ Not very deterministic
➔ Have to manually create runnable code snippets
➔ Manual analysis

```
CONS
```

##### cProfile Module

Best approach: cProfile
➔ Python comes with two profiling tools, profile and cProfile.
➔ Both share the same API, and should act the same.

3 function calls in 0.000 seconds
Ordered by: standard name
ncalls tottime percall cumtime percall filename:lineno(function)
1 0.000 0.000 0.000 0.000 <string>:1(<module>)
1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler'}

>>> **import** cProfile
>>> cProfile.run('2 + 2')


##### Running a script with cProfile

# slow.py
**import** time
def main():
sum = 0
for i in range(10):
sum += expensive(i // 2)
return sum

def expensive(t):
time.sleep(t)
return t

if __name__ == '__main__':
print(main())


25 function calls in 20.030 seconds

Ordered by: standard name

ncalls tottime percall cumtime percall filename:lineno(function)
10 0.000 0.000 20.027 2.003 slow.py:11(expensive)
1 0.002 0.002 20.030 20.030 slow.py:2(<module>)
1 0.000 0.000 20.027 20.027 slow.py:5(main)
1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler'objects}
1 0.000 0.000 0.000 0.000 {print}
1 0.000 0.000 0.000 0.000 {range}
10 20.027 2.003 20.027 2.003 {time.sleep}

python -m cProfile slow.py


##### cProfile sort by options

```
For the number of calls
```
**ncalls**

```
for the total time spent in the given function
```
**tottime**

```
is the quotient of tottime divided by ncalls
```
**percall**

```
is the cumulative time spent in this and all subfunctions.
```
**cumtime**

```
is the quotient of cumtime divided by primitive calls
```
**percall**

**filename:lineno(function)**
provides the respective data of each function


**cProfile result sorted by tottime**

25 function calls in 20.015 seconds

Ordered by: **internal time**

ncalls **tottime** percall cumtime percall filename:lineno(function)
10 **20.015** 2.001 20.015 2.001 {built-in method time.sleep}
1 **0.000** 0.000 0.000 0.000 {built-in method builtins.print}
1 **0.000** 0.000 20.015 20.015 slow.py:6(main)
10 **0.000** 0.000 20.015 2.001 slow.py:13(expensive)
1 **0.000** 0.000 20.015 20.015 slow.py:3(<module>)
1 **0.000** 0.000 20.015 20.015 {built-in method builtins.exec}
1 **0.000** 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler' objects}

python -m cProfile -s tottime slow.py


**cProfile result sorted by ncalls**

25 function calls in 20.015 seconds

Ordered by: **call count**

**ncalls** tottime percall cumtime percall filename:lineno(function)
**10** 20.020 2.002 20.020 2.002 {built-in method time.sleep}
**10** 0.000 0.000 20.020 2.002 slow.py:13(expensive)
**1** 0.000 0.000 20.020 20.020 {built-in method builtins.exec}
**1** 0.000 0.000 0.000 0.000 {built-in method builtins.print}
**1** 0.000 0.000 20.020 20.020 slow.py:6(main)
**1** 0.000 0.000 20.020 20.020 slow.py:3(<module>)
**1** 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler' objects}

python -m cProfile -s ncalls slow.py


##### Easiest way to profile Python code

def main():
sum = 0
for i in range(10):
sum += expensive(i // 2)
return sum
def expensive(t):
time.sleep(t)
return t

if __name__ == '__main__':
**pr = cProfile.Profile()
pr.enable()**
main()
**pr.disable()
pr.print_stats()**


25 function calls in 20.030 seconds

Ordered by: standard name

ncalls tottime percall cumtime percall filename:lineno(function)
10 0.000 0.000 20.027 2.003 slow.py:11(expensive)
1 0.002 0.002 20.030 20.030 slow.py:2(<module>)
1 0.000 0.000 20.027 20.027 slow.py:5(main)
1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler'objects}
1 0.000 0.000 0.000 0.000 {print}
1 0.000 0.000 0.000 0.000 {range}
10 20.027 2.003 20.027 2.003 {time.sleep}

**cProfile output**


### We can also save the output!

if __name__ == '__main__':
pr = cProfile.Profile()
pr.enable()
main()
pr.disable()
**pr.dump_stats("profile.output")**


## How do we use the profiling

## information?


##### pstats Module

➔ You can use pstats to format the output in various ways.
➔ pstats provides sorting options. **( calls, time, cumulative )**

**import pstats**

p = pstats.Stats("profile.output")
p.strip_dirs().sort_stats(" **calls** ").print_stats()


23 function calls in 20.019 seconds

Ordered by: call count

ncalls tottime percall cumtime percall filename:lineno(function)
10 20.019 2.002 20.019 2.002 {built-in method time.sleep}
10 0.000 0.000 20.019 2.002 slow.py:14(expensive)
1 0.000 0.000 0.000 0.000 {built-in method builtins.print}
1 0.000 0.000 20.019 20.019 slow.py:7(main)

**pstats module Output**


#### An easy way to visualize cProfile

#### results

##### ➔ Snakeviz library

##### ➔ PyCallGraph library


##### SNAKEVIZ

```
pip install snakeviz
```
➔ Snakeviz provides two ways to explore profiler data
➔ Summaries Times
➔ You can choose the sorting criterion in the output
table

```
$ snakeviz profile.output
```

##### SNAKEVIZ Browser View


##### PyCallGraph

```
pip install pycallgraph
```
➔ Visual extension of cProfile.
➔ Understand code structure and Flow
➔ Summaries Times
➔ Darker color represent more time spent.

```
$ pycallgraph graphviz -- python slow.py
```

##### Other profiling options

➔ line_profiler will profile the time individual lines of code take
to execute.
➔ https://github.com/rkern/line_profiler

```
Line profiler
```
➔ Monitoring memory consumption of a process.
➔ line-by-line analysis of memory consumption.
➔ https://pypi.org/project/memory_profiler/

```
Memory profiler
```

##### Live Example Interlude

```
https://github.com/akkefa/pycon-python-performance-profiling
```
```
Profiling Example Code
```

# Thank you.

## Questions?

```
Contact : mrikram1989@gmail.com
```

