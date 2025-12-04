# Kathara Lab – eBPF Usage

This lab uses a single node named **firewall**, as defined in the `lab.conf` file:

```conf
# Firewall eBPF
firewall[0]="A"
firewall[image]="kathara/ebpf"
firewall[ipv6]="false"
```

The **kathara/ebpf** Docker image includes all required dependencies to compile and run eBPF programs.

The provided eBPF program is designed to **discard all incoming packets** once attached.

---

## 1. Check Active eBPF Programs

Before compiling or attaching any new eBPF program, check what is already loaded:

```bash
bpftool prog show
bpftool net
```

These commands display the currently active eBPF programs and network hooks.

---

## 2. Compile the eBPF Program

Move to the home directory and use the Makefile to compile the program (you may modify the source code if needed):

```bash
cd /home && make
```

This command generates the `firewall.o` object file, ready to be attached to the network interface.

---

## 3. Attach the eBPF Program

To attach the program to interface **eth0**:

```bash
ip link set dev eth0 xdp obj firewall.o sec xdp
```

After attaching, verify that the program is active:

```bash
bpftool prog show
bpftool net
```

---

## 4. Disable the eBPF Program

To remove the program from `eth0`:

```bash
ip link set dev eth0 xdp off
```

---

## Additional Notes

For more details and additional commands, refer to the README in the parent directory.
It contains comprehensive explanations about using eBPF programs and should be read first.

---

Happy testing and learning! 🚀
