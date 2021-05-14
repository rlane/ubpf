# uBPF

Userspace eBPF VM

[![Build Status](https://travis-ci.org/iovisor/ubpf.svg?branch=master)](https://travis-ci.org/iovisor/ubpf)
[![Coverage Status](https://coveralls.io/repos/iovisor/ubpf/badge.svg?branch=master&service=github)](https://coveralls.io/github/iovisor/ubpf?branch=master)

## About

This project aims to create an Apache-licensed library for executing eBPF programs. The primary implementation of eBPF lives in the Linux kernel, but due to its GPL license it can't be used in many projects.

[Linux documentation for the eBPF instruction set](https://www.kernel.org/doc/Documentation/networking/filter.txt)

[Instruction set reference](https://github.com/iovisor/bpf-docs/blob/master/eBPF.md)

This project includes an eBPF assembler, disassembler, interpreter,
and JIT compiler for x86-64.

## Building

Run `make -C vm` to build the VM. This produces a static library `libubpf.a`
and a simple executable used by the testsuite. After building the
library you can install using `make -C vm install` via either root or
sudo.

## Running the tests
To run the tests, you first need to build the vm code then use nosetests to execute the tests. Note: The tests have some dependencies that need to be present. See the [.travis.yml](https://github.com/iovisor/ubpf/blob/master/.travis.yml) for details.

### Before running the test (assuming Debian derived distro)
```
sudo apt-get update
sudo apt-get -y install python3-pip
python3 -m pip install -r requirements.txt
python3 -m pip install cpp-coveralls
```

### Running the test
```
make -C vm COVERAGE=1
nosetests -v   # run tests
```

### After running the test
```
coveralls --gcov-options '\-lp' -i $PWD/vm/ubpf_vm.c -i $PWD/vm/ubpf_jit_x86_64.c -i $PWD/vm/ubpf_loader.c
```

## Compiling C to eBPF

You'll need [Clang 3.7](http://llvm.org/releases/download.html#3.7.0).

    clang-3.7 -O2 -target bpf -c prog.c -o prog.o

You can then pass the contents of `prog.o` to `ubpf_load_elf`, or to the stdin of
the `vm/test` binary.

## Contributing

Please fork the project on GitHub and open a pull request. You can run all the
tests with `nosetests`.

## License

Copyright 2015, Big Switch Networks, Inc. Licensed under the Apache License, Version 2.0
<LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0>.
