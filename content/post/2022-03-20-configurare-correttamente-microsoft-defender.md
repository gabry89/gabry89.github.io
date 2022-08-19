+++
title = "Configurare correttamente Microsoft Defender"
date = 2022-03-20T14:18:12Z
description = "Windows include Microsoft Defender, un software antimalware integrato con il sistema in grado di proteggere il computer dalle minacce più recenti"
draft = false
cover = "/images/defender-header.webp"
slug = "configurare-correttamente-microsoft-defender"
summary = "Windows include Microsoft Defender, un software antimalware integrato con il sistema in grado di proteggere il computer dalle minacce più recenti"
+++


Windows include Microsoft Defender, un software antimalware integrato con il sistema in grado di proteggere il computer dalle minacce più recenti.

Negli ultimi anni Microsoft ha fatto enormi investimenti ed evoluzioni per alzare il livello di sicurezza di base di Defender. I primi tentativi lasciavano un po' a desiderare, tuttavia recentemente è arrivato a competere con le soluzioni di protezione più famose e in alcuni casi ne esce persino migliore.

Defender, nella sua configurazione di base, offre una protezione che si potrebbe considerare già sopra la media, tuttavia nasconde delle opzioni che possono essere attivate manualmente per incrementare di molto il livello di protezione e la sicurezza dell'intero sistema.

Alcune di queste opzioni si attivano dalle Impostazioni di Windows, altre invece sono un po' più nascoste e si attivano solamente tramite dei comandi PowerShell.

Fortunatamente, esiste un semplice strumento di terze parti che permette di configurare tutte queste opzioni da una comoda interfaccia.

> Se si possiede una soluzione di protezione a pagamento con l'abbonamento scaduto, occorre disinstallarla immediatamente. Una protezione in abbonamento senza licenza è totalmente inefficace e vi fornisce una falsa sensazione di sicurezza!

## Configure Defender

Lo strumento si chiama Configure Defender e lo possiamo scaricare da questo repository GitHub. L'eseguibile da scaricare è "ConfigureDefender.exe".

[https://github.com/AndyFul/ConfigureDefender](https://github.com/AndyFul/ConfigureDefender)

All'apertura veniamo accolti con una serie di pulsanti e selettori, raggruppati per sezioni.

{{< figure src="/images/configure-defender.png" caption="L'interfaccia principale di ConfigureDefender" position="center">}}

È possibile andare ad impostare ogni singola voce, oppure usare delle configurazione pre-impostate ottimali.

Nello specifico, l'impostazione che consiglio di attivare è **HIGH**.

L'impostazione HIGH attiva molte delle funzionalità di protezione contro gli exploit, attiva la Network Protection e molte delle regole di riduzione della superficie di attacco (ASR).

**Per attivarla è sufficiente cliccare su HIGH, attendere qualche secondo la comparsa della finestra di conferma e riavviare il computer.**

{{< figure src="/images/image-2.png" caption="Avviso di conferma di avvenuta configurazione" position="center">}}

**Fatto! In pochi minuti avete appena eseguito un aggiornamento veramente sostanziale a Microsoft Defender e alla sicurezza del vostro computer.**

>Questa configurazione funziona solamente se non sono presenti altre soluzioni di protezione antivirus installate nel sistema (es. McAfee, Norton, etc...).

Ma cosa abbiamo fatto? Se siete curiosi, vediamo un po' nel dettaglio qualche impostazione.

## Funzionalità antimalware attivate con l'impostazione HIGH

#### PUA Protection [On]

Le applicazioni potenzialmente indesiderate (PUA) sono una categoria di software che può rallentare il funzionamento del computer, visualizzare annunci indesiderati o, nel peggiore dei casi, installare altro software senza il vostro consenso. Protezione imprescindibile.

>_Approfondimento tecnico:_
>
>[https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/detect-block-potentially-unwanted-apps-microsoft-defender-antivirus?view=o365-worldwide](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/detect-block-potentially-unwanted-apps-microsoft-defender-antivirus?view=o365-worldwide)

#### Cloud Protection Level [Highest]

La protezione Cloud è la forma di protezione più avanzata presente nei moderni antivirus e antimalware. Usa la potenza di elaborazione del Cloud per determinare se un file è o meno una minaccia. Per farla breve, abbiamo girato la manopola un po' più aggressiva.

>_Approfondimento tecnico:_
>
>[https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/specify-cloud-protection-level-microsoft-defender-antivirus?view=o365-worldwide](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/specify-cloud-protection-level-microsoft-defender-antivirus?view=o365-worldwide)

#### Cloud Check Time Limit [20s]

Quando Microsoft Defender trova un nuovo file sospetto, può impedire l'esecuzione del file mentre verifica sul servizio Cloud di protezione l'esito della scansione. Questa funzione si chiama BAFS (Block At First Sight).

Il periodo predefinito per il blocco del file è di 10 secondi, con questa impostazione l'abbiamo portato ad un massimo 20 secondi.

Terminati i 20 secondi, se Defender non ha ancora ricevuto una risposta, il file verrà sbloccato ed eseguito, ma la scansione continuerà comunque nel Cloud fino alla ricezione del risultato. Se dovesse risultare malevolo dopo i 20 secondi, il file verrà inserito in quarantena.

Nella maggior parte dei casi, il risultato della scansione Cloud avviene ben prima dei 20 secondi.

>_Approfondimento tecnico:_
>
>[https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-cloud-block-timeout-period-microsoft-defender-antivirus?view=o365-worldwide](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-cloud-block-timeout-period-microsoft-defender-antivirus?view=o365-worldwide)

#### EXPLOIT GUARD Attack Surface Reduction (ASR)

Nella sezione EXPLOIT GUARD di Configure Defender vengono riportate tutte le impostazioni attivate per la riduzione della superficie di attacco.

{{< figure src="/images/image-3.png" caption="Sezione relativa alle regole ASR" >}}

Queste funzionalità di riduzione della superficie di attacco sono l'introduzione più interessante fatta da Microsoft negli ultimi anni. Le regole ASR mirano a chiudere o disattivare funzionalità di sistema pericolose che non vengono quasi mai usate dagli utenti, ma vengono spesso abusate dai malware per eseguire comandi malevoli e ottenere l'accesso al computer.

Per dirla breve, mette le sbarre a quella finestra che non usate mai e non sapete di avere, ma che fa tanto gola ai ladri.

Le regole ASR sono numerose e la loro spiegazione nel dettaglio può risultare complessa per chi non è molto pratico. Oltretutto, l'attivazione di una regola ASR può comportare dei falsi positivi, ovvero il blocco del funzionamento di una applicazione benigna.

La configurazione a HIGH attiva solamente le regole ASR che nel 99% dei casi non causano problemi nell'uso quotidiano, ma bloccano la stragrande maggioranza delle funzionalità di Windows usate dai malware. Per fare qualche esempio:

* impedisce alle macro di Office di fare chiamate di sistema pericolose
* impedisce agli scripts di lanciare eseguibili scaricati da internet
* impedisce l'esecuzione di scripts offuscati, spesso nascosti negli allegati e-mail
* blocca l'esecuzione di processi non firmati da chiavette USB

La lista delle regole ASR è in costante aumento e verranno introdotte molte altre regole nel prossimo futuro.

>_Approfondimento tecnico:_
>
>[https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide)

>_Se volete leggere l'esperienza di un team tecnico che ha implementato le ASR in ambiente business corporate:_
>
>[https://blog.palantir.com/microsoft-defender-attack-surface-reduction-recommendations-a5c7d41c3cf8](https://blog.palantir.com/microsoft-defender-attack-surface-reduction-recommendations-a5c7d41c3cf8)

#### Network Protection [ON]

La Network Protection è abbastanza parlante e il suo funzionamento è molto semplice: impedisce di accedere a domini e IP pericolosi come phishing, scam e siti che ospitano malware.

{{< figure src="/images/image-4.png" caption="Notifica di blocco" position="center">}}

## Altre impostazioni avanzate per esperti

Come avrete notato, Microsoft Defender presenta molte altre funzioni che la configurazione HIGH non ha attivato. Sono funzioni avanzate, che possono generare falsi positivi se non vengono gestite correttamente.

Consiglio di approfondire il funzionamento solo se si è consapevoli e se sapete fare una ricerca tecnica su Google.

Per esempio la funzione di Controlled Folder Access permette di impedire ai ransomware di cifrare le cartelle dei documenti, ma la protezione è estremamente severa e il rischio di bloccare l'accesso ad applicazioni sicure è molto frequente. Attivatela solo se siete disposti a voler gestire qualche falso positivo da aggiungere alle esclusioni.

Volendo è possibile impostare Defender in modalità "paranoia", bloccando tutti gli eseguibili che non hanno raggiunto un certo livello di prevalenza, età o non vengano aggiunti ad una lista sicura da parte di Microsoft.

{{< figure src="/images/image-5.png" >}}

È una funzionalità estremamente utile per i computer che necessitano di un livello di protezione senza compromessi, ma il rischio di vedersi bloccato qualche aggiornamento di un software che usate solo voi e pochi altri è estremamente elevato.

## Testare il corretto funzionamento

La configurazione proposta è pensata per un uso massivo comune e non dovrebbe causare falsi positivi nell'uso quotidiano. Nello specifico, l'unica parte che potrebbe causare qualche falso positivo in qualche caso specifico sono le regole ASR, ma mi vengono in mente solo scenari aziendali con software particolari tipo gestionali balordi mal progettati o macro incredibilmente complesse che operano ad un livello molto integrato con il sistema.

Per un uso domestico o piccolo ufficio, questa configurazione garantisce un aumento considerevole della sicurezza con un rischio pressoché nullo di falsi positivi.

