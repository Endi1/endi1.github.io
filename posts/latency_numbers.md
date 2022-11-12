# Useful latency numbers

1ns = 10^(-9)s

1μs = 10^(-6)s

1ms = 10^(-3)s


1ns - 10ns ---> L1/L2 Cache

10ns - 100ns ---> L3 cache

100ns - 1μs ---> [System call] -> [C library] -> Kernel or MD5 hashing 64 bit number

1μs - 10μs ---> Context switching between threads. Depends on the size of data

10μs - 100μs ---> process HTTP request or sequential of a 8KB file from SSD

100μs 1ms ---> SSD write latency or intra-zone networking round-trip or Memcache/Redis GET

1ms - 10ms ---> Intra-zone networking latency or seektime of HDD.

10ms - 100ms ---> Network round-trip for US-EU or read 1GB from main memory

100ms - 1s ---> bcrypt a password or TLS handshake or network roundtrip between US west coast and Singapore or 1GB sequential read from SSD.

1s+ ---> Transfer 1GB over the network within the same region.