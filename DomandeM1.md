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
Algoritmo che permette di calcolare funzioni matematiche complesse
utilizzando operazioni logiche e aritmetiche semplici con interi (utilizzi
notevoli con missione apollo; inizalmente cpu intel calcolatore con solo
operazioni intere, per utilizzare floating point bisogna acquistare un
coprocessore matematico → i calcoli di questo coprocessore venivano
svolti grazie a questo algoritmo)
Il metodo CORDIC è un processo iterativo, dove ad ogni iterazioni si esegue
la somma o sottrazione di un valore notevole, a seconda se si è superato il
valore da calcolare o meno. Per esempio, se si vuole calcolare il cos(58), si
può eseguire:
1° iterazione -> 45°
2° iterazione -> 22,5°
3° iterazione -> 11,25°
Nonostante ciò, per il calcolo della funzione goniometrica sono necessarie
altre moltiplicazioni, quelle tra i valori delle funzioni goniometriche. Questi
valori prendono il nome di cordic gain e sono costanti prendendo in
considerazioni lo stesso numero iterazioni (cambiando il numero di
iterazioni cambia il numero di fattori e di conseguenza anche il valore del
cordic gain). Per questo motivo i valori possono essere precomputati e la
moltiplicazione può anche essere eseguita via software, salvati quindi in LUT</details>

<details>
  <summary>FPGA (anche che tipi di memorie hanno)</summary>
Field Programmate
Dispositivi programmabili tramite linguaggio HDL ma anche ad alto livello come C e C++, con la possibilità di essere programmate in modo specifico con prestazioni migliori e minor consumo di energia
Cosituite da un insieme di CLB, unita esecutiva Configurable Logic Block, a loro volta composta da slice, che sono insiemi di Logic Cell: formata un Mux, una LUT ( funge da rete combinatoria programmabile) e un FlipFlop, inoltre unità moltiplicative ( NO FLOATING POINT ) e RAM.</details>

<details>
  <summary>GRS per approssimazione float</summary>
EEE 754 definisce uno standard anche per quanto riguarda l’arrotondamento, altrimenti due calcolatori diversi potrebbero produrre
risultati diversi con gli stessi numeri.
Nello specifico lo fa tramite tre bit nascosti, ovvero Guard (G), Round (R) e Sticky (S). Essi sono concatenati all’estremità destra per
decidere se il +1 sia necessario oppure no</details>

<details>
  <summary>Metodi efficienti per rappresentare reali su FPGA (Fixed-Point/BFloat/CORDIC)</summary>
Cordic, Fixed Point e altri formati FP, sicuramente piu leggeri rispetto IEEE 754 32/64 bit </details>

<details>
  <summary>Fixed-Point</summary>
  Il formato Fixed-Point rappresenta i numeri con una precisione fissa, risultando efficiente in hardware ma meno flessibile del floating point.
  Il loro limite principale è l'intervallo limitato ( -2 <sup>k</sup> ; 2<sup>k</sup> k cifre intere) ma ciò rappresenta anche il suo punto di forza, ovvero una precisione costante lungo tutto il range rappresentabile, al contrario invece di FP.
</details>

<details>
  <summary>Weight Sharing (relativa al mio progetto)</summary>
   Tecnica con lʼobbiettivo di memorizzare un numero
minore di pesi. Infatti, partendo da una griglia di pesi, si può ricavare dei
cluster di i pesi simili tra loro e per ciascun cluster si può calcolare il
rispettivo centroide (una sorta di valore medio). In questo modo, al posto di
memorizzare i singoli pesi, si possono memorizzare solamente i centroidi
ed una tabella di lookup che per ogni peso iniziale indica il centroide con il
quale è stato sostituito.
</details>

<details>
  <summary>Tecniche per ridurre la memoria usata da un software</summary>
1) Quantizzazione dei Dati
  Passare per esempio da FP32 a E5M2 riduce la memory footprint di 1/4, con il difetto che la rete risultante sara molto meno accurata di quella originale e necessiterà di un fine tuning.
2) Weight Sharing o Paletizzation
  Tecnica per ridurre il consumo di memoria utilizzando un numero ridotto di pesi
3) Utilizzazione di Interi o Fixed Point
</details>
