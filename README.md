# Izveštaj o statičkoj analizi koda i metrikama

## 1. Broj linija koda (LOC)
* **Ukupno linija u fajlu:** 188
* **Stvarni broj linija koda (bez praznina):** 134

---

## 2. Pregled koda i uočene greške (Zapažanja)

Ovo su stvari koje sam primetio tokom pregledanja koda u fajlu `Calculator.java`, a koje bi mogle da se napišu bolje ili lakše za razumevanje:

Calculator.java – 4 – static float finalResult; -> Promenljiva finalResult je stavljena da bude statička van metoda. Ovo može da napravi problem ako aplikaciju koristimo više puta zaredom, jer stara vrednost može da ostane upisana i pokvari sledeće računanje.
Calculator.java – 11 – public static String ToString() -> Ime metode 'ToString' počinje velikim slovom. U Javi je pravilo da se metode pišu malim početnim slovom (treba da bude 'toString').
Calculator.java – 20 – expression = 0 + expression; -> Sabiranje nule i stringa na ovaj način je malo čudno rešenje da se reši problem sa predznakom, može se uraditi jasnije kroz tekstualne funkcije.
Calculator.java – 22 – String[] numbers = expression.split(...) -> Kod funkcije split, karakter zvezdica (*) ima specijalno značenje u programiranju i regularnim izrazima. Pošto nije stavljena kosa crta ispred njega da ga "neutrališe", program može da se sruši tokom rada.
Calculator.java – 40 – catch (Exception exc) -> U try-catch bloku se hvata uopšteni 'Exception'. Bolje je napisati tačan naziv greške koja se očekuje, a to je u ovom slučaju 'NumberFormatException' (kada tekst ne može da se pretvori u broj).
Calculator.java – 46 – private static void Calculate(...) -> Još jedno kršenje pravila pisanja koda u Javi. Metoda 'Calculate' počinje velikim slovom umesto malim.
Calculator.java – 52 – float result = 0; -> Promenljiva 'result' je podešena na nulu, ali se ta nula nigde dalje ne koristi jer se vrednost odmah u sledećim koracima menja, pa je ova nula višak.
Calculator.java – 57 – result += numbers.get(indexMultiply) * numbers.get(indexMultiply + 1); -> Koristi se '+=' bez potrebe. Pošto se odmah nakon toga radi rekurzivni poziv i izlazi iz metode, obično znak jednako '=' bi uradio isti posao i kod bi bio jasniji.
Calculator.java – 54 – Kod ima previše 'if' naredbi koje se ponavljaju. Skoro isti blokovi koda su napisani za množenje, deljenje, sabiranje i oduzimanje. Sve je moglo da se spakuje u kraću i pregledniju strukturu (npr. pomoću aswitch-case naredbe) kako kod ne bi bio ovoliko dugačak.
