# eBPF Firewall Lab

In this lab, the firewall runs an eBPF program that **counts the number of incoming packets**.
For example, when sending a ping from *pc0*, the counter inside the eBPF program will increase.

Follow the steps below to compile, attach, and monitor the program’s behavior.

---

## Compiling and Attaching the eBPF Program

### 1. Compile the eBPF Program

Navigate to the home directory and compile the program using the Makefile.
You may modify the source code if needed.

```bash
cd /home && make
```

This command generates the **firewall.o** object file.

---

### 2. Attach the eBPF Program

Attach the compiled program to the **eth0** interface:

```bash
ip link set dev eth0 xdp obj firewall.o sec xdp
```

After attaching, verify that the program is active:

```bash
bpftool prog show
bpftool net
```

These commands display the active eBPF programs and their associated maps.

---

### 3. View the Packet Count Map

Identify the map ID using `bpftool prog show`, then run:

```bash
bpftool map dump id <ID>
```

Replace `<ID>` with the actual map ID to view the packet counter.

---

### 4. Ping the Firewall from pc0

From the **pc0** node, generate traffic toward the firewall:

```bash
ping <firewall_IP>
```

---

### 5. View the Updated Packet Count Map

Run again:

```bash
bpftool map dump id <ID>
```

You will now see the updated counter value.

---

### 6. Disable the eBPF Program (Optional)

To detach the program:

```bash
ip link set dev eth0 xdp off
```

---

## Additional Lab Information

You can also attach the **Wireshark** container to the interface to analyze the generated traffic and obtain deeper insights into network behavior.

For more details, refer to the README in the parent directory, which provides comprehensive instructions on using eBPF programs and related commands.

---

Happy testing and learning! 🚀
