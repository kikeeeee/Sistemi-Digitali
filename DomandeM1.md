# MATTOCCIA #
<details>
  <summary>Calcolare il valore decimale di un floating point dato</summary>
  
</details>

<details>
  <summary>Numeri subnormali</summary>
  sono un sottoinsieme di numeri a virgola mobile utilizzati per rappresentare valori molto piccoli, prossimi a zero, che non possono essere codificati nel formato normalizzato standard, sappiamo che più ci avviciniamo allo zero, più l'accuracy aumenta, questo però non vale all'infinito, in quanto non si può avere un esponente negativo infinito, di conseguenza per toccare lo zero bisogna utilizzare i numeri subnormali(denormalizzati); quando infatti un esponente di avvicina al valore minimo **-expmin**, la notazione scientifica porterebbe ad un buco tra 0 e 2^-expmin, di conseguenza si introducono i numeri subnormali come __x = 0.m x 10^-expmin__ , piu la mantissa è piccola piu mi avvicino a zero.
</details>

<details>
  <summary>Calcolo di un numero floating point</summary>
  Un numero floating point si calcola decomponendolo in segno, esponente e mantissa, e applicando la formula standard del formato IEEE 754.
</details>

<details>
  <summary>Definizione e utilizzo FMA</summary>
  FMA (Fused Multiply-Add) è un'operazione che esegue una moltiplicazione seguita da un'addizione con un solo arrotondamento, migliorando la precisione.
</details>

<details>
  <summary>Che numero è questo? (conversione FP - decimale)</summary>
  La conversione da floating point a decimale si ottiene analizzando la codifica IEEE 754 e ricostruendo il valore numerico.
</details>

<details>
  <summary>FMA (accurancy, un solo rounding, numero di istruzioni ridotto)</summary>
  L'FMA è utile perché riduce il numero di arrotondamenti e istruzioni, aumentando la precisione e l'efficienza dei calcoli.
</details>

<details>
  <summary>FMA Fixed Point, vantaggi e svantaggi</summary>
  Il Fixed Point con FMA ha il vantaggio di migliorare l'efficienza computazionale, ma può introdurre limitazioni nella rappresentazione dei valori.
</details>

<details>
  <summary>Floating Point, vantaggi e svantaggi, codifica esponente</summary>
  Il floating point consente di rappresentare un ampio intervallo di numeri con precisione variabile, ma introduce errori di arrotondamento e una maggiore complessità hardware.
</details>

<details>
  <summary>"Trucco" utilizzato da CORDIC subnormali</summary>
  Il CORDIC usa iterazioni successive per calcolare funzioni trigonometriche ed esponenziali con operazioni di somma e shift.
</details>

<details>
  <summary>FPGA (anche che tipi di memorie hanno)</summary>
  Le FPGA utilizzano vari tipi di memoria come BRAM, DRAM ed EEPROM per archiviare dati e configurazioni.
</details>

<details>
  <summary>CORDIC gain</summary>
  Il gain del CORDIC è un fattore di scala introdotto dalle iterazioni successive dell'algoritmo.
</details>

<details>
  <summary>Weight sharing</summary>
  Il Weight Sharing è una tecnica per ridurre la memoria nei modelli di deep learning comprimendo i pesi attraverso la quantizzazione o la condivisione.
</details>

<details>
  <summary>GRS per approssimazione float</summary>
  Il metodo GRS (Guard, Round, Sticky) è usato negli arrotondamenti dei numeri floating point per migliorare l'accuratezza.
</details>

<details>
  <summary>Come vengono gestiti gli arrotondamenti nei float?</summary>
  Gli arrotondamenti nei float seguono le modalità definite dallo standard IEEE 754, inclusi "round to nearest" e "truncate".
</details>

<details>
  <summary>Metodi efficienti per rappresentare reali su FPGA (Fixed-Point/BFloat/CORDIC)</summary>
  Su FPGA, i numeri reali possono essere rappresentati in diversi modi come Fixed-Point, BFloat e CORDIC, ognuno con vantaggi in termini di precisione e utilizzo di risorse.
</details>

<details>
  <summary>Fixed-Point</summary>
  Il formato Fixed-Point rappresenta i numeri con una precisione fissa, risultando efficiente in hardware ma meno flessibile del floating point.
</details>

<details>
  <summary>Weight Sharing (relativa al mio progetto)</summary>
  La tecnica di Weight Sharing nel tuo progetto potrebbe ridurre la memoria necessaria comprimendo i pesi di una rete neurale.
</details>

<details>
  <summary>Tecniche per ridurre la memoria usata da un software</summary>
  Tecniche comuni includono la quantizzazione, la compressione dei dati e la riduzione della precisione dei numeri rappresentati.
</details>
