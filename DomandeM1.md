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
  In un sistema a virgola fissa, la posizione della virgola è stabilita a priori, il principale svantaggio di questo sistema di rappresentazione è il range molto limitato, numeri molto piccoli e grandi non possono essere correttamente rappresentati.
  Dall esigenza di rappresentare numeri con grandezze molto diverse, nasce floating point, basato sulla notazione scientifica: 
  N=Mantissa×Base<sup>Esponente</sup>
  riportando nella formula classica del FP: 
  N=(-1)<sup>s</sup>xMantissa×Base<sup>Esponente</sup>
La mantissa quindi rappresenta il valore, spesso normalizzato in notazione scientifica, del numero; l'esponente invece esprime di quanto debba essere spostata la virgola per ricondursi al numero originale.(Discorso non valido per numeri subnormali).
  I vantaggi sono molteplici, l'ampio range di dati rappresetabili, una precisione relativa costante ( più il numero è piccolo più è preciso ) e una standard ampiamente adottato, al contempo pero presenta vari difetti, uno su tutti quello dell'approssimazione, in quanto possiamo affermare che quasi tutti i numeri rappredsentati solo solo approssimazioni arrotondate alla codifica piu vicina disponibile; inoltre altri problemi possono essere nelle operazioni, come durante l'addizione tra un numero molto grande ed uno molto piccolo, cercando di adeguare l'esponente del numero piccolo a quello grande il valore piccolo perde sensibilmente precisione, oppure ad esempio sottrarre due numeri molto vicini, con il probabile risultato di un numero approssimato a zero.
  Tra gli svantaggi inoltre inserisco il costo, approssimatamente calcolabile come energia = mantissa<sup>2</sup>
</details>

<details>
  <summary>Cordic e "Trucco" subnormali</summary>
  Il CORDIC usa iterazioni successive per calcolare funzioni trigonometriche ed esponenziali con operazioni di somma e shift.
</details>

<details>
  <summary>FPGA (anche che tipi di memorie hanno)</summary>
  Le FPGA utilizzano vari tipi di memoria come BRAM, DRAM ed EEPROM per archiviare dati e configurazioni.
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
</details>

<details>
  <summary>Tecniche per ridurre la memoria usata da un software</summary>
1) Quantizzazione dei Dati
  Passare per esempio da FP32 a E5M2 riduce la memory footprint di 1/4, con il difetto che la rete risultante sara molto meno accurata di quella originale e necessiterà di un fine tuning.
2) Weight Sharing o Paletizzation
  Tecnica per ridurre il consumo di memoria utilizzando un n
3) Utilizzazione di Interi o Fixed Point
</details>
