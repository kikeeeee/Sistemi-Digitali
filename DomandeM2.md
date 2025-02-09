# TOSI #

<details>
  <summary>Differenza SIMT e SIMD</summary>
Sono due modelli di esecuzione, il primo prevede un unica istruzione eseguita da piu thread, cio significa che come SIMD un unico flusso di istruzioni governa diversi dati, in questo caso pero ognuno di questi è elaborato da un thread diverso con stato proprio ( in CUDA parallelismo può essere gestito sia a livello di warp che di thread ).
SIMD invece esegue lo stesso flusso di istruzioni su piu dati in modo parallelo, SIMD inoltre si basa sul principio vettoriale e abbiamo registri che circa come array contengono piu dati sui quali elaborare</details>

<details>
  <summary>Warp scheduler e dispatcher</summary>
Il warp scheduler è una componente essenziale dell'architettura CUDA che si occupa della gestione e dell'esecuzione dei warps, gruppi di 32 thread che operano in parallelo nei Streaming Multiprocessors (SM) delle GPU NVIDIA. Ogni SM dispone di più scheduler. Il compito dello scheduler è selezionare un warp e tra quelli eleggibili e inviarlo alle unità di esecuzione, gestendo la priorità minimizzando la latenza. Se un warp è in attesa di dati, ad esempio per un accesso a memoria globale, lo scheduler può passare a un altro warp pronto per l'esecuzione, detto eleggibile ( LATENCY HIDING ). Questo meccanismo è fondamentale per nascondere la latenza delle operazioni di memoria e massimizzare il throughput della GPU. In uno scenario ideale, il numero di warps attivi per scheduler dovrebbe essere sufficiente a coprire le latenze delle istruzioni, garantendo un utilizzo costante delle unità computazionali.
  La dispatcher unit lavora a comando dello scheduler ed è responsabile della decodifica delle istruzioni del warp, assegnando successivamente i thread del warp alle relative unità di calcolo

L’efficienza di questo meccanismo dipende dalla capacità di mantenere un numero sufficiente di warps attivi per nascondere le latenze e sfruttare al massimo le risorse della GPU. Se il numero di warps attivi è troppo basso, gli scheduler possono rimanere inattivi, causando un sottoutilizzo delle unità computazionali. Al contrario, un numero eccessivo di warps può portare a competizione per le risorse di memoria e registri, riducendo l'efficienza. Un aspetto critico dell'ottimizzazione CUDA è quindi trovare un equilibrio tra questi fattori, minimizzando la divergenza dei warps e garantendo un accesso efficiente alla memoria​.
</details>

<details>
  <summary>Giga Thread Engine e Occupancy</summary>
Il GigaThread Engine è un componente centrale dell'architettura delle GPU NVIDIA, agisce come scheduler globale per la distribuzione dei blocchi all'interno di un SM, quando viene lanciato un kernel, tenendo presente i limiti architettuturali e la disponibilità delle risorse.
L'occupancy è il rapporto tra warp attivi e massimo numero di warp supportati per SM, ne misura infatti il grado di risorse effettivamente utilizzate; si divide in occupancy teorica ovvero occupancy massima raggiungibie da un kernel basato sui limiti e configurazioni, e occupancy effettiva che rappresenta il reale numero di warp attivi; aumentare l'occupancy teorica è sicuramente un primo passo verso l'idea di aumentare quella effettiva, che però può risultare comunque inferiore a quella teorica a causa di carico di lavoro sbilanciato tra i blocchi oppure un numero di blocchi lanciati insufficente, questo può essere sia causato da un'errata configurazione della GRID, ma anche da un utilizzo improprio delle risorse ( shared memory ).
Inoltre tra le cause inseriamo anche le wave, ovvero il numero di blocchi attivi che possono essere eseguiti per SM, se ( tipicamente alla fine dell'esecuzione ) si arriva con un un numero di blocchi rimanenti minore rispetto a quello supportato, l'occupancy andrà per forza a calare.

</details>

<details>
  <summary>Shared memory, come accedere (banchi) e come allocare</summary>
Memoria temporanea che funghe da canale di comunicazione per tutti i thread di un blocco, aumenta la banda disponibile e riduce la latenza, si trova piu vicina alle unità di un SM rispetto a una cache L2.
  Ne viene allocata una quantita fissa ad ogni blocco di thread, all inizio della sua esecuzione, e dura tutto il ciclo di vita di un SM; gli accessi alla memoria avvengono per warp ( caso migliore 1 transazione, peggiore 32).
  SMEM è una risorsa limitata che dipende dall architettura di una GPU, un uso eccessivo riduce il numero di blocchi di thread attivi concorrentemente, e quindi limita il parallelismo.
  Dopo ogni elaborazione/caricamento, è necessario eseguire una sincronizzazione in quanto è possibile che altri thread debbano utilizzare quei dati.
  L'allocazione può essere sia dinamica che statica, in base a se la Quantità di SMEM da allocare è nota al momento di compilazione ( variabile extern).
  La memoria è uno spazio di indirizzamento lineare, ma per massimizzare la banda di memoria, la SMEM è divisa in 32 moduli di memoria di ugual dimensione chiamati banchi ( da 4/8 byte in base all architettua) , essi sono 32 in quanto numero di thread di un warp, potendo permettere la lettura simultanea da parte di tutti i thread.
  -scenario ideale => operazione di lettura o scrittura emessa da un warp accede solo ad un indirizzo per banco, perfetto in quanto in un solo ciclo di clock effettuo tutti i trasferimenti
  -scenario NON ideale => operazione di lettura o scrittura emessa da un warp accede a piu indirizzi per banco, necessario quindi effettuare piu transazioni di memoria in quanto un banco puo servire al massimo una richiesta.
  I tipi di accesso possono essere di tre tipi: 
  1) Parallelo ( ideale ) indirizzi presenti su diversi bank che potenzialmente possono essere lette in una sola operazione
  2) Seriale (NON ideale) indirizzi presenti nello stesso bank che richiedono una serializzazione quindi delle operazioni di lettura
  3) Broadcast , tutti i thread leggono lo stesso indirizzo, il dato quindi viene trasmesso in parallelo a tutti i thread , efficente per transazioni ( 1 ) ma inefficente uso della bandwith.

</details>

<details>
  <summary>Occupancy (Definizione, Teorica VS Effettiva)</summary>
  L'occupancy è il rapporto tra warp attivi e massimo numero di warp supportati per SM, ne misura infatti il grado di risorse effettivamente utilizzate; si divide in occupancy teorica ovvero occupancy massima raggiungibie da un kernel basato sui limiti e configurazioni, e occupancy effettiva che rappresenta il reale numero di warp attivi; aumentare l'occupancy teorica è sicuramente un primo passo verso l'idea di aumentare quella effettiva, che però può risultare comunque inferiore a quella teorica a causa di carico di lavoro sbilanciato tra i blocchi oppure un numero di blocchi lanciati insufficente, questo può essere sia causato da un'errata configurazione della GRID, ma anche da un utilizzo improprio delle risorse ( shared memory ).
Inoltre tra le cause inseriamo anche le wave, ovvero il numero di blocchi attivi che possono essere eseguiti per SM, se ( tipicamente alla fine dell'esecuzione ) si arriva con un un numero di blocchi rimanenti minore rispetto a quello supportato, l'occupancy andrà per forza a calare.
Un altra cosa che puo influenzare occupancy sono i registri, per quanto siano la memoria on chip piu veloce, ce un limite architetturale e vengono allocati dinamicamente tra warp attivi, influenzando l'occupancy; come per la shared memory un minor uso permette di avere piu blocchi concorrenti per SM, e quindi maggior occupancy; se invece si eccede il limite hardware questi vengono spostati in memoria locale, che è collocata nella stessa posizione della memoria globale e presenta alta latenza e bassa banda -> REGISTER SPILLING</details>

<details>
  <summary>Zero-Copy Memory</summary>
La memoria zero-copy è una tecnica che consente al device di accedere direttamente alla memoria dell host, senza copiare esplicitamente i dati ( eccezione alle regole di mutua esclusività di memorie CPU e GPU).
Sia host che device accedono quindi a questa memoria, tramite PCI express, con trasferimenti eseguiti implicitamente quando richiesti dal kernel, è ovviamente necessario Sincronizzare accessi in memoria.
  Potremmo riassumere la memoria come una pinned dell'host, che è mappata negli indirizzi del device, senza quindi necessità di trasferimenti ( utile solo se la GPU non ha spazio oppure per pochissimi o addirittura 1 solo trasferimento, in quanto il bus PCI ha banda notevolmente ridotta rispetto alla banda della GPU)
</details>

<details>
  <summary>Shared Memory VS Cache L1</summary>
Memoria Condivisa e Cache L1 condividono lo stesso hardware on cip, ma tra loro ci sono differenze fondamentali, sui pattern di accesso, in quanto la SMEM utilizza i 32 banchi per l'accesso parallelo, mentre la cache si basa sulle linee per il caricamento, ed inoltre sul controllo, poichè al contrario della SMEM, la cache L1 non può essere minimamente toccata dal programmatore ed è interamente gestita dall hardware.
  La configurazione ottimale ( es tramite Carvout ) di queste due dipende da esigenze del kernel:
  -Piu SMEM => ideale per un uso intensivo di SMEM per ridurre latenza di accessi a global memory, attenzione all occupancy
  -Piu Cache => piu utile quando il kernel fa accessi frequenti a dati globali con buona località spaziale, oppure per ottimizzare il register spilling

  L1 -> località spaziale e temporale
  SMEM -> località spaziale
</details>

<details>
  <summary>Unified Memory</summary>
  La memoria UM ( Unified Memory ) è uno spazio di memoria virtuale unificato, che permette di accedere agli stessi dati da qualunque processore con un unico puntatore, gestita automaticamente da runtime tramite Page Migration Engine, che trasferisce tramite PCI o NVlink dati da host a device, e gestisce in modo trasparente il trasferimento causato da un eventuale Page Fault => MANAGED MEMORY.L'allocazione avviene in modo lazy, le pagine vengono allocate solo al primo utilizzo e possono migrare in base alle necessità.
  I vantaggi sono allocazione unica, unico puntatore e semplificazione, gli svantaggi che presenta latenza aggiuntiva, in base al num di page fault</details>

<details>
  <summary>Gerarchia di memoria CUDA</summary>

Si compone cosi: 
-Registri -> on chip, memoria piu veloce, privata per ogni thread usata per variabili temporanee, soggetti a Register Spilling -> si passa in memoria locale.
-Shared Memory -> on chip, condivisa tra thread di un blocco per comunicazione e cooperazione
-Caches -> memoria intermedia automatica, riduce tempi di accesso per dati usati frequentemente
-Memoria Locale -> Off chip, alta latenza poiche risiede nello spazio della DRAM, privata per ogni thread usata per grandi variabili o registri spillati ( dati memorizzati anche in cache L1)
-Memoria Costante -> Off chip,read only, dati che non cambiano, accessibile a tutti i thread di un kernel.
-Memoria Texture -> Off chip,read only, per accessi spazialmente coerenti ( es elaborazione immagini)
-Memoria Globale -> Off chip, memoria del device, caratterizzata da alta latenza e capacità, accessibile da ogni thread in ogni SM, 
Piu si va verso l'alto, piu le memorie sono veloci, con meno latenza, ma meno capienti.
</details>

<details>
  <summary>Metodi per gestire i trasferimenti di memoria da host a device</summary>
1)pinned
2)zero-copy
3)UVA
4)UM
</details>

<details>
  <summary>Accesso ai dati nei warp e organizzazione della memoria shared</summary>
Istruzioni ed operazioni di memoria sono emesse ed eseguite per Warp, suddivisi per pattern:
Allineati => i vari indirizzi di accesso alla memoria debbano essere multipli del primo indirizzo di accesso
Coalescenti => che gli indirizzi di accesso siano contigui
</details>

<details>
  <summary>?????????????????????????Organizzazione della GDDR e shared memory</summary>
  La GDDR è usata per la memoria globale, mentre la shared memory è locale a ciascun multiprocessore.
</details>

<details>
  <summary>Legge di Little e stati dei warp, latency hiding e ILP/TLP</summary>
  La legge di little permette di calcolare il numero di warp necessari per mascherare la latenza:
  Num Warp ( per nascondere latenza ) = Latenza ( tempo di completamento istruzione) x Throughput ( num di warp eseguiti a ciclo)
  Latency Hiding tecnica per mascherare i tempi di attesa, attraverso esecuzione concorrente di piu warp ( ILP E TLP ). Scheduler vede quali warp sono in stallo e ne seleziona altri eleggibili.
  ILP e TLP sono due tecniche utilizzate per migliorare l'efficenza, per la prima suddividiamo una in piu istruzioni per eseguirle in modo concorrente ( PIPELINE ) , invece con TLP aumentiamo il numero di thread che possono essere eseguiti contemporaneamente.
</details>

<details>
  <summary>????????????????????Tipi di variabili e allocazione</summary>
  Le variabili possono essere allocate nei registri, shared memory, memoria globale o texture memory.
</details>

<details>
  <summary>Ricorsione in CUDA</summary>
Prima dell utilizzo di tecniche di ricorsione, la CPU era l'unica in grado di chiamare kernel e quindi c'era un continuo scambio di messaggi poco efficenti, un nuovo kernel ( griglia ) puo essere chiamato da un thread,blocco,griglia.
  E stato sensibilmente diminuito lo scambio di messaggi tra CPU e GPU mediante :
  -Dynamic Parallelism: in base alla densità di dati da analizzare, vengono lanciate piu o meno griglie "figlie" con sensibilità maggiore, quindi su "meno dati", gestita autonomamente da GPU.
Il Kernel/Griglia child ereditano attributi come SMEM/Cache L1 ( distinte ) e devono sempre terminare tutte prima che la Parent sia considerata completa,  inoltre la child è visibile a tutti i thread dello stesso blocco.
</details>

<details>
  <summary>Tipi di sincronizzazione</summary>
  CUDA fornisce sincronizzazione a livello di warp, blocco e griglia tramite __syncthreads(), fence e stream.
</details>

<details>
  <summary>Diagramma di Roofline e AI</summary>
Il modello roofline è un metodo grafico utile per rappresentare le prestazioni di un algoritmo ( Kernel CUDA ) in relazione alle capacità di calcolo e memoria di un sistema, utile per capire se un algoritmo viene limitato da problemi di calcolo o di accesso in memoria.
  L'AI ( Aritmetic Intensity ) che compone l'asse delle ascisse sul nostro grafico Roofline, misura il rapporto tra le quantita di operazioni di calcolo e il volume di dati trasferiti da/verso la memoria.
  AI = FLOPs / Bytes Trasferiti ; dove FLOPs sono il numero di operazioni in virgola mobile / Volume di dati letti e scritti dalla memoria DRAM.
  Ciò si paragona con la Soglia (AI) calcolata come il rapporto tra Massima capacita teorica di calcolo per secondo( FLOPs ) e Velocità massima con la quale i dati possono essere trasferiti tra GPU e DRAM.
Se AI < Bandwith => memory bound
  Altrimenti compute bound
</details>

<details>
  <summary>Memoria pinned e memoria globale</summary>
La memoria allocata di default dall host è pageable ( soggetta a page fault ) ovvero che il sistema operativo puo spostare i dati della memoria virtuale host in diverse locazioni fisiche, di conseguenza la GPU non puo accedere con sicurezza a questi dati ( che potrebbero non essere in RAM ma sul disco -> ciò causa un ritardo significativo se il dato deve essere letto ).
  Il trasferimento quindi avviene dal driver cuda, che alloca una memoria host pinned ( non soggetta a page fault , bloccata in ram ) , copia i dati dalla memoria in questa pinned e poi li trasferisce al device; la soluzione sarebbe allocare direttamente i dati in una memoria page locked, accessibile al device con larghezza di banda maggiore, ciò pero puo degradare le prestazioni del sistema host, poiche effettua grande pressione sulla RAM, i trasferimenti avvengono inoltre in maniera sincrona.
  La memoria globale è uno spazio logico accessibile dal kernell, i dati dell applicazione risiedono nella DRAM del device, le richieste del kernel quindi sono gestite o da DRAM DEVICE oppure da memoria on chip dell SM, tutti gli accessi in memoria globale passano attraverso cache L2 e molti anche da L1.
  Gli accessi possono essere
  -Allineati -> indirizzo multiplo della dimensione di transazione
  -Coalescenti -> quando i 32 thread di un warp accedono a un blocco di memoria contiguo, e l'HW puo combinarli in un numero ridotto di transazioni
  -Allineati e Coalescenti -> insieme di questi due, ottimizza di molto il throughput
</details>

<details>
  <summary>Zero-Copy, UVA e UMA</summary>
La memoria zero-copy è una tecnica che consente al device di accedere direttamente alla memoria dell host, senza copiare esplicitamente i dati ( eccezzione alle regole di mutua esclusività di memorie ).
Sia host che device accedono quindi a questa memoria, tramite PCI express, con trasferimenti eseguiti implicitamente quando richiesti dal kernel, è ovviamente necessario Sincronizzare accessi in memoria.
  Potremmo riassumere la memoria come una pinned che è mappata negli indirizzi del device, senza quindi necessità di trasferimenti ( utile solo se la GPU non ha spazio oppure per trasferimenti molto piccoli , altrimenti degradano le prestazioni)
  La memoria UVA (Unified Virtual Addressing ) è una tecnica che permette a CPU e GPU di condividere lo stesso spazio di indirizzamento virtuale, non ci sono distinzioni tra puntatori host e device e ci pensa il runtime a mappare gli indirizzi virtuali a quelli fisici sulle rispettive memorie( non si possono comunque dereferenziare puntatori tra host e device -> zero copy).
  La memoria UM ( Unified Memory ) è uno spazio di memoria virtuale unificato, che permette di accedere agli stessi dati da qualunque processore con un unico puntatore, gestita automaticamente da runtime tramite Page Migration Engine, che trasferisce tramite PCI o NVlink dati da host a device, e gestisce in modo trasparente il trasferimento causato da un eventuale Page Fault => MANAGED MEMORY.L'allocazione avviene in modo lazy, le pagine vengono allocate solo al primo utilizzo e possono migrare in base alle necessità.
  I vantaggi sono allocazione unica, unico puntatore e semplificazione, gli svantaggi che presenta latenza aggiuntiva, in base al num di page fault
</details>

<details>
  <summary>Shared Memory vs Cache L1</summary>
  Ogni SM ha memoria on-chip limitata condivisa tra shared memory e cache L1, ques'ultima si trova fra i thread block di un SM ed ha alta velocità e banda, con bassa latenza; comune quindi a tutte le sottopartizioni dell SM.
  La shared memory è organizzata in memory banks di uguale dimensione che permettono l'accesso simultaneo a piu dati, a condizione che i thread leggano da indirizzi diversi su banchi distinti, evitando BANK CONFLICT.
</details>

<details>
  <summary>Divergenza tra warp pre/post volta</summary>
Pre volta, il parallelismo era a livello di warp, tutti i thread di un warp eseguivano la stessa istruzione, dopodiche il parallelismo è diventato thrad parallelism.
  ITS ( Indipendent Thread Scheduling ) consente piena concorrenza tra thread indipendentemente da warp, avendo un loro PC, 
</details>


<details>
  <summary>Come accedere a matrice in modo lineare</summary>
</details>

```
