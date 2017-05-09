# Simulation runs

| *Benchmarks*    | *PYPYLOG* | *perf* | *papi* | *pin* | *time* |
| --------------- | --------- | ------ | ------ | ----- | ------ |
| Benchmarks game | (1)       | (2)    | (3)    | (4)   | (5)    |
| PyPy benchmarks | (6)       | (7)    | (8)    | (9)   | (10)   |

## (7)

```
  ./runner.py --use-perf --changed=<path-to-pypy>
```

## (9)

(Uses activity file gen pin options)

```
  ./runner.py --track-phases --changed=<path-to-pypy>
```

## (10)

Compare PyPy to CPython and PyPy w/o JIT (`pypy --jit off`).

```
  <python-baseline> ./runner.py --changed=<path-to-pypy>
```

## (5)

```
  cd bencher/bin
  ./bencher.py
```

## (2)

TODO(berkin): need cmdline flag for bencher for this

```
  cd bencher/bin
  ./bencher.py
```

## (3)

```
  ./run.sh
```

## Benchmarks Game

Need a specific version of gcc: 4.8.

## pin

### activity file gen

activity interval = 10K

```
  pin -t <path-to-pintools>/source/tools/pypy_phases/obj-intel64/pypy_phases.so -o <output> -activity-file <activity-file> -activity-interval <activity-interval> -- <program>
```

## perf

```
  perf stat -e instructions,cycles,branches,branch-misses -I 100 -x , -o <output> <program>
```

## PYPYLOG

```
  PYPYLOG=jit:<pypylog-output> pypy ...
```

## papi

```
   PYXCEL_SETTINGS=perf_hooks,perf_stats_filename=perf_out/$3.csv,perf_num_samples=$NUM_SAMPLES pypy ...
```

## TODO:

Common dir for gcc, pypy, pycket. Common dir for sim results.

Globally install racket.

Clean up benchmarksgame.

## Output dir

```
  /work/bits0/pyxcel/sim_outs/outs_<id>_<study>
  e.g.:
  /work/bits0/pyxcel/sim_outs/outs_0_10
```

## Simulation runs table

| *id* | *Start date/time* | *who* | *machine* | *st/mt* | *study* | *dir* | *pypy hash* | *pin hash* | *benchmark hash* | *notes* |
| ---- | ----------------- | ----- | --------- | ------- | ------- | ----- | ----------- | ---------- | ---------------- | ------- |

## Plotting

https://github.com/dl575/tsg_plot

