# Investigating various Haskell GC configs

https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/runtime_control.html#rts-options-to-control-the-garbage-collector

* -N                                  Use all CPU cores when available
* -A                                  Set the allocation area size used by the garbage collector (default 1m)
* -H2500M                             Allocate 2.5GB of RAM at startup and keep this minimum allocated
* -M15G                               Set the maximum heap size
* -F1.5                               Allocate 1.5x of live data size for the older generations (default 2)
* -I0                                 Disables the idle GC (default 0.3sec)
* -Iw600                              Ensure that an idle GC runs at most once every 10 minutes
* -c                                  Use a compacting algorithm for collecting the oldest generation
* -n                                  Divides the allocation area (-A value) into chunks of the specified size
* -qb                                 Disable load-balancing in the parallel GC in generation ⟨gen⟩ and higher (1 for -A < 32M, 0 otherwise)
* -qg                                 Disable parallel GC in generation ⟨gen⟩ and higher (default 0)
* -qn                                 Use a specific number of cores to participate in GC (default -N)
* --copying-gc                        Uses the generational copying garbage collector for all generations
* --nonmoving-gc                      Enable the concurrent mark-and-sweep garbage collector for old generation collectors
* --disable-delayed-os-memory-return  More accurate memory usage stats in memory usage reporting tools
* -s                                  Produce a detailed runtime statistics at the end of the program

## Relay Stats

```
docker inspect relay | jq -r .[].Config.Env
docker inspect relay | jq -r .[].RestartCount
```

### Relay -N -A32m -H4g -M7680m -F1.5 -O1g -Iw600 -qb --copying-gc -s

```
export CARDANO_RTS_OPTS="-N -A32m -H4g -M7680m -F1.5 -O1g -Iw600 -qb --copying-gc -s"
```

### Relay -N -A64m -H4g -M7680m -F1.5 -O1g -Iw600 -qb --copying-gc -s

```
export CARDANO_RTS_OPTS="-N -A64m -H4g -M7680m -F1.5 -O1g -Iw600 -qb --copying-gc -s"
```

## BProd Stats

```
docker inspect bprod | jq -r .[].Config.Env
docker inspect bprod | jq -r .[].RestartCount
```

### BProd -N -A128m -H4g -M15g -F1.5 -Iw300 -qb --copying-gc -s

```
export CARDANO_RTS_OPTS="-N -A128m -H4g -M15g -F1.5 -Iw300 -qb --copying-gc -s"
```
