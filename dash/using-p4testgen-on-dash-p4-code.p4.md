# Introduction

How to run p4testgen on DASH P4 reference code.

## Using locally-built p4c:

First, build the `p4testgen` binary from source code.  One way to do
so on a freshly installed Ubuntu 20.04 or 22.04 system is to run the
`install-p4dev-v6.sh` script described here:

+ https://github.com/jafingerhut/p4-guide/blob/master/bin/README-install-troubleshooting.md

Then follow these steps:

```bash
git clone https://github.com/sonic-net/DASH
cd DASH/dash-pipeline/bmv2
p4testgen -DTARGET_BMV2_V1MODEL --target bmv2 --arch v1model --max-tests 10 --out-dir out-p4testgen --test-backend ptf dash_pipeline.p4
```
That will write a file named `out-p4testgen/dash_pipeline.py`
containing a Python test using the `ptf` package.

If you want to run `p4testgen` for the DPDK PNA variant of the DASH
pipeline code, use this command instead.  I do not know why, but with
these command line options it is not supported to generated a test in
PTF form.

```bash
p4testgen -DTARGET_DPDK_PNA --target dpdk --arch pna --max-tests 10 --out-dir out-p4testgen --test-backend metadata dash_pipeline.p4
```

## Using Pre-built Docker image
An alternative to building `p4c` locally is to use pre-built Docker images available on Dockerhub under the `p4lang` registry [here](https://hub.docker.com/u/p4lang). Below are the commands to build for bmv2 or p4dpdk, respectively. (Use the aforementioned instructions for cloning the DASH project).

First, ensure you have [Docker installed on your system](https://docs.docker.com/desktop/)

```
docker run -it --rm -v $PWD:/proj p4lang/p4c:latest p4testgen -DTARGET_BMV2_V1MODEL --target bmv2 --arch v1model --max-tests 10 --out-dir /proj/out-p4testgen --test-backend ptf /proj/dash_pipeline.p4
```
or
```
docker run -it --rm -v $PWD:/proj p4lang/p4c:latest p4testgen -DTARGET_DPDK_PNA --target dpdk --arch pna --max-tests 10 --out-dir /proj/out-p4testgen --test-backend metadata /proj/dash_pipeline.p4
```
