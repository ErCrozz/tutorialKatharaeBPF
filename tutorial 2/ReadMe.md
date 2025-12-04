# eBPF Firewall Lab

In questo laboratorio, il firewall esegue un programma eBPF che **conta il numero di pacchetti in ingresso**.
Ad esempio, inviando un ping da *pc0*, il contatore nel programma eBPF verrà incrementato.

Segui i passaggi riportati di seguito per compilare, attivare e monitorare il comportamento del programma.

---

## Compiling and Attaching the eBPF Program

### 1. Compile the eBPF Program

Spostati nella home e compila il programma con il Makefile.
Puoi modificare il codice sorgente se necessario.

```bash
cd /home && make
```

Questo comando genera il file oggetto **firewall.o**.

---

### 2. Attach the eBPF Program

Collega il programma compilato all’interfaccia **eth0**:

```bash
ip link set dev eth0 xdp obj firewall.o sec xdp
```

Dopo l’aggancio, verifica che il programma sia attivo:

```bash
bpftool prog show
bpftool net
```

Questi comandi mostrano i programmi eBPF attivi e le relative mappe.

---

### 3. View the Packet Count Map

Individua l’ID della mappa tramite `bpftool prog show`, poi esegui:

```bash
bpftool map dump id <ID>
```

Sostituisci `<ID>` con l’ID reale della mappa per visualizzare il contatore dei pacchetti.

---

### 4. Ping the Firewall from pc0

Dal nodo **pc0**, genera traffico verso il firewall:

```bash
ping <firewall_IP>
```

---

### 5. View the Updated Packet Count Map

Esegui nuovamente:

```bash
bpftool map dump id <ID>
```

La mappa ora mostrerà il nuovo valore del contatore aggiornato.

---

### 6. Disable the eBPF Program (Optional)

Per disattivare il programma:

```bash
ip link set dev eth0 xdp off
```

---

## Additional Lab Information

Puoi anche collegare il container **Wireshark** all’interfaccia per analizzare il traffico generato e ottenere una visione più dettagliata del comportamento di rete.

Per ulteriori informazioni, consulta il README nella cartella superiore: contiene istruzioni approfondite sull’uso dei programmi eBPF e dei comandi correlati.

---

Buon lavoro e buon apprendimento! 🚀
