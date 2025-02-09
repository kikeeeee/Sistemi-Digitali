# TOSI #

<details>
  <summary>Differenza SIMT e SIMD</summary>
  SIMD esegue la stessa istruzione su più dati contemporaneamente, mentre SIMT permette ai thread di eseguire istruzioni indipendenti all'interno di un warp.
</details>

<details>
  <summary>Warp scheduler e dispatcher</summary>
  Il warp scheduler gestisce l'esecuzione dei warp nei CUDA core, mentre il dispatcher assegna i compiti ai diversi multiprocessori.
</details>

<details>
  <summary>Giga Thread Engine e Occupancy</summary>
  Il Giga Thread Engine gestisce il contesto e lo scheduling dei thread su GPU, mentre l'occupancy misura il livello di utilizzo delle risorse disponibili.
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
  -scenario NON ideale => operazione di lettura o scrittura emessa da un warp accede a piu indirizzi per banco, necessario quindi effettuare piu transazioni di memoria in quanto un banco puo servire al massimo una richiesta
</details>

<details>
  <summary>Occupancy (Definizione, Teorica VS Effettiva)</summary>
Un altra cosa che puo influenzare occupancy sono i registri, per quanto siano la memoria on chip piu veloce, ce un limite architetturale e vengono allocati dinamicamente tra warp attivi, influenzando l'occupancy; come per la shared memory un minor uso permette di avere piu blocchi concorrenti per SM, e quindi maggior occupancy; se invece si eccede il limite hardware questi vengono spostati in memoria locale, che è collocata nella stessa posizione della memoria globale e presenta alta latenza e bassa banda -> REGISTER SPILLING</details>

<details>
  <summary>Zero-Copy Memory</summary>
La memoria zero-copy è una tecnica che consente al device di accedere direttamente alla memoria dell host, senza copiare esplicitamente i dati ( eccezione alle regole di mutua esclusività di memorie CPU e GPU).
Sia host che device accedono quindi a questa memoria, tramite PCI express, con trasferimenti eseguiti implicitamente quando richiesti dal kernel, è ovviamente necessario Sincronizzare accessi in memoria.
  Potremmo riassumere la memoria come una pinned dell'host, che è mappata negli indirizzi del device, senza quindi necessità di trasferimenti ( utile solo se la GPU non ha spazio oppure per pochissimi o addirittura 1 solo trasferimento, in quanto il bus PCI ha banda notevolmente ridotta rispetto alla banda della GPU)
</details>

<details>
  <summary>Shared Memory (Struttura, come si accede)</summary>
  La shared memory è organizzata in banchi e consente accessi rapidi se non ci sono conflitti di bank.
</details>

<details>
  <summary>Shared Memory VS Cache L1</summary>
Memoria Condivisa e Cache L1 condividono lo stesso hardware on cip, ma tra loro ci sono differenze fondamentali, sui pattern di accesso, in quanto la SMEM utilizza i 32 banchi per l'accesso parallelo, mentre la cache si basa sulle linee per il caricamento, ed inoltre sul controllo, poichè al contrario della SMEM, la cache L1 non può essere minimamente toccata dal programmatore ed è interamente gestita dall hardware.
  La configurazione ottimale ( es tramite Carvout ) di queste due dipende da esigenze del kernel:
  -Piu SMEM => ideale per un uso intensivo di SMEM per ridurre latenza di accessi a global memory, attenzione all occupancy
  -Piu Cache => piu utile quando il kernel fa accessi frequenti a dati globali con buona località spaziale, oppure per ottimizzare il register spilling
</details>

<details>
  <summary>Unified Memory</summary>
  La Unified Memory permette a CPU e GPU di accedere agli stessi dati senza copie esplicite, gestendo automaticamente i trasferimenti.
</details>

<details>
  <summary>Gerarchia di memoria CUDA</summary>
Si compone cosi: 
-Registri -> memoria piu veloce, privata per ogni thread usata per variabili temporanee
-Shared Memory -> condivisa tra thread di un blocco per comunicazione e cooperazione
-Caches -> memoria intermedia automatica, riduce tempi di accesso per dati usati frequentemente
-Memoria Locale -> privata per ogni thread usata per grandi variabili o registri
-Memoria Costante -> read only, dati che non cambiano
-Memoria Texture -> read only, per accessi spazialmente coerenti ( es elaborazione imm)
-Memoria Globale -> memoria piu grande e lenta
Piu si va verso l'alto, piu le memorie sono veloci, con meno latenza, ma meno capienti.
</details>

<details>
  <summary>Metodi per gestire i trasferimenti di memoria da host a device</summary>
  I metodi includono memoria paginata, pinned memory, unified memory e zero-copy memory.
</details>

<details>
  <summary>Struttura fisica della shared memory</summary>
  La shared memory è divisa in banchi di memoria, con possibili conflitti di bank che rallentano l'accesso.
</details>

<details>
  <summary>Warp scheduler vs Dispatcher</summary>
  Il warp scheduler decide quale warp eseguire, mentre il dispatcher distribuisce i carichi di lavoro tra i multiprocessori.
</details>

<details>
  <summary>Accesso ai dati nei warp e organizzazione della memoria shared</summary>
  I warp accedono ai dati tramite accessi coalescenti per massimizzare l'efficienza della memoria shared.
</details>

<details>
  <summary>Organizzazione della GDDR e shared memory</summary>
  La GDDR è usata per la memoria globale, mentre la shared memory è locale a ciascun multiprocessore.
</details>

<details>
  <summary>Legge di Little e stati dei warp, latency hiding e ILP/TLP</summary>
  Num Warp ( per nascondere latenza ) = Latenza ( tempo di completamento istruzione) x Throughput ( num di warp eseguiti a ciclo)
  Latency Hiding tecnica per mascherare i tempi di attesa, attraverso esecuzione concorrente di piu warp ( ILP E TLP ). Scheduler vede quali warp sono in stallo e ne seleziona altri eleggibili.
</details>

<details>
  <summary>ILP e TLP</summary>
  L'ILP (Instruction Level Parallelism) e il TLP (Thread Level Parallelism) massimizzano l'uso della GPU eseguendo più operazioni in parallelo.
</details>

<details>
  <summary>Pinned memory, UM, UVA e page-fault</summary>
  La pinned memory accelera i trasferimenti tra CPU e GPU, mentre UM e UVA semplificano la gestione della memoria condivisa.
</details>

<details>
  <summary>Tipi di variabili e allocazione</summary>
  Le variabili possono essere allocate nei registri, shared memory, memoria globale o texture memory.
</details>

<details>
  <summary>Accesso in shared memory con e senza bank conflict</summary>
  Gli accessi senza conflitti di bank sono paralleli ed efficienti, mentre i conflitti rallentano l'accesso.
</details>

<details>
  <summary>Accessi allineati e coalescenti in memoria</summary>
  Gli accessi allineati e coalescenti massimizzano il throughput riducendo gli accessi inefficaci alla memoria globale.
</details>

<details>
  <summary>Ricorsione in CUDA</summary>
  CUDA supporta la ricorsione con limitazioni, poiché i kernel non possono eseguire chiamate ricorsive dirette senza uno stack gestito manualmente.
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
  <summary>Latency Hiding</summary>
  Il latency hiding maschera i tempi di attesa sfruttando la parallelizzazione e l'overlapping dei calcoli.
</details>

<details>
  <summary>Memoria pinned e memoria globale</summary>
La memoria allocata di default dall host è pageable ( soggetta a page fault ) ovvero che il sistema operativo puo spostare i dati della memoria virtuale host in diverse locazioni fisiche, di conseguenza la GPU non puo accedere con sicurezza a questi dati ( che potrebbero non essere in RAM ma sul disco -> ciò causa un ritardo significativo se il dato deve essere letto ).
  Il trasferimento quindi avviene dal driver cuda, che alloca una memoria host pinned ( non soggetta a page fault , bloccata in ram ) , copia i dati dalla memoria in questa pinned e poi li trasferisce al device; la soluzione sarebbe allocare direttamente i dati in una memoria page locked, accessibile al device con larghezza di banda maggiore, ciò pero puo degradare le prestazioni del sistema host, poiche effettua grande pressione sulla RAM, i trasferimenti avvengono inoltre in maniera sincrona.
  La memoria globale è uno spazio logico accessibile dal kernell, i dati dell applicazione risiedono nella DRAM del device, le ricbieste del kernel quindi sono gestite o da DRAM DEVICE oppure da memoria on chip dell SM, tutti gli accessi in memoria globale passano attraverso cache L2 e molti anche da L1.
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
