## Kapitel 1.1 (*ej obligatoriskt*)
* *Intractable ("NP-svåra") problems*: Problem som *kan* lösas av datorer, dock ej i någon praktisk bemärkelse.

### Automater
#### Finita automater
Ett system som kan representeras av ett ändligt antal tillstånd. Eftersom antalet tillstånd är ändligt kan hela
systemets historik inte generellt sparas. Systemet måste därför designas noggrannt, så att endast väsentlig information
sparas.

Yttre påverkan på systemet representeras som *"inputs"* (t.ex. "Push" som kan tänkas representera ett knapptryck).

Exempel: On/off-switch (två tillstånd med dubbelriktad övergång)

## Kapitel 1.5 - Centrala begrepp inom automatteori<font color="red">*</font>
### Alfabet
Ett *alfabet* är en finit, icke-tom mängd av symboler. Normalt betecknas alfabet med $\Sigma$. Exempel:

* $\Sigma=\{0,1\}$, det *binära* alfabetet
* $\Sigma=\{a,b,\dots,ö\}$, alfabetets gemener
* Alla tecken i en viss teckenuppsättning, t.ex. ASCII.

### Strängar
Strängar (eller *ord*) är finita sekvenser av symboler ur ett visst alfabet. T.ex. är $01101$ en stäng ur det binära alfabetet $\Sigma=\{0,1\}$.

* Den tomma strängen ($\epsilon$) innehåller inga förekomster av symboler, och kan därför skapas ur alla alfabet.
* Längden av en sträng betecknas t.ex. $|\epsilon|=0$ (tomma strängen).
* Mängden av alla strängar med längd $n$ ur ett alfabet $\Sigma$ betecknas $\Sigma^n$. På motsvarande sätt kan vi skriva $0^3=000$.
* Mängden $\Sigma^*$ består av alla möjliga strängar, medan $\Sigma^+$ inte innehåller den tomma strängen (dvs. $\Sigma^*=\Sigma^+\cup\{\epsilon\}$).
* Konkatenering av strängar $x$ och $y$: $xy$

### Språk
Ett *språk* är en mängd strängar, en delmängd av $\Sigma^*$, där $\Sigma$ är något språk. Om $L\subseteq\Sigma^*$ är $L$ ett språk över $\Sigma$. Notera att ett språk inte nödvändigtvis måste innehålla alfabetets alla symboler. Därmed är $L$ också ett språk över alla alfabet som är utökningar av $\Sigma$.

Några vanliga språk:

* $\theta$, det tomma språket, är ett språk över alla alfabet
* $\{\epsilon\}$, språket bestående endsat av den tomma strängen, är också ett språk över alla alfabet. Obs! $\theta\neq\{\epsilon\}$, eftersom den förstnämnda inte innehåller några strängar, medan den senare innehåller en sträng (den tomma strängen $\epsilon$).

*Ett språk kan innehålla oändligt många strängar, men alfabet måste vara ändliga.*

> Bevisexempel:
> 
> Om vi kan visa att det är svårt att avgöra huruvuda en sträng tillhör ett (programmerings)språk $L_X$, följer att det är lika svårt att kompilera koden, eftersom att, om detta vore möjligt, skulle utgången av kompileringsprocessen kunna användas för att besvara frågan om strängen tillhör språket ("ja" om detta lyckas, annars "nej").
> 
> Detta är ett motsägelsebevis för att *"om det är svårt att kontrollera medlemskap i $L_X$, är det också svårt att kompilera program skrivna i språket $X$"*.

## Kapitel 2 (*ej obligatoriskt*)
Reguljära språk - Ett språk som kan beskrivas av en finit automat.

När en automat mottar ett "meddelande" genomförs motsvarande övergång. Om ingen sådan övergång finns "dör" automaten. För att undvika detta används en ögla, vilket innebär att dessa meddelanden bortses ifrån (automatens tillstånd är oförändrat). *Observera att en automat beter sig på samma vis, oavsett från vem ett meddelande kommer.*

Om vi har flera parallella automater, som tillsammans utgör ett system, kan dessa beskrivas med en *produktautomat*. Tillståndet hos denna beskrivs då av tupler av varje ingående automats nuvarande tillstånd.

Obs! Om vi har en produktautomat i tillståndet $(i,x)$, och mottar meddelande $Z$, måste det finnas två övergångar ($i\mapsto j$ och $x\mapsto y$) för att övergången $(i,x)\mapsto(j,y)$ skall existera. Om någon av de enskilda övergångarna saknas finns heller ingen övergång för $Z$ från $(i,x)$ hos produktautomaten, och helheten "dör" då.

## Kapitel 2.2 - Deterministiska finita automater (DFA)<font color="red">*</font>
En deterministisk finit automat består av

1. En ändlig mängd *tillstånd*: $Q$
2. En ändlig mängd *indatasymboler*: $\Sigma$
3. En *övergångsfunktion*: $\delta(q,a)=p$ där $p$ är det nya tillståndet vid övergång från $q$ med indatasymbol $a$.
4. Ett *utgångstillstånd* $q_0\in Q$. I övergångsdiagram används en pil märkt "Start" för att visa utgångstillståndet.
5. En mängd slutgiltiga/mottagande tillstånd $F\subseteq Q$.

Dessa delar kan sammanfattas som

$$A=(Q,\Sigma,\delta,q_0,F)$$

> **Exempel:** En DFA som bara accepterar strängar där $01$ ingår.
> 
> Motsvarande *språk* $L=\{x01y\ |\ x\text{ och }y\text{ består av 0:or och 1:or}\}$
> 
> En DFA som accepterar sådana strängar måste alltså ha indata-alfabetet $\Sigma=\{0,1\}$.
> 
> Automaten kan vara i tre tillstånd:
> * $q_0$ - 01 har inte ännu setts, och senaste indata saknas eller är 1.
>     * Övergångarna är $\delta(q_0,0)=q_2$ och $\delta(q_0,1)=q_0$
> * $q_1$ - Vi har sett 01. Detta är ett *mottagande* tillstånd och är stabilt oavsett vidare indata (alla övergångar leder till $q_1$).
>     * Alla övergångar leder till $q_1$: $\delta(q_1,0)=q_1$, $\delta(q_1,1)=q_1$
> * $q_2$ - 01 har inte ännu setts, men senaste indata var 0.
>     * Möjliga övergångar är $\delta(q_2,1)=q_1$ och $\delta(q_2,0)=q_2$.
> 
> Automaten kan sammanfattningsvis beskrivas som
> $$A=(\{q_0,q_1,q_2\},\{0,1\},\delta,q_0,\{q_1\})$$

### DFA och språk
En DFA definierar ett språk: mängden av alla strängar som kan byggas av en sekvens av alla övergångar längs varje väg från starttillståndet till ett mottagande tillstånd.

> **Utökad övergångsfunktion:**
> 
> Den utökade övergångsfunktionen $\hat\delta(q,w)=p$ ger, givet ett utgångsläge $q$ och en sträng $w$, det tillstånd $p$ i vilket automaten befinner sig efter att ha i tur och ordning ha mottagit alla indata (symboler) från $w$.

Språket som motsvarar en DFA $A=(Q,\Sigma,\delta,q_0,F)$ definieras som

$$L(A)=\{w\ |\ \hat\delta(q_0,w)\in F\}$$

Språk som kan definieras genom en DFA kallas för *reguljära språk*.

## Kapitel 3.1 - Reguljära uttryck
Kan definiera samma språk som automater kan, dvs. reguljära språk.

I ett reguljärt uttryck finns tre grundläggande operationer:

1. $L\cup M$: *Unionen* av två språk $L$ och $M$.
2. $LM$: *Konkatenteringen* av två språk $L$ och $M$. Det resulterande språket består av alla strängar som kan bildas genom att konkatenera två strängar (en från $L$ och en från $M$).
3. $L^*$: *Star*/*closure*. Betecknar ett språk (mängd av strängar) som kan bildas genom att konkatenera valfritt antal strängar ur språket $L$ (upprepning tillåten).

## Kapitel 3.3 - Regex i unix
*(jag hoppade över)*

## Kapitel 3.4 - Algebraiska lagar för reguljära uttryck
...

