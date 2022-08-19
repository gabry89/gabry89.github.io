+++
title = "Fastweb FTTH e Modem Libero"
date = 2019-04-03T21:02:50Z
description = "Una breve guida per utilizzare il modem libero proprietario con Fastweb FTTH e togliere il FASTGate."
draft = false
cover = "/images/thomas-jensen-602189-unsplash.webp"
slug = "fastweb-e-modem-libero"
summary = "Una breve guida per utilizzare il modem libero proprietario con Fastweb FTTH e togliere il FASTGate."
+++


**AGGIORNAMENTO del 01/03/2020**

Questo articolo è stato scritto diversi mesi fa. Potrebbe non essere più attuale e potrebbe consigliare procedure non più attuabili con i nuovi router e le nuove attivazioni Fastweb.

**ARTICOLO ORIGINALE**

Con l’entrata in vigore della delibera AGCOM n. 348/18/CONS sul modem libero, gli operatori di rete fissa sono stati obbligati a rilasciare tutti i parametri relativi alla corretta configurazione di un modem alternativo a quello fornito dall'operatore.

Purtroppo la realtà dei fatti è che non tutti gli operatori hanno risposto in modo efficiente. Tra questi c'è Fastweb che ha interpretato "a modo suo" la delibera, soprattutto in ambito FTTH.<!--more-->

Se andiamo a leggere nella [pagina dedicata alla delibera](https://www.fastweb.it/adsl-fibra-ottica/dettagli/altri-modem/) sul sito di Fastweb, si legge:

> Nel caso di **Fibra**, l'apparato scelto dal cliente alternativo al FASTGate deve essere collegato alla terminazione ottica (ONT) fornita da Fastweb mediante un cavo di rete Ethernet Categoria 5e o superiore alla porta WAN Ethernet.

Insomma, non proprio chiarissimo. Ma soprattutto, se oggi prendete il vostro bel cavo in fibra ottica, prendete il vostro modem router e lo collegate al posto del FASTGate fornito da Fastweb, usando solo le informazioni riportate sul sito, il risultato è che non succede proprio nulla.

**Fastweb ha mentito? Non direi**. Diciamo che ha omesso tipo quelle due o tre informazioni utili a far funzionare il tutto. Nel momento in cui vi scrivo infatti è possibile usare un router alternativo al FASTGate, a patto di spendere qualche soldino e rispettare un po' di requisiti tecnici.

## Requisiti

Partiamo da quel poco che ci dice Fastweb, ovvero che _"l'apparato [...] deve essere collegato alla terminazione ottica (ONT) [...] con un cavo di rete [...] alla porta WAN"_.

L'ONT fornita da Fastweb è quella piccola terminazione di metallo che collega il cavo in fibra ottica direttamente al router.

{{< figure src="/images/IMG_20190403_224147.resized.jpg" >}}

Questo piccolo stick è un trasmettitore che invia e riceve segnali luminosi sulla fibra ottica e li trasforma in impulsi elettrici, interpretabili dal router. L'ONT va collegato alla porta WAN del router con un cavo di rete, quindi... serve un **convertitore**!

### TP-Link MediaConverter MC220L

L'MC220L di TP-Link è attualmente il miglior MediaConverter che potete acquistare per questo caso d'uso. Il costo è inferiore ai 30 euro.

{{< figure src="/images/image-1.png" caption="TP-Link MediaConverter MC220L" position="center" >}}

È un dispositivo abbastanza stupido nel suo funzionamento. Ha due porte: in una ci mettete la ONT di Fastweb, nell'altra ci mettete il cavo di rete che va al vostro router. Fine. Non ha setup, non ha IP, non ha pagine di configurazioni.

Secondo Fastweb, dovrebbe essere finita qui (lol). In pratica, manca un enorme dettaglio, ovvero quali parametri inserire nella porta WAN del router che andremo ad usare.

### Scelta del router...

Se state già pensando di usare il vostro Netgear di 10 anni fa o il TP-Link da 15 euro comprato al Black-Friday, scordatevelo! **Il primo requisito** del router da usare per sostituire il FASTGate è che sia **performante**, perché deve essere in grado di gestire un traffico NAT WAN-to-LAN di 1Gbps.

Ebbene sì, benvenuti nel magico mondo delle connessioni domestiche ultra-veloci, dove anche in casa servono router carrozzati e performanti per poter sfruttare tutta la banda offerta dal provider.

Se volete sapere a grandi linee le performance del router che avete o che volete acquistare, potete fare riferimento a questa tabella comparativa di SmallNetBuilder:

[https://www.smallnetbuilder.com/tools/charts/router/bar/179-wan-to-lan-tcp/35](https://www.smallnetbuilder.com/tools/charts/router/bar/179-wan-to-lan-tcp/35)

Questo però non è l'unico requisito che il router deve rispettare. Ce n'è un altro, che è fondamentale.

### ...con supporto dhcp option 60 sulla WAN

Per sostituire il FASTGate con un router qualsiasi e far sì che funzioni, quest'ultimo deve "annunciarsi" come se fosse il FASTGate. Per fare questa cosa, occorre impostare alcuni **parametri personalizzati** nel dhcp-client della WAN, ovvero il servizio che richiede l'IP al provider in fase di connessione.

Nello specifico il parametro da configurare è l'Option 60 `vendor-class-identifier` del dhcp.

Ecco, questa non è una cosa proprio diffusa. È difficile trovare dei router ad uso consumer con questa opzione configurabile. In genere tutto ciò che è basato su OpenWRT e similari e/o ha una CLI, ha la possibilità di abilitare questo parametro. Non vorrei dirvi una stupidata ma gli ultimissimi Netgear consumer di fascia alta (300+ euro) dovrebbero supportare questi parametri, ma non fidatevi di me. Verificate.In genere queste opzioni sono facilmente configurabili su router SOHO/professionali tipo MikroTik, Ubiquiti EdgeRouter, tutto ciò che è basato su OpenWRT, pfSense, etc...

### Recap dei requisiti

Quindi, punto elenco rapido di quello che serve:

* Un MediaConverter per collegare la ONT originale fornita da Fastweb
* Un router performante...
* ...che supporti la dhcp option 60 (vendor class identifier) sulla WAN

Detto ciò, vediamo nel concreto.

## Tempo di spendere soldi e collegare due cavi

Per il mio setup ho scelto di riciclare un vecchio Ubiquiti EdgeRouter-Lite che avevo nel cassetto. Per mia fortuna è un router che supporta tutti i parametri che servono e grazie all'accelerazione hardware riesce tranquillamente a gestire un throughput di 1 Gbps. Non ha il WiFi, ma per quello avevo già intenzione di prendermi un UniFi nanoHD, che per l'occasione ho acquistato insieme al MediaConverter.

{{< figure src="/images/MVIMG_20190329_132819.resized.jpg" position="center">}}
{{< figure src="/images/MVIMG_20190329_132649.resized.jpg" position="center">}}

### Colleghiamo i cavi

Iniziamo quindi a **sfilare l'ONT dal FASTGate**. Prima di procedere con questa operazione, vi consiglio di spegnere il router e lasciarlo spento per qualche minuto. L'ONT infatti si **surriscalda molto** e c'è il rischio di scottarsi durante le operazioni.

Ricollegate quindi il tutto nel MediaConverter. Prima di collegare il cavo di rete al router però, dobbiamo configurare i parametri sulla WAN.

{{< figure src="/images/IMG_20190403_000923.resized.jpg" position="center">}}

### Impostare i parametri sulla WAN del router

**La prima cosa da fare** è impostare l'interfaccia in modalità DHCP, in modo da prendersi automaticamente l'IP dal provider in fase di connessione. Questo è un setup praticamente standard che hanno tutti i router.

**La seconda cosa da fare** è clonare il MAC address del FASTGate. Il MAC Address è scritto sulla destra di un'etichetta dietro il FASTGate.

{{< figure src="/images/MVIMG_20190403_203840.resized.jpg" position="center">}}

Per impostare il MAC sulla WAN nel EdgeRouter è possibile farlo da CLI, oppure andando nella tab Config Tree dell'interfaccia web e navigare fino a `interfaces / ethernet / eth0`, inserendo poi il MAC nell'apposito campo, facendo attenzione ad inserirlo nel formato `00:00:00:00:00:00`.

{{< figure src="/images/Schermata-da-2019-04-03-20-50-46_450112b1995490a1e086956374eb0845.png" position="center">}}

Confermate la configurazione con `Preview` e `Apply`.

**La terza cosa da fare** è impostare il parametro DHCP Option 60. Per impostarlo possiamo sempre andare da CLI, oppure anche qui possiamo modificare le proprietà dal **Config Tree**. Navighiamo fino a `interfaces / ethernet / eth0 / dhcp-options` e nel campo **client-option** dobbiamo inserire questa stringa esatta:

`send vendor-class-identifier &quot;askey_HW_ES1_SW_0.00.67/dslforum.org&quot;;`

{{< figure src="/images/Schermata-da-2019-04-03-20-56-57_2e6aafa7f4b30c73220b551db0b42e5a.png" position="center">}}

Confermate sempre con `Preview` e `Apply`.

**NOTA IMPORTANTE**: la stringa esatta da inserire dovrebbe essere `send vendor-class-identifier "askey_HW_ES1_SW_0.00.67/dslforum.org";` tuttavia l'EdgeRouter restituisce un errore se si usano gli " nella stringa, quindi li ho sostituiti con `&quot;`.

A questo punto, dovrebbe essere tutto pronto.

### Test Drive

Colleghiamo il cavo di rete alla porta WAN del nostro router.

{{< figure src="/images/IMG_20190403_001118.resized.jpg" position="center">}}

I LED sul MediaConverter dovrebbero accendersi e nel giro di qualche secondo il router dovrebbe effettuare la connessione.

{{< figure src="/images/Schermata-da-2019-03-31-16-02-01.resized.jpg" caption="Connected! Velocità di download molto vicina alla nominale da contratto." >}}

Fate un test di navigazione e se avete fatto tutto correttamente dovrebbe funzionare tutto!

A questo punto potete prendere il vostro FASTGate e fargli fare la fine che merita...

{{< figure src="/images/MVIMG_20190331_180053.resized.jpg" position="center">}}

## Conclusioni

Come avete visto, sostituire il router fornito da Fastweb è possibile, tuttavia è un'operazione non alla portata di tutti. Richiede un hardware comunque al pari se non superiore al FASTGate (che tecnicamente schifo non fa) e un minimo di competenze tecniche e di networking per destreggiarsi tra collegamenti di rete e stringhe strane.

**A questo punto però, a molti di voi verrà spontanea una domanda: perché sostituire il router del provider con un router di proprietà? Cosa si guadagna? Il FASTGate non va bene?**

Ebbene, non è tanto l'hardware il problema in questo caso, ma il software.

Il software che gestisce questi router infatti è sviluppato su misura dal provider stesso e generalmente è un'accozzaglia di codice scritto male e senza criterio. Come risultato si ottiene un firmware con poche funzioni, spesso basilari e quelle poche funzioni avanzate che ci sono hanno bug assurdi che ne compromettono il corretto funzionamento.

Un esempio? Il famoso caso di Destiny e il router "scolapasta" di TIM:  
[https://www.bungie.net/it/Forums/Post/242575705](https://www.bungie.net/it/Forums/Post/242575705)

**A questo punto qualcuno dirà... vabbé ma io devo solo navigare sull'internetto, cosa me ne frega di tutto il resto? Non posso stare col mio bel router che collego e funziona?**

Sì, certamente, però qui entriamo nel secondo problema di questi router, a parer mio il più grave e che riguarda praticamente il 100% degli utenti: la sicurezza e la privacy.

Come sicuramente saprete, questi router dei provider sono famosi per essere **telegestiti**, ovvero il provider può tecnicamente entrare nel vostro router e apportare modifiche da remoto. Queste modifiche generalmente sono aggiornamenti firmware, e fin qui nulla di male, però non si limitano a questo: possono tecnicamente modificare il funzionamento del router, estrapolare informazioni e vedere quello che c'è all'interno della vostra rete domestica, vedere i vostri dispositivi, vedere se avete un iPhone, un Android, un Roomba, una telecamera, e così via.

Se si considera la rete locale come le quattro mura di casa, è come se il vostro fornitore del gas venisse a casa vostra a vedere cosa state cucinando sui fornelli tutte le sere. Non solo, volendo ha la possibilità di decidere arbitrariamente di regolarvi la potenza della fiamma. Non suona come una bella cosa, vero?

**Però anche qui, sono quasi certo che qualcuno dirà "è il mio provider, sono consapevole e non ho nulla da nascondere. Mi fido!".**

Ebbene, durante questa fase di setup del nuovo router mi sono accorto per puro caso che dal mio IP pubblico con il vecchio FASTGate risultava una **porta aperta in ingresso** da internet. (Premessa per i neofiti: a meno di configurazioni particolari e volute, le porte in ingresso da internet devono essere assolutamente **chiuse**!)

Verificando la configurazione del FASTGate non ho trovato alcuna traccia di una configurazione che citasse questa porta, né tanto meno che fosse aperta indiscriminatamente a tutti.

Al che mi è venuto un dubbio: se non fosse solo un mio problema?

Facendo una velocissima ricerca su **Shodan**, ho scoperto di non essere solo. Cercando infatti per questa porta e isolando la ricerca sul suolo italiano, di quasi 59.000 risultati, **ben 44.000 sono di Fastweb** con un nettissimo distacco dal secondo posto.

{{< figure src="/images/Schermata-da-2019-04-03-22-10-17-1.png" position="center">}}
{{< figure src="/images/Schermata-da-2019-04-03-22-10-39_2c33668b1461ab69de2885337110f5e9-2.png" position="center">}}

Abbiamo quindi una situazione dove il router telegestito e controllato dal provider espone su internet un servizio del router non chiaramente identificato, aperto a tutti, in modo assolutamente silente, lasciando quindi il cliente ignaro della situazione di potenziale pericolo.

Il rischio, seppur remoto, è che qualcuno possa tentare di sfruttare delle potenziali vulnerabilità del servizio esposto, prendendo così il controllo dell'apparato e accedere così all'interno della vostra rete. Se mettiamo insieme anche il fatto che spesso il firmware di questi router è scritto coi piedi, il rischio di subire una violazione non sembra poi così remoto.

Ed ecco quindi che il provider, di cui vi fidate, può commettere degli errori e mettere in pericolo la vostra sicurezza e la vostra privacy.

Quindi, tornando alla domanda originale, perché sostituire il router del provider con un router proprietario?

La risposta è molto semplice:

**PRIVACY.**

**SICUREZZA.**

**LIBERTÀ.**



