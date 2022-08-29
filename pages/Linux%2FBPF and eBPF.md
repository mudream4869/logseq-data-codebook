title:: Linux/BPF and eBPF

- ## 簡介
	- 於 1992 年，Steven McCanne and Van Jacobson 提出的在 kernel 使用虛擬機過濾封包的方式。
		- [https://www.tcpdump.org/papers/bpf-usenix93.pdf](https://www.tcpdump.org/papers/bpf-usenix93.pdf)
	- 在 2013 年，Alexei Starovoitov 推出了 extend BPF = eBPF，並且稱以前的 BPF 為 classic BPF = cBPF。
- ## BPF
	- 在 BPF 之前，假如要對封包進行過濾，需要從 kernel space **複製**所有封包到 user space，所以把過濾邏輯放在 kernel space 是一個很必要的處理。
	- 這論文主要說了兩件事：
		- 把 Filter 放到 kernel 確實可以更有效率
		- 把 Filter 從 Tree Representation 轉成 CFG Representation 可以更不錯
- ## eBPF
	- TODO eBPF
- ## Reference
	- [BPF - the forgotten bytecode](https://blog.cloudflare.com/bpf-the-forgotten-bytecode/)
	- [BPF 和 eBPF 初探](https://forsworns.github.io/zh/blogs/20210311/)