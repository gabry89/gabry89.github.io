---
title: 'VPPN: Virtual Private "Pi-Holed" Network'
date: 2018-11-02T16:34:08.000Z
#description: Come installare OpenVPN su Pi-Hole per poterlo usare con smartphone e tablet tramite VPN!
draft: false
cover: 
   image: /images/pihole1-1.jpg
slug: virtual-private-pi-holed-network
summary: Come installare OpenVPN su Pi-Hole per poterlo usare con smartphone e tablet tramite VPN!
---


Chi mi conosce sa molto bene che provo un amore incondizionato per [Pi-Hole](https://pi-hole.net/). Navigare in una rete non protetta da Pi-Hole mi fa sentire nudo e vulnerabile, perché Pi-Hole non è un semplice ad-blocker, è un incredibile strumento per alzare in modo considerevole la sicurezza della navigazione.

Senza gli ads, si riduce quasi del 100% la possibilità di venire dirottati su qualche pagina malevola a causa di malvertising.

Per esempio, attacchi di questo tipo verrebbero completamente neutralizzati grazie a Pi-Hole:

{{< tweet user="GabrielLandau" id="1055300918101598208" >}}

Fin dai primi giorni di utilizzo ho desiderato poter avere lo stesso livello di protezione anche in mobilità con lo smartphone. L'idea era quella di fare una VPN per dirottare tutto il traffico di navigazione nella rete di casa e far gestire quindi i DNS da Pi-Hole. Purtroppo questa idea è durata qualche secondo, giusto il tempo di ricordarsi di avere una misera aDSL da 8Mbps/700Kbps. #FuckDigitalDivideNon potendo fare una VPN domestica a causa delle scarse prestazioni della mia rete, ho adottato per diverso tempo una VPN di terze parti, nello specifico F-Secure Freedome, che implementa un filtro per siti malevoli e tracking. Non siamo ai livelli di Pi-Hole, però mi ha aiutato a navigare in modo sicuro anche in mobilità negli ultimi due anni.

Ora però la musica è cambiata. Ora ho cambiato casa e ho finalmente a disposizione una connessione stratosferica, 1Gbps/200Mbps con OpenFiber!

{{< figure src="/images/asdasdasdasda.gif" >}}

Appena ho collegato il router e ho provato con mano l'incredibile velocità, l'idea della VPN è tornata a solleticarmi, così una sera mi sono messo comodo sul divano e ho realizzato la mia **Virtual Private Pi-Holed Network**!

Nota: esiste una [guida ufficiale molto ben dettagliata](https://docs.pi-hole.net/guides/vpn/overview/), da cui ho preso informazioni a piene mani, tuttavia parte da una Raspbian pulita senza nulla, ci installa prima OpenVPN e poi Pi-Hole. E' molto più facile invece trovarsi in una situazione dove si ha un RPi con Pi-Hole già installato e configurato e si desidera installarci sopra la VPN.

Ed è esattamente quello che faremo ora.

Situazione di partenza: Raspberry Pi nella rete locale con Raspbian Lite e Pi-Hole installato e funzionante. Darò per scontato che se siete arrivati qui, sapete connettervi in SSH al vostro RPi. Consiglio prima di partire con l'installazione, di fare un controllo su eventuali aggiornamenti disponibili:

```
sudo apt update && sudo apt upgrade
```

### Installiamo OpenVPN su Pi-Hole

La prima cosa da fare è scaricare e installare OpenVPN [road warrior](https://en.wikipedia.org/wiki/Road_warrior_(computing)). Connettetevi in SSH al RPi ed eseguite questo comando:

```
wget https://git.io/vpn -O openvpn-install.sh
chmod 755 openvpn-install.sh
sudo ./openvpn-install.sh
```

Verrà eseguita la procedura guidata per l'installazione. E' molto semplice e basta inserire poche informazioni chiave. Vi consiglio di tenere le stesse impostazioni che ho messo io e cambiare solamente il vostro indirizzo pubblico e il nome del client alla fine:

```
Welcome to this quick OpenVPN "road warrior" installer

I need to ask you a few questions before starting the setup
You can leave the default options and just press enter if you are ok with them

First I need to know the IPv4 address of the network interface you want OpenVPN
listening to.
IP address: 10.8.0.1

This server is behind NAT. What is the public IPv4 address or hostname?
Public IP address / hostname: x.x.x.x 
#Nota: qui dovete inserire il vostro IP pubblico di casa

Which protocol do you want for OpenVPN connections?
   1) UDP (recommended)
   2) TCP
Protocol [1-2]: 1

What port do you want OpenVPN listening to?
Port: 1194

Which DNS do you want to use with the VPN?
   1) Current system resolvers
   2) Google
   3) OpenDNS
   4) NTT
   5) Hurricane Electric
   6) Verisign
DNS [1-6]: 1

Finally, tell me your name for the client certificate
Please, use one word only, no special characters
Client name: GabrielePixel2 
#Nota: qui dovete inserire un nome profilo per il vostro utente remoto

Okay, that was all I needed. We are ready to setup your OpenVPN server now
Press any key to continue...
```

A questo punto partirà l'installazione e terminerà così:

```
Finished!

Your client configuration is available at /root/GabrielePixel2.ovpn
If you want to add more clients, you simply need to run this script again!
```

Il file che ha generato è il profilo `.ovpn` da copiare nel vostro client per poter effettuare la connessione. Di default lo salva in `/root/`, vi consiglio di spostarlo nella vostra home per poterlo prelevare dal RPi con più semplicità:

```
cd ~
sudo mv /root/GabrielePixel2.ovpn GabrielePixel2.ovpn
```

A seconda della configurazione del vostro RPi, potrebbe essere necessario diventare proprietari del file per poterlo gestire. Se usate l'utente di default `pi` basta digitare:

```
sudo chown pi:pi GabrielePixel2.ovpn
```

Di questo file ci occuperemo più avanti. Procediamo con la configurazione del server VPN.

### Configuriamo OpenVPN Server

Ora dobbiamo cambiare i resolver DNS per l'interfaccia `tun0` creata dallo script di OpenVPN. Prima però ci serve sapere l'IP di questa interfaccia. Se non avete cambiato nulla in fase di installazione rispetto alla guida, dovrebbe avere indirizzo `10.8.0.1`, in alternativa potete sempre verificare con questi semplici comandi

Per Raspbian Jessie:

```
ifconfig tun0 | grep 'inet addr'
```

Per Raspbian Stretch:

```
ip a
```

Annotiamoci l'IP dell'interfaccia `tun0`, quindi andiamo a modificare il file di configurazione di OpenVPN:

```
sudo nano /etc/openvpn/server.conf
```

Cercate all'interno del file la riga che inizia con `push "dhcp option DNS x.x.x.x"` e modificatela con l'IP della vostra `tun0`:

```
push "dhcp-option DNS 10.8.0.1"
```

Chiudete salvando il file e riavviate il servizio di OpenVPN:

```
sudo service openvpn restart
```

Ora colleghiamoci al pannello admin web di Pi-Hole, nelle impostazioni, sezione DNS, impostate l'interfaccia di ascolto su "Listen on all interfaces" e salvate:

{{< figure src="/images/Screenshot_2018-11-02-Pi-hole-Admin-Console.png" caption="Listen on all interfaces" >}}

A questo punto il server OpenVPN è configurato e funzionante con Pi-Hole come resolver DNS! 👏

### Port Forward dal router

Per poter accedere da fuori è necessario esporre su internet la porta 1194 UDP del vostro RPi. Non vi lascio una guida per farlo perché cambia a seconda del router che avete, però vi consiglio di fare molta attenzione ad esporre SOLAMENTE la porta 1194 UDP e non tutto il RPi. **Non mettete il RPi in DMZ!!**

### Esportiamo il file .ovpn

Ora possiamo provare a connetterci alla nostra VPN, però prima dobbiamo recuperare il nostro file di configurazione `.ovpn` dal RPi e copiarlo sullo smartphone. Per farlo è sufficiente connettersi con un client SFTP e prelevare dalla home directory il file. Se siete su Windows vi consiglio [WinSCP](https://winscp.net/eng/docs/lang:it). Salvate pure il file `.ovpn` dove volete nel vostro smartphone, non è importante.

Se volete, potete creare ulteriori nuovi utenti semplicemente eseguendo di nuovo lo script iniziale

```
sudo ./openvpn-install.sh
```

Questa volta apparirà un menu differente, dove potremo creare un nuovo profilo `.ovpn` da usare per esempio su un altro device.

```
Looks like OpenVPN is already installed

What do you want to do?
   1) Add a new user
   2) Revoke an existing user
   3) Remove OpenVPN
   4) Exit
Select an option [1-4]: 1

Tell me a name for the client certificate
Please, use one word only, no special characters
Client name: iPhone7
```

La procedura per esportare il file .ovpn è la medesima riportata sopra.

### Connettiamoci alla VPN

Procediamo quindi all'installazione del client OpenVPN. Su Android vi consiglio OpenVPN for Android, poiché è un'alternativa OpenSource al client ufficiale e permette una personalizzazione più granulare.

Ora dovete solamente importare il profilo `.ovpn` nell'app e provare a connettervi.

Nota: potete cambiare liberamente il nome della connessione. Per esempio io l'ho chiamata `Casa`.

{{< figure src="/images/Screenshot_20181102-153132.png" >}}

Se tutto è stato fatto come da manuale, dovrebbe connettersi e dovreste riuscire a navigare.

{{< figure src="/images/Screenshot--2-nov-2018-17_30_49-.png" >}}

Come prova dell'effettiva navigazione con Pi-Hole, potete provare a visitare questo sito: [https://blockads.fivefilters.org/?pihole](https://blockads.fivefilters.org/?pihole)

{{< figure src="/images/Screenshot_2018-11-02-Block-Ads--1.png" >}}

Se vi restituisce questo messaggio, potete farvi i complimenti, avete appena creato la vostra **Virtual Private Pi-Holed Network**!