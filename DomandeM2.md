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
  La shared memory è divisa in banchi di memoria, accessibili tramite il qualificatore __shared__, e viene allocata specificando il terzo parametro nel lancio del kernel.
</details>

<details>
  <summary>Occupancy (Definizione, Teorica VS Effettiva)</summary>
  L'occupancy teorica è il massimo numero di warp attivi, mentre quella effettiva dipende dalle risorse effettivamente disponibili.
</details>

<details>
  <summary>Zero-Copy Memory</summary>
  La memoria zero-copy permette alla CPU e alla GPU di condividere gli stessi dati senza bisogno di trasferimenti espliciti.
</details>

<details>
  <summary>Shared Memory (Struttura, come si accede)</summary>
  La shared memory è organizzata in banchi e consente accessi rapidi se non ci sono conflitti di bank.
</details>

<details>
  <summary>Shared Memory VS Cache L1</summary>
  La shared memory è controllata esplicitamente dal programmatore, mentre la cache L1 è gestita automaticamente dall'hardware.
</details>

<details>
  <summary>Unified Memory</summary>
  La Unified Memory permette a CPU e GPU di accedere agli stessi dati senza copie esplicite, gestendo automaticamente i trasferimenti.
</details>

<details>
  <summary>Gerarchia di memoria CUDA</summary>
  La memoria CUDA è gerarchica e include registri, shared memory, cache L1/L2, memoria globale e memoria host.
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
  <summary>Pinned memory, UM, UVA</summary>
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
  Il diagramma di Roofline mostra il rapporto tra computazione e accesso alla memoria, mentre l'AI (Arithmetic Intensity) indica l'efficienza del calcolo.
</details>

<details>
  <summary>Latency Hiding</summary>
  Il latency hiding maschera i tempi di attesa sfruttando la parallelizzazione e l'overlapping dei calcoli.
</details>

<details>
  <summary>Memoria pinned e memoria globale</summary>
  La memoria pinned permette trasferimenti DMA più veloci rispetto alla memoria globale standard.
</details>

<details>
  <summary>Zero-Copy, UVA e UMA</summary>
  Zero-Copy consente alla GPU di accedere direttamente alla RAM, UVA unifica gli spazi di indirizzo e UMA permette la gestione automatica della memoria.
</details>

<details>
  <summary>Shared Memory vs Cache L1</summary>
  La shared memory è gestita dal programmatore, mentre la cache L1 è automatica e migliora le performance della memoria globale.
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
