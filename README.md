# pscache-data

This Github site contains the artifact and all experimental results of the submission titled "Partial Solution Based Constraint Solving Cache in Symbolic Execution" in FSE 2024. We provide this repository to assist the reviewers in evaluating our work.

The experimental results are provided in the form of tar packages, which are also included in the artifact along with data analysis scripts. The artifact is a Docker image that contains executables, benchmarks, raw data and running scripts of our submission. The reviewers can follow the steps below to install the docker image and try our work.

## Install docker-engine
Before attempting to reproduce the experimental results, please ensure that you have installed the Docker. If you have not done so, please refer to [this site](https://docs.docker.com/get-docker/) for assistance. Our Docker image has been tested on a **Linux** host operating system.

## Load the Docker image
First, you should pull the Docker image from hub:
```bash
$ docker pull pscache/pscache-fse:v2
```
If the image is loaded successfully, you can use the following command to verify it. You should see that an image named `pscache/pscache-fse` exists.
```bash
$ docker images
REPOSITORY            TAG    IMAGE ID      CREATED          SIZE
pscache/pscache-fse   v2     <image_id>    1 minutes ago    4.99GB
```
Then, you can start to run the container in interactive mode.
```bash
$ docker run -it pscache/pscache-fse:v2 /bin/bash
```
Now, you are inside the Docker image. We provide some useful commands here. You can find more useful [tutorials](https://docs.docker.com/) in the official site.

**Notes:**

when inside the container,

(1) to exit the docker container and terminate all running jobs inside the container, use:
```bash
$ exit
```
(2) to turn interactive mode into daemon mode (detach from the running container without stopping it),
```bash
press CTRL+P followed by CTRL+Q
```

when outside the container and looking to attach to it, use:
```bash
$ sudo docker attach <container_id>
```

## Experimental environment
Once inside the container, navigate to the /cache directory and list its contents. Then, the experimental environments of three experiments in our submission are shown.
```bash
$ cd /cache && ls
experiment1  experiment2  experiment3  klee-uclibc
```
Next, we will guide the reviewers to view the detailed results of our experiments, which are computed by the data analysis script inside each environment.

Take the first experiment as an example, navigate to the directory containing the compression package of its raw data and decompress the package.
```bash
$ cd ./experiment1/raw_data && tar xf exp1-results.tar.gz && ls
exp1-results exp1-results.tar.gz
```
The data analysis script `analyze.py` is located inside `exp1-results`, and each directory corresponds to the raw data of one combination of parameters Kp and Ks. For example, `250_50` indicates that the value of Kp is 250 and that of Ks is 50.
```bash
$ cd exp1-results && ls
150_100  150_150  150_50  300_100  300_150  300_50  analyze.py
```
The script incorporates many options. We recommend that the reviewers focus on the `-t` option, which displays a visually appealing ASCII table (`PrettyTable`) of the detailed results. Other options generate the latex code of figures and tables in the submission. For example, if you are interested in the results of `250_50`, you can invoke the script in the following way. The detailed results (average value of five runs) under three search strategies will be displayed.
```bash
$ ./analyze.py -t 250_50/*
+----------------------------------------------------------------------------------------------------------------------------------------------------+
|                                                                Statistics under dfs                                                                |
+--------------------+--------+---------+----------+------------------------+------------------+---------------------------+-------------------------+
|      Program       |  Time  |  Paths  | Queries  |      Unsat / Sat       | Hits (Hit Rate)  |           psTime          | Assignments / PsEntries |
+--------------------+--------+---------+----------+------------------------+------------------+---------------------------+-------------------------+
|      apr | ps      | 1808.4 |  107.6  |  1535.8  |  175.6 / 60.2 (0.74)   |  1300.0(0.8465)  |    1.24(0.71+0.22+0.31)   |    307.8 / 69.8(4.41)   |
|   apr | vanilla    | 1813.0 |   80.6  |  1161.4  |  165.6 / 74.4 (0.69)   |  920.8(0.7928)   |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|     cmark | ps     | 1800.6 |  1790.4 | 11894.8  | 3183.6 / 2946.8 (0.52) |  5764.4(0.4846)  | 213.49(58.92+153.72+0.85) |  7218.2 / 1625.2(4.44)  |
|  cmark | vanilla   | 1801.0 |  1931.6 | 13243.6  | 3643.6 / 3464.0 (0.51) |  6136.0(0.4633)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    fribidi | ps    | 1810.2 |  336.8  | 13414.4  |  936.8 / 69.6 (0.93)   | 12398.0(0.9242)  |    9.75(3.51+4.95+1.29)   |  10570.8 / 547.4(19.31) |
| fribidi | vanilla  | 1813.4 |  273.8  | 11357.0  |  865.2 / 163.0 (0.84)  | 10320.4(0.9087)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|      gas | ps      | 1815.8 |  312.0  | 13966.0  | 1278.8 / 264.0 (0.83)  | 12399.2(0.8878)  |    6.03(2.3+3.47+0.26)    |  11582.2 / 464.0(24.96) |
|   gas | vanilla    | 1830.4 |  305.0  | 13792.0  | 1244.0 / 439.0 (0.74)  | 12088.0(0.8765)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    json-c | ps     | 1815.6 |  409.8  | 13677.6  | 5594.6 / 358.0 (0.94)  |  7725.0(0.5648)  |  65.39(21.84+40.94+2.61)  |  16791.8 / 801.4(20.95) |
|  json-c | vanilla  | 1818.0 |  397.8  |  8935.4  | 3342.2 / 2465.2 (0.58) |  3128.0(0.3501)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|     sqli | ps      | 1803.8 |  1007.6 | 16776.4  |  953.4 / 118.0 (0.89)  | 15705.0(0.9361)  |    1.87(1.04+0.67+0.16)   |   1894.2 / 266.0(7.12)  |
|   sqli | vanilla   | 1805.4 |  1194.8 | 19722.4  |  903.0 / 163.4 (0.85)  | 18656.0(0.9459)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    libtom | ps     | 1809.0 | 38006.6 | 48781.0  | 2038.2 / 1244.6 (0.62) | 45498.2(0.9327)  |   24.62(3.79+20.58+0.25)  |   5352.8 / 928.8(5.76)  |
|  libtom | vanilla  | 1811.4 | 36151.8 | 46614.6  | 2002.4 / 1968.4 (0.5)  | 42643.8(0.9148)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|      m4 | ps       | 1800.6 | 14938.4 | 134711.2 | 3672.8 / 1098.4 (0.77) | 129940.0(0.9646) | 124.29(66.59+39.45+18.25) |   9244.4 / 960.8(9.62)  |
|    m4 | vanilla    | 1800.8 | 20304.8 | 183724.2 | 3770.0 / 1935.6 (0.66) | 178018.6(0.9689) |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|   discount | ps    | 1801.0 | 86474.6 | 502528.2 | 9557.0 / 3688.2 (0.72) | 489283.0(0.9736) |  25.74(10.59+13.84+1.31)  | 22726.4 / 1795.2(12.66) |
| discount | vanilla | 1800.8 | 69764.0 | 423620.4 | 7767.2 / 4029.4 (0.66) | 411823.8(0.9722) |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|      pac | ps      | 1802.8 |   36.8  |  1852.2  |  604.8 / 17.4 (0.97)   |  1230.0(0.6641)  |   12.02(0.95+10.95+0.12)  |   1261.0 / 143.0(8.82)  |
|   pac | vanilla    | 1803.8 |   35.8  |  1621.6  |  535.6 / 46.8 (0.92)   |  1039.2(0.6408)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|      ptx | ps      | 1800.8 |  3312.6 | 266333.2 |  3930.6 / 72.8 (0.98)  | 262329.8(0.985)  |   10.67(2.86+5.44+2.37)   | 51103.0 / 2844.4(17.97) |
|   ptx | vanilla    | 1800.8 |  2579.0 | 191602.2 | 2986.4 / 518.4 (0.85)  | 188097.4(0.9817) |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    sha1-cd | ps    | 1815.8 |   80.6  |  591.6   |  145.4 / 43.2 (0.77)   |   352.6(0.596)   |  38.26(24.97+12.53+0.76)  |  1296.6 / 101.2(12.81)  |
| sha1-cd | vanilla  | 1808.8 |   73.2  |  516.0   |  127.6 / 78.2 (0.62)   |  268.6(0.5205)   |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|     smaz | ps      | 1818.0 |   52.4  |  1244.8  |  338.4 / 131.8 (0.72)  |  774.6(0.6223)   |   34.65(23.02+3.95+7.68)  |  3142.8 / 186.2(16.88)  |
|   smaz | vanilla   | 1806.8 |   41.6  |  1194.4  |  320.4 / 156.2 (0.67)  |  717.6(0.6008)   |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    sqlite3 | ps    | 1814.0 |   42.0  |  6192.0  |  877.6 / 48.4 (0.95)   |  5265.2(0.8503)  |    8.68(2.55+4.94+1.19)   |   3042.4 / 311.8(9.76)  |
| sqlite3 | vanilla  | 1811.8 |   47.4  |  6819.6  |  856.4 / 244.6 (0.78)  |  5718.0(0.8385)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
|    sundown | ps    | 1800.0 |  1524.8 | 43463.4  | 8098.2 / 957.4 (0.89)  | 34407.8(0.7916)  |  74.05(50.36+7.64+16.05)  |  22554.2 / 6904.4(3.27) |
| sundown | vanilla  | 1802.6 |  1430.8 | 40738.4  | 7449.0 / 3823.0 (0.66) | 29466.4(0.7233)  |     0.00(0.0+0.0+0.0)     |       0.0 / 0.0(0)      |
+--------------------+--------+---------+----------+------------------------+------------------+---------------------------+-------------------------+
...
+--------------------------------------------------+
|               Statistics under bfs               |
+--------------------------------------------------+
...
+--------------------------------------------------+
|               Statistics under rcn               |
+--------------------------------------------------+
...
```
The detailed results of the other two experiments also can be shown through `-t` option.
```bash
  # Experiment 2 
$ cd /cache/experiment2/raw_data && tar xf exp2-results.tar.gz && cd exp2-results
$ ./analyze.py -t ps_2500_2500/* vanilla/*
  # Experiment 3
$ cd /cache/experiment3/raw_data && tar xf exp3-results.tar.gz && cd exp3-results
$ ./analyze.py -t 100_100/*
```

## Reproduce the experimental results

We can use this docker image to repoduce the experimental results presented in the submission. Our experiments were performed on a server with **Intel(R) Xeon(R) Platinum 8269CY CPU @ 2.50GHz** and the operating system is **Ubuntu 20.04**. To reproduce the results, a machine with similar **CPUs(~2.50GHz)** is required. Running the artifact on a different machine could possibly diverge the execution and lead to different results.

### Experiment 1
Navigate to `/cache/experiment1/scripts` and invoke the running script.
```bash
$ cd /cache/experiment1/scripts
$ ./run.py # wait...
```
The `run.py` script will automatically detect the number of cores in the current working machine, set the number of programs in parallel to a half, and perform the experiment on each benchmark program for 1,800 seconds (30 min).

### Experiment 2
Similarly, navigate to `/cache/experiment2/scripts` and invoke the running script.
```bash
$ cd /cache/experiment2/scripts
$ ./run.py # wait...
```

### Experiment 3

Navigate to `/cache/experiment3/jpf-cache/jpf-symbc` and invoke `run-exp.sh`, which is a shell script.
```bash
$ cd /cache/experiment3/jpf-cache/jpf-symbc
$ ./run-exp.sh exp-1 8 # wait...
```
The first argument of `run-exp.sh` is the output directory, and the second argument is the number of parallel tasks. In the above his case, we are analyzing 8 benchmark programs simultaneously.
