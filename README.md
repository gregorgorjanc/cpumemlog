# cpumemlog
Monitor CPU and RAM usage of a process (and its children)

## Description

**cpumemlog.sh** is a Bash shell script that monitors CPU and RAM usage of a given
process and its children. The main aim for writing this script was to get insight
about the behaviour of a process and to spot bottlenecks without GUI tools, e.g.,
cpumemlog.sh it is very useful to spot that the computationally intensive process
on a remote server died due to hitting RAM limit or something of that sort. The
statistics about CPU, RAM, and all that are gathered from the system utility
[ps](http://man7.org/linux/man-pages/man1/ps.1.html).

While the utility [top](http://www.unixtop.org) can be used for this interactively,
it is tedious to stare at its dynamic output and quite hard to spot consumption at
the peak and follow the trends etc. Yet another similar utility is [time](http://man7.org/linux/man-pages/man1/time.1.html), which though only gives
consumption of resources at the peak.

**cpumemlogplot.R** is a companion [R](http://www.r-project.org) script to cpumemlog.sh
used to summarize and plot gathered data.

## Usage

To monitor usage of a process, we first need to know its process ID (PID), which
can be found, for example, from the output of the [top](http://www.unixtop.org) or [ps](http://man7.org/linux/man-pages/man1/ps.1.html) utilities. Say, the process
has PID 1234, then we can monitor its CPU and RAM usage with (note the & character
that puts the script into background until the process stops running):

```shell
cpumemlog.sh 1234 &
```

and the gathered data are stored into a file called cpumemlog_1234.txt:

```shell
$ cat cpumemlog_1234.txt
DATE TIME PID PCPU PMEM RSS VSZ ETIME COMMAND
YYYY-MM-DD 21:21:13 3060 0.0 2.0 336348 4257284 7:19.48 processName
YYYY-MM-DD 21:21:13 3062 0.2 2.0 335364 3342724 11:30.61 processName
YYYY-MM-DD 21:21:23 3060 0.1 2.0 336344 4256760 7:19.48 processName
...
```

which can be in turn summarised using:

```shell
cpumemlogplot.R cpumemlog_1234.txt
```

Both scripts have some useful options, so make sure you check their
documentation in the source Luke!

## TODO (any volunteers?)

Compute cumulative usage for the monitored process and all its children.
