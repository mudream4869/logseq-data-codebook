title:: Linux/BPF and eBPF

- ## Introduction
	- In 1992, Steven McCanne and Van Jacobson propose filtering packet in kernel model using virtual machine.
	- In 2013, Alexei Starovoitov propose extend BPF (=eBPF), and named BPF as classic BPF (=cBPF)
- ## BPF
	- Before BPF, to filter packets, the process should **copy whole data** from kernel space to user space. Hence putting filtering process into kernel space is necessary.
	- https://www.tcpdump.org/papers/bpf-usenix93.pdf
		- Putting filter process into kernel mode is more efficient.
		- Transfer filter from Tree Representation to CFG Representation is more efficient.
- ## eBPF
	- TODO eBPF
- ## Reference
	- [BPF - the forgotten bytecode](https://blog.cloudflare.com/bpf-the-forgotten-bytecode/)
	- [BPF 和 eBPF 初探](https://forsworns.github.io/zh/blogs/20210311/)