# Timing Process in C

## Process Time - CPU time used by a process since it was created. 
Can be seen as a sum of :
1. User Time - time spent executing in user mode, i.e. when it appears to the program that it has access to CPU  
2. System Time - amount of time spent executing in kernel mode, i.e. time spent by kernel executing sys calls etc on behalf of the program.


## 4 ways to measure:
1. times()
2. clock()
3. clock_gettime()
4. __rdtsc() intrinsic

## times()

```
#include <sys/times.h>

clock_t times(struct tms* buf)
```
ignore the return value unless it's -1 which means an error. It will fill in the passed in tms buffer ```buf``` which has the following fields

```
struct tms {
    clock_t tms_utime;      /* User CPU time used by caller */
    clock_t tms_stime;      /* System CPU time used by caller */
    clock_t tms_cutime;     /* User CPU time of all (waited for) children */
    clock_t tms_cstime;     /* System CPU time of all (waited for) children */
}
```
clock_t data type in an integer that measures time in units called *clock ticks*. Call sysconf(_SC_CLK_TCK) to get the number of clock ticks per second
Process time taken (in seconds) = ```(tms_utime + tms_stime) / sysconf(_SC_CLK_TCK) ```

## sysconf
Allows an application to obtain values of system limits at run time
```
# include <unistd.h>

long sysconf(int name)
```
Returns value of limit specified by name or -1 in indeterminate or error.


## clock()

```
#include <time.h>

clock_t clock(void);
```
Returns total CPU time used by calling process measured in CLOCKS_PER_SEC or -1 on error.
Value of clock_t returned by clock() is measured in ```CLOCKS_PER_SEC``` so we must divide ```clock_t/CLOCKS_PER_SEC``` to get time in seconds.
CLOCKS_PER_SEC is a macro defined in time.h, value is usually 1 million. 

## clock_gettime()
```
#include <time.h>

int clock_gettime(clockid_t clockid, struct timespec *tp)
int clock_getres(clockid_t clockid, struct timespec * res)
```
Both return ```0``` on success ```-1``` on error and fill in the passed in timespec struct

```c
time_t tv_sec;      /* Seconds */
long tv_nsec;       /* Nanoseconds */
```
```time_t``` unspecified what it's a typedef to but typically a 32- or 64-bit integer
```tv_nsec``` specifies a nanosecond value between 0 - 999,999,999

Altough the timespec structure affords nanosec precision the value returned by clock_gettime() maybe coarser.

```clock_getres()``` returns in res a timespec struct containing the resolution of the clock specified in the clockid parameter.

``` clockid_t ``` type represents a clock identifier
Values for ```clockid_t```
1. CLOCK_REALTIME - Settable systemwide real-time clock
2. CLOCK MONOTONIC - Nonsettable monotonic clock
3. CLOCK_PROCESS_CPUTIME_ID - Per process CPU-time clock >= Linux 2.6.12
4. CLOCK_THREAD_CPUTIME_ID - Per thread CPU-time clock >= Linux 2.16.12
5. CLOCK_MONOTONIC_RAW - nonsettable clock similar to CLOCK_MONOTONIC  but gives access to pure hardware time unaffected by NTP > Linux 2.6.28
6. CLOCK_REALTIME_COARSE  similar to CLOCK_REALTIME - but obtain minimal cost lower resolution timestamps
7. CLOCK_MONOTONIC_COARSE - similar to CLOCK_MONOTONIC - but at minimal cost but lower resolution

## INTEL __rdtsc() intrinsic
On linux
```
include <x86intrin.h>

int64_t start = __rdtsc();
// WORK HERE
int64_t stop = __rdtsc();
int64_t elapsed = stop - start;
```
Returns the current value of the processor's 64-bit time stamp counter. Not implemented on Itanium based processors or AMD(?)

### Not safe or portable due to different meanings of tie TSC(time stamp counter)

#### References
1. TLPI
2. Intel C++ Compiler for Linux Intrinsics Reference
3. https://stackoverflow.com/questions/12392278/measure-time-in-linux-time-vs-clock-vs-getrusage-vs-clock-gettime-vs-gettimeof?rq=1
4. https://man7.org/linux/man-pages/man7/time.7.html
5. https://man7.org/linux/man-pages/man7/vdso.7.html