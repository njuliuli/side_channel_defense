# side_channel_defense

An improve credit scheduler to defend against side channel attack

Refer to http://www.cs.gmu.edu/~sqchen/open-access/new-scheduler-report.pdf

## how to use

The directory "/xen" contains source code of xen-4.6.0, cloned by "git clone -b RELEASE-4.6.0 git://xenbits.xen.org/xen.git". 

The file /xen/xen/common/sched\_credit.c is modified to improve credit scheduler to defend against side channel attacks.

The different version of improved schedulers can be switched to by chaning the value of "choose" in this file, in the function "csched\_schedule". 
A summary of how each scheduler behave is described in line 2101-2113 of this file.

```C

    // Li
    // define an option to switch between different version of improved scheduler
    // difference mainly in vCPU picking up and returning process
    // A: which runq to pick next vCPU (local/random/global)
    // B: which order to pick next vCPU in given runq (ordered/random)
    // C: which runq to return current vCPU (local/global)
    // the combinations of different value for choose are:
    //  choose=1: A-random, B-order, C-local
    //        =2: A-local, B-random, C-local
    //        =3: A-random, B-random, C-local
    //        =4: A-global, B-order, C-global
    //        =5: A-global, B-random, C-global
    // LCS in paper using choose=3, GCS using choose=5
    int choose           = 5;

```

After set the value of "choose", xen can be build and install as normal.
A good guide to compile Xen from source: https://wiki.xenproject.org/wiki/Compiling_Xen_From_Source
