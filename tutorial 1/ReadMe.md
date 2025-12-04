---

# Kathara Lab – eBPF Usage

Questo laboratorio utilizza un singolo nodo chiamato **firewall**, come definito nel file `lab.conf`:

```conf
# Firewall eBPF
firewall[0]="A"
firewall[image]="kathara/ebpf"
firewall[ipv6]="false"
```

L’immagine Docker **kathara/ebpf** contiene tutte le dipendenze necessarie per compilare ed eseguire programmi eBPF.

Il programma eBPF fornito è progettato per **scartare tutti i pacchetti in ingresso** una volta attivato.

---

## 1. Verificare i programmi eBPF attivi

Prima di compilare o collegare un nuovo programma, controlla cosa è già caricato:

```bash
bpftool prog show
bpftool net
```

Questi comandi mostrano i programmi eBPF attivi e gli hook di rete utilizzati.

---

## 2. Compilare il programma eBPF

Spostati nella home e usa il Makefile per compilare (puoi modificare il programma se necessario):

```bash
cd /home && make
```

Questo comando genera il file oggetto `firewall.o`, pronto per essere collegato all’interfaccia di rete.

---

## 3. Attivare il programma eBPF

Per collegare il programma all’interfaccia **eth0**:

```bash
ip link set dev eth0 xdp obj firewall.o sec xdp
```

Dopo l’operazione, verifica di nuovo i programmi attivi:

```bash
bpftool prog show
bpftool net
```

---

## 4. Disattivare il programma eBPF

Per rimuovere il programma da `eth0`:

```bash
ip link set dev eth0 xdp off
```

---

## Note aggiuntive

Per ulteriori dettagli e comandi aggiuntivi, consulta il README nella cartella superiore.
Contiene spiegazioni complete riguardo all’uso dei programmi eBPF ed è consigliato leggerlo per primo.

---

Buon testing e buon apprendimento! 🚀
