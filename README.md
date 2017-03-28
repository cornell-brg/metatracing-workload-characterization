# Cross-Layer Characterization of Meta-Tracing JIT Performance

## Cross-layer hints

Implemented
[here](https://github.com/cornell-brg/pypy/blob/master/rpython/rlib/pyxcel/hook.py).
 - [Blackhole hints](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/metainterp/blackhole.py#L327-L340)
 - Tracing hints:
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/metainterp/pyjitpl.py#L2374),
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/metainterp/pyjitpl.py#L2392),
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/metainterp/pyjitpl.py#L2412),
   and [here](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/metainterp/pyjitpl.py#L2435)
 - GC hints:
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/memory/gc/incminimark.py#L1613),
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/memory/gc/incminimark.py#L1814),
   [here](https://github.com/cornell-brg/pypy/blob/master/rpython/memory/gc/incminimark.py#L2198),
   and [here](https://github.com/cornell-brg/pypy/blob/master/rpython/memory/gc/incminimark.py#L2414)
 - IR-level hints:
   [usage](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/backend/x86/regalloc.py#L372) and
   [implementation](https://github.com/cornell-brg/pypy/blob/master/rpython/jit/backend/x86/assembler.py#L2765-L2767)
 - Application-level hints: I think this is in an older version of this
   repo. I'll find the code for that and merge it into this PyPy repo.

## Benchmarks

 - PyPy benchmark suite: PyPy, PyPy-nojit, CPython
 - Computer Languages Benchmarks Game:
    * Python: PyPy, PyPy-nojit, CPython
    * C/C++:  GCC
    * Racket: Racket, Pycket

## Title

 - Cross-Layer Characterization of Meta-Tracing JIT Performance (first
   choice)
 - Workload Characterization of Meta-Tracing JIT Using Cross-Layer Hints

## Introduction

 - Productivity of Dynamic languages
 - Productivity/Performance gap of dynamic languages
 - Effort of building JIT compilers for new languages
 - Meta-JIT and Meta-tracing
 - Why is it difficult to do cross-layer characterization
 - Overview

## Meta-Tracing JIT and RPython

 - Effort of building JIT compilers for new languages and Meta-JIT
 - Why is it challenging to do characterization

## Methodology

 - Cross-layer Hints (CLHints)
   CLHints stack:
    * Application
    * Bytecode/AST interpreter
    * RPython runtime
    * Trace (bridge info etc.)
    * IR nodes
    * Assembly
 - Kinds of CLHints; types of analysis we can do with this
 - Application-level CLHints (find an example use case)
 - PyPylogs + Pin

## IR-Level Characterization

 - Hit count vs. trace length
   * Takeaway: some benchmarks have few traces that are used frequently,
               but many benchmarks have many traces that are also long Java
               people have observed something similar (find cites), all
               programs are flat
 - Dynamic IR node / assembly histogram -- instruction distribution
   (branches, mem ops etc)
   * Takeaway: most IR nodes are cheap, expensive IR nodes are rare

## RPython phase analysis (CLHints from RPython runtime)

 - Early phase breakdowns
   * Takeaway: At warmup, blackhole interpreter execution is significant
               (look into conventional wisdom) -- guard paper: guards don't
               fail often; arxiv draft: warmup (TODO: cfbolz arxiv)
               behavior, reasons why warmup is slow
   * Takeaway: Until having a good JIT coverage, frequent guard failures
 - AOT-compiled Function calls back to the interpreter and break down
   which functions called
   * Takeaway: functional calls occupy a very hight % of execution time.
               We expect realistic benchmarks to spend more time here
   * Takeaway: many of the functions are due to fundamental Python niceties
               (e.g. lists, dicts, strings etc) -- how to mitigate the
               overheads by using cheaper non-Python data structures. JIT
               doesn't know about what rpython code is trying to do.
               e.g.:
               ```
               if a not in l:
                 l[a] = foo
               ```
               will incur in two lookups. Distinguish rpython versus python
               datastructures

## Microarchitectural measurements

 - Compare benchmarks game C/C++ IPC to Python
   * Takeaway: The IPC variation in Python is less than C/C++
   * Takeaway: IPC across CPython, PyPy w/o JIT and w/ JIT are similar
   * Takeaway: IPC is comparable to C/C++, the performance of Python is due
               to number of instruction (doing more stuff)
 - Branches / instruction
   * Takeaway: BPI is mostly constant w/ and w/o JIT
 - Branch miss rate
   * Takeaway: branch miss rate with JIT is lower with JIT 


## Removal of Safe Guards (tracing jits might have more guards)

 - Remove guards that never fail and code that feeds just to those
   * Takeaway: speedup by removal is minimal
 - Estimate removing guards that rarely fail
   * Takeaway: even unsafe guard removal doesn't give significant speedup

## Related Work

 - emphasize why this is difficult to do
 - methodology is a contribution
 - Southern, G. and Renau, J. Overhead of Deoptimization Checks in the V8
   JavaScript Engine. IISWC 2016.

## Roadmap

 - 3/27-4/2: Get entire set of benchmarks and languages/interpreters working
 - 4/3-4/9: Microarchitectural measurements using perf
 - 4/10-4/16: Safe guard removal
 - 4/17-4/23: Phase analysis
 - 4/24-4/30: Writing
 - 5/1-5/7: Writing
 - 5/8-5/14: Writing
 - 5/26: Abstract deadline
 - 6/2: Submission deadline

