# Directory Layout Convention in Modules
> **Assume foobar/ provided as a module, then:**

## foobar/*.mk
* Build recipe

## foobar/foobar.h       
* Export header, may include sub-headers or not
* Include:
    - foobar_*() API declarations
    - unittest_foobar_*() declarations for API test
    - benchmark_foobar_*() declarations for API benchmark

## foobar/foobar_config.h
* Internal picking up sub-modules, won't export, optional

## foobar/foobar_internal.h
* Include:
    - foobar.h, so sub-modules refs *_internal.h instead 
    - foobar_config.h, so picking up or not implied also
    - internal data-structs and internal declarations

## foobar/foobar.c
* Encapsulate sub-modules to foobar_*() symbols, optional

## foobar/xxx.c foobar/yyy.c foobar/zzz.c          
* They all "#include foobar_internal.h"
* Sub-modules as fundament/provider of foobar_*() symbols 

## foobar/foobar_testsuites.c
* Provides unittest_foobar_*() definitions for API test

## foobar/foobar_benchmarks.c
* Provides benchmarks_foobar_*() definitions for API test
