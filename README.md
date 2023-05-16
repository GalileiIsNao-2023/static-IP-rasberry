# Static IP on Rasberry
Abbiamo deciso di impostare un IP fisso sul Rasberry in modo da essere [trovato](https://github.com/GalileiIsNao-2023/NAO-Rasberry_python-socket) più facilmente dal NAO sulla rete.

## Configurazione
L'IP viene assegnato dal sistema operativo di Rasberry tramite il file `dhcpd.conf`. Prima di andare a modificare questo file ci serviranno le seguenti informazioni:

- **tipo di connessione**:
    - **wlan0** → Rasberry è collegato al router via wireless
    - **eth0** → Rasberry è collegato al router tramite cavo

- **l'indirizzo IP attualmente assegnato a Rasberry** → con il comando `hostname -I`

- **l'indirizzo IP del gateway del router** → per trovarlo puoi utlizzare `ip r` o `grep default`

- **l'indirizzo IP DNS** → lo possiamo trovare all'interno di `resolv.conf` visualizzabile con il comando `sudo nano /etc/resolv.conf`. Annotare l'indirizzo di fianco alla voce nameserver premere quindi `Ctrl+X` per chiudere


## Impostazioni IP statico
Ora siamo pronti a modificare il file `dhcpd.conf`. Apriamolo con il comando `sudo nano /etc/dhcpcd.conf`.
Ora aggiungi in fondo al file le seguenti righe:
```
interface NETWORK
static ip_address=STATIC_IP/24
static routers=ROUTER_IP
static domain_name_servers=DNS_IP
```
Sostituisci:
- **NETWORK** → tipo di connessione wlan0 (wireless) o eth0(Ethernet)
- **STATIC_IP** → l'indirizzo IP statico che si desidera impostare per il Raspberry Pi (consiglio di lasciare lo stesso già assegnato in precedenza dal router per essere sicuri che sia libero)
- **ROUTER_IP** → l'indirizzo IP del gateway del router
- **DNS_IP** → l'indirizzo IP DNS
Esempio:
```
interface wlan0
static ip_address=192.168.1.120/24
static routers=192.168.1.254
static domain_name_servers=192.168.1.254
```

## Riavvio
A questo punto possiamo riavviare il Rasberry:
```
sudo reboot
```

Per verificare l'indirizzo IP attuale digitare:
```
hostname -I
```
