# README – eBPF TCP/UDP Counter Lab

This lab demonstrates an eBPF XDP program running on the **firewall** node, which counts the number of incoming TCP and UDP packets.
The eBPF program uses a map with two counters: **index 0 for TCP packets** and **index 1 for UDP packets**.

---

## Lab Configuration

* **Firewall Node**

  * Runs the eBPF XDP program.
  * Uses the Docker image **kathara/ebpf**.

* **pc0 Node**

  * Generates test traffic.
  * Includes two scripts in the *shared* folder:

    * `udp_sender.py` — sends UDP packets
    * `tcp_sender.py` — sends TCP packets

* **Wireshark (optional)**

  * Can be attached to monitor traffic in real time.

---

## Compiling and Attaching the eBPF Program

### 1. Compile the eBPF Program

Navigate to the home directory on the firewall node and compile the program using the Makefile.
This will generate an object file (e.g., `firewall.o`).

```bash
cd /home && make
```

---

### 2. Attach the eBPF Program

Attach the compiled program to the network interface (e.g., `eth0`):

```bash
ip link set dev eth0 xdp obj firewall.o sec xdp
```

Verify that the program is active:

```bash
bpftool prog show
bpftool net
```

---

## Monitoring the Packet Count Map

### 1. View Initial Map Values

Find the map ID from `bpftool prog show`, then dump the map:

```bash
bpftool map dump id <ID>
```

Replace `<ID>` with the actual map ID.
Initially, both counters should be **zero**.

---

### 2. Generate Traffic

From the **pc0** node, run the sender scripts located in the *shared* folder:

* UDP traffic:

  ```bash
  python3 udp_sender.py
  ```

* TCP traffic:

  ```bash
  python3 tcp_sender.py
  ```

---

### 3. View Updated Map Values

Dump the map contents again:

```bash
bpftool map dump id <ID>
```

The counters should now reflect the number of received **TCP** and **UDP** packets.

---

### 4. (Optional) Disable the eBPF Program

To detach the program:

```bash
ip link set dev eth0 xdp off
```

---

## Additional Information

For further details or troubleshooting, refer to the README in the parent directory.
It includes comprehensive explanations of eBPF commands and program usage and is recommended reading before starting the lab.

---

Happy testing and learning! 🚀

---
