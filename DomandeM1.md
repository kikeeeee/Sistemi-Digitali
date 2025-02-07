# MATTOCCIA #
<details>
  <summary>Calcolare il valore decimale di un floating point dato</summary>
  
</details>

<details>
  <summary>Numeri subnormali</summary>
  sono un sottoinsieme di numeri a virgola mobile utilizzati per rappresentare valori molto piccoli, prossimi a zero, che non possono essere codificati nel formato normalizzato standard, sappiamo che più ci avviciniamo allo zero, più l'accuracy aumenta, questo però non vale all'infinito, in quanto non si può avere un esponente negativo infinito, di conseguenza per toccare lo zero bisogna utilizzare i numeri subnormali(denormalizzati); quando infatti un esponente di avvicina al valore minimo -expmin, la notazione scientifica porterebbe ad un buco tra 0 e 2<sup>-expmin</sup>, di conseguenza si introducono i numeri subnormali come x = 0.m x 2<sup>-expmin</sup> , piu la mantissa è piccola piu mi avvicino a zero.
</details>

<details>
  <summary>Definizione e utilizzo FMA</summary>
  Introduciamo inizialmente la MAC, multiply and accumulate, una moltiplicazione seguita da un addizione del tipo Temp = A x B ; Z = Temp + C. Questo approccio convenzionale, esegue due approssimazioni ( che abbiamo visto essere delicate durante calcoli tra FP ).
  Per risolvere questo problema introduciamo FMA (Fused Multiply-Add) è un'operazione che esegue una moltiplicazione seguita da un'addizione in un unico step : Z = A x B + C con un solo arrotondamento senza dover rappresentare la variabile intermedia; nonostante la FMA sia piu dispendiosa a livello di hardware, migliora la precisione e accuratezza,e la rende tendenzialmente piu veloce rispetto alla MAC.
</details>

<details>
  <summary>Floating Point, vantaggi e svantaggi, codifica esponente</summary>
  # Rappresentazione in Virgola Mobile (Base 10)

La rappresentazione in virgola mobile è un metodo che consente di esprimere numeri con ampiezze molto diverse, utilizzando un numero fisso di cifre significative. Questo approccio si basa sulla formula:

\[
N = (-1)^s \times \text{Val} \times 10^{\text{exp}}
\]

In questa espressione, \( (-1)^s \) determina il segno del numero: se \( s = 0 \) il numero è positivo, mentre se \( s = 1 \) il numero è negativo. La parte \(\text{Val}\) rappresenta la mantissa normalizzata, cioè le cifre significative del numero, e \(10^{\text{exp}}\) indica come la posizione della virgola (o del punto decimale) deve essere spostata per ottenere il valore corretto.

Il termine "virgola mobile" nasce proprio dalla possibilità di spostare la virgola: in un sistema a virgola fissa la posizione della virgola è determinata in anticipo, il che limita la rappresentazione di numeri molto grandi o molto piccoli. Invece, grazie all'esponente, la virgola può essere "spostata" in avanti o indietro, adattando dinamicamente il numero alla scala necessaria.

Ad esempio, consideriamo il numero 123.45. Per rappresentarlo in forma normalizzata, spostiamo la virgola in modo tale che la mantissa sia compresa tra 1 e 10. In questo caso, lo scriviamo come:

\[
123.45 = 1.2345 \times 10^2
\]

Qui, la virgola è stata spostata di due posizioni a sinistra per ottenere la mantissa 1.2345, e l'esponente \(2\) ci dice esattamente quante posizioni sono state spostate. Se l'esponente fosse negativo, ciò significherebbe che la virgola è stata spostata verso sinistra, ottenendo un numero più piccolo.

L'esponente, quindi, è essenziale perché determina la scala del numero: un esponente positivo sposta la virgola a destra, aumentando il valore del numero, mentre un esponente negativo sposta la virgola a sinistra, riducendolo. Questo meccanismo consente di mantenere una precisione relativa costante, poiché la mantissa conserva sempre lo stesso numero di cifre significative, indipendentemente dalla grandezza del numero.

Grazie a questa rappresentazione, è possibile gestire numeri di dimensioni estremamente diverse senza perdere informazione rilevante, il che è fondamentale in molti ambiti scientifici e tecnici. La flessibilità offerta dallo spostamento dinamico della virgola permette di esprimere e operare con numeri grandi e piccoli in modo efficiente e preciso.

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
