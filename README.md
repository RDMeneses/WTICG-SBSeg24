## Introduction

This repository is associated with the research project submitted to the "Workshop de Trabalhos de Iniciação Científica e Graduação" (WTICG) of the "XXIV Simpósio Brasileiro em Segurança da Informação e de Sistemas Computacionais (SBSeg 2024).

**Project Title:** Compact Memory Implementations of the ML-DSA Post-Quantum Digital Signature Algorithm

**Abstract:** This paper explores memory optimization techniques in the implementation of the Module-Lattice-Based Digital Signature Algorithm (ML-DSA) in the context of post-quantum cryptography. It shows how to achieve significant reductions in memory usage, and evaluates the trade-offs in computational speed. Moreover, it demonstrates how the secret (private) key can be managed to reduce significantly its storage requirements, thereby enhancing ML-DSA’s applicability in some resource-constrained environments.

Many of the scripts available in this repository are an adaptation of the official [CRYSTALS-Dilithium](https://github.com/pq-crystals/dilithium) implementation.

## Build Instructions

### Overview

This repository presents two implementations of the ML-DSA post-quantum digital signature algorithm:

1. ML-DSA-v: vector-by-vector generation
2. ML-DSA-p: polynomial-by-polynomial generation

To compile the test programs in Linux or macOS, go to `ML-DSA-v/` or `ML-DSA-p/` and run:

```
make clean all
make && make speed
```
Which will generate the executables:

```
test/test_dilithium$X
test/test_vectors$X
test/test_speed$X
PQCgenKAT_sign$X
```

Where `$X` ranges over the parameter (sets 2, 3, 5, 2aes, 3aes, 5aes), corresponding to each of the NIST's security levels. The first two tests are useful for evaluating the correctness of the algorithm, and `PQCgenKAT_sign$X` allows for tests with NIST's official test vectors. More information about these tools is available in the official [CRYSTALS-Dilithium](https://github.com/pq-crystals/dilithium) repository.

### Minimal test

The minimal test consists in a series of executions of the algorithm with key generation, signing and signature verification for a random message of 59 bytes. This can be done by running the command:

```
./test_dilithium$X
```

If the tests are sucessful, the program should output:

```
CRYPTO_PUBLICBYTES: size of the public key (pk)
CRYPTO_SECRETBYTES: size of the secret key (sk)
CRYPTO_BYTES: size of the signature (σ)
```

## Benchmarking [^1]

After compiling the test programs, it is possible to execute `test/test_speed$X` to obtain the median and the average of CPU cycles for each of ML-DSA's main functions (KeyGen, Sign and Verify), for each security level. The number of executions can be changed in the script `test_speed.c` by changing the parameter `NTESTS`.

For RAM usage, comment the counting of CPU cycles and the `print_results` command in `test_speed.c` and run:

```
valgrind --tool=massif --stacks=yes ./test_speed$x
```
Which will generate the file `massif.out.<PID>`. To visualize this file, use the `massif-visualizer` tool with the command:

```
massif-visualizer massif.out.<PID>
```
Will show the peak RAM values for the functions KeyGen, Sign and Verify during the program execution.

[^1]: Some of the benchmarking programs require [OpenSSL](https://openssl.org/) and [Valgrind](https://valgrind.org/).



