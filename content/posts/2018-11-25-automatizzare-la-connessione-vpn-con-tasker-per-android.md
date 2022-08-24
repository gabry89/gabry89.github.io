---
title: Automatizzare la connessione VPN con Tasker per Android
date: 2018-11-25T17:19:34.000Z
#description: Configurare Tasker per automatizzare la connessione VPN con OpenVPN per Android
draft: false
cover: 
    image: /images/pihole1-1-1-.jpg
slug: automatizzare-la-connessione-vpn-con-tasker-per-android
summary: Configurare Tasker per automatizzare la connessione VPN con OpenVPN per Android
---


Da quando ho iniziato ad usare le VPN sul telefono mi sono accorto di un problema di semplice natura: ricordarmi di attivarla quando esco di casa. Spesse volte mi è capitato infatti di ricordarmene solo dopo qualche ora passata fuori casa, o ancora peggio accorgermi della mancanza di un filtro sulla navigazione quando ormai avevo già vinto 4 iPad e 9 iPhone X.

L'idea mi è quindi balzata in testa: perché non automatizzare la connessione VPN appena mi allontano dal WiFi di casa? Viceversa, appena torno a casa la VPN si deve disconnettere.

Ebbene, grazie a Tasker e il client OpenVPN for Android è possibile farlo!

Questa breve guida fa seguito alla mia guida iniziale su [come realizzare una VPN domestica con Pi-Hole integrato](__GHOST_URL__/virtual-private-pi-holed-network/) da poter utilizzare fuori casa per navigare sicuri e protetti in mobilità.

## Tasker per Android

Tasker è un'app per Android che permette di eseguire delle automazioni in base a determinate condizioni. La trovate nel [Play Store](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm) ad un costo di 2.99 €.

Il principio di funzionamento di Tasker è molto semplice:

**SE** [CONDIZIONE DA RISPETTARE] **È VERO, ALLORA** [ESEGUI ATTIVITÀ]

In Tasker le "condizioni da rispettare" si chiamano **Profili**. Ogni **Attività** può contenere una o più **Azioni**.

## AUTOMATIC Virtual Private Pi-Holed Network

La logica con cui ho creato l'automazione è la seguente:

* **SE** [WiFi non connesso] **È VERO, ALLORA** [Connetti VPN]
* **SE** [WiFi di casa connesso] **È VERO, ALLORA** [Disconnetti VPN]

La realizzazione è abbastanza semplice, vediamola insieme.

## Creiamo le attività "Start VPN" e "Stop VPN"

Iniziamo con la creazione delle due attività di connessione e disconnessione della VPN.

### Start VPN

Apriamo Tasker e posizioniamoci sulla tab **Attività**, dopodiché premiamo **+**, scriviamo **"Start VPN"** e poi confermiamo.

A questo punto possiamo andare ad inserire una o più azioni che verranno eseguite quando verrà avviata questa attività, noi dobbiamo connettere il nostro profilo VPN salvato in OpenVPN for Android.

Per farlo, premiamo **+** e scegliamo **Sistema** e **Invia Intent**. Vi apparirà un form dove bisogna inserire dei valori. I valori da inserire sono questi:

Azione  
`android.intent.action.MAIN`  
Cat  
`None`  
Extra (dove "Casa" è il nome del vostro profilo VPN)  
`de.blinkt.openvpn.shortcutProfileName: Casa`  
Pacchetto  
`de.blinkt.openvpn`  
Classe  
`de.blinkt.openvpn.LauchVPN`  
Target  
`Activity`  

Ora torniamo indietro fino al pannello principale delle attività.

### Stop VPN

Creiamo la seconda attività per disconnettere la VPN sempre seguendo la stessa procedura con **+**, scriviamo **"Stop VPN"** e confermiamo.

Di nuovo qui facciamo **+**, **Sistema**, **Invia Intent** e scriviamo questi parametri:

Azione  
`android.intent.action.MAIN`  
Cat  
`None`  
Extra (dove "Casa" è il nome del vostro profilo VPN)  
`de.blinkt.openvpn.shortcutProfileName: Casa`  
Pacchetto  
`de.blinkt.openvpn`  
Classe  
`de.blinkt.openvpn.api.DisconnectVPN`  
Target  
`Activity`

Torniamo di nuovo indietro alla home e se avete fatto tutto giusto, dovreste ritrovarvi con due Attività: **"Start VPN"** e **"Stop VPN"**.

## Creazione dei profili "WIFI Collegato" e "Non WIFI Collegato"

Ora possiamo passare alla creazione dei profili.

### WIFI Collegato

Iniziamo con la creazione del profilo **"WIFI Collegato"**, dobbiamo quindi dirgli di disconnettere la VPN se siamo connessi al nostro WiFi di casa.

Spostiamoci nella tab **Profili**, premiamo **+**, selezioniamo **Stato** dal menu a scomparsa, poi **Rete** e **WIFI Collegato**.

Nel form che vi compare, dovete semplicemente inserire il **nome della vostra rete WiFi** nel campo **SSID** e lasciare tutto il resto invariato. Ora premendo indietro per tornare alla pagina principale vi comparirà automaticamente un menu rapido dal quale possiamo scegliere l'azione da eseguire quando si attiverà questo profilo. In questo specifico caso dovete selezionare **"Stop VPN"**.

Ed ecco quindi che abbiamo creato il primo profilo:

**SE** [WiFi di casa connesso] **È VERO, ALLORA** [Disconnetti VPN]

{{< figure src="/images/Screenshot_20181125-161151-2.png" >}}

### Non WIFI Collegato

Passiamo quindi alla creazione del profilo **"Non WIFI Collegato"**. Dobbiamo dire che se il WiFi viene scollegato, deve connettersi automaticamente la VPN.

Procediamo quindi allo stesso modo nella tab **Profili**, premiamo **+**, selezioniamo **Stato** dal menu a scomparsa, poi **Rete** e **WIFI Collegato**. A questo punto dobbiamo semplicemente mettere la spunta su **Inverti** lasciando tutto vuoto.

Torniamo indietro e ci verrà riproposto lo stesso menu rapido di prima, dove selezioniamo **"Start VPN"**.

Facendo così, abbiamo creato anche il nostro secondo profilo:

**SE** [WiFi non connesso] **È VERO, ALLORA** [Connetti VPN]

{{< figure src="/images/Screenshot_20181125-161157-2.png" >}}

## Salvataggio della configurazione

Ora che abbiamo creato le azioni e i profili, non resta che salvare tutta la configurazione. Per farlo è sufficiente premere il **check** sulla barra del titolo e Tasker si attiverà.

{{< figure src="/images/Screenshot_20181125-162303.png" >}}

## Autorizzare Tasker per l'interazione con OpenVPN

Alla prima esecuzione di un'attività potrebbe apparirvi un popup di richiesta autorizzazione. Questo è un avviso di sicurezza di **OpenVPN per Android** per indicarvi che un'applicazione di terze parti vuole interagire con i profili VPN. Qui dovete semplicemente **mettere la spunta** per considerare Tasker attendibile e fare **Ok**.

{{< figure src="/images/Screenshot_20181125-170252-2.png" >}}

## Considerazioni finali

Sto utilizzando questa configurazione da circa un mesetto e posso confermarvi che il setup è abbastanza solido. So per certo però che i più attenti di voi non potranno fare a meno di notare che c'è un piccolo "bug" nella mia configurazione, che si presenta nella situazione in cui si passa dalla rete WiFi di casa direttamente ad un'altra rete WiFi. In questo specifico caso infatti il profilo "Non WIFI Collegato" non entra in azione, poiché il dispositivo migra direttamente da una rete WiFi ad un'altra, non scatenando l'evento che fa switchare il profilo.

La situazione potrebbe sembrare di facile risoluzione semplicemente inserendo il nome dell'SSID di casa insieme al parametro Inverti (SE ![SSID di Casa]), tuttavia impostando così il profilo ho riscontrato che viene attivato erroneamente nel momento in cui il telefono si connette in VPN, generando così un loop di connessione e disconnessione continua.

Ammetto di non avere indagato a fondo, tuttavia il setup così impostato è perfetto per la mia esperienza d'uso, ovvero:

**Sono a casa ->** Profilo "WiFi Collegato", navigo in rete locale.

**Fuori casa in 4G ->** Mi sgancio dal wifi, si attiva "Non WiFi Collegato" e navigo in VPN.

**Mi collego ad altri WiFi ->** La condizione per "WiFi Collegato" non viene rispettata in quanto SSID differente, quindi continuo a navigare in VPN.

**Torno a casa ->** Rileva connessione al WiFi domestico, entra "WiFi Collegato" che disconnette la VPN.

E con questo è tutto. Se avete bisogno di ulteriori informazioni, segnalarmi degli errori o fornirmi feedback, scrivetemi! ☺️

