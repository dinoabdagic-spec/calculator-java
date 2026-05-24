[testrezultat.md](https://github.com/user-attachments/files/28193872/testresultat.md)
# Rezultati Black Box (Sistemskog) Testiranja Kalkulatora

Tokom testiranja aplikacije po metodi crne kutije (Black Box), proveravani su različiti matematički izrazi kako bi se utvrdilo da li kalkulator radi ispravno i kako reaguje na specifične unose. Uočeni su sledeći nedostaci i softverske greške:

### 1. Greška kod deljenja sa nulom (Division by Zero)
* **Unos:** `10/0`
* **Očekivani rezultat:** Poruka o grešci (npr. "ERROR: Division by zero") ili prekid operacije.
* **Stvarni rezultat:** Kalkulator vraća rezultat `Infinity`. U pravoj matematici i stabilnim aplikacijama, deljenje nulom ne bi smelo da prođe kao uspešno izračunato bez jasne poruke korisniku.

### 2. Bag sa specijalnim karakterom za množenje (*)
* **Unos:** `5*5`
* **Očekivani rezultat:** `25.0`
* **Stvarni rezultat:** Program izbacuje grešku (`ERROR` ili se ruši). Razlog je to što kod u pozadini koristi funkciju `split()` u kojoj karakter zvezdica (`*`) ima specijalno značenje u regularnim izrazima, a pošto programer nije stavio kosu crtu ispred nje da je neutrališe, kalkulator ne može da prepozna operaciju množenja.

### 3. Problem sa redosledom operacija (Prioritet) i negativnim brojevima
* **Unos:** `2+3*-4` ili unosi sa više vezanih operacija koje uključuju promenu znaka.
* **Očekivani rezultat:** Ispravno računanje poštujući predznake i matematički prioritet.
* **Stvarni rezultat:** Kalkulator se zbuni ili vrati grešku `ERROR` jer njegova unutrašnja logika deli tekst isključivo preko simbola operacija, pa ne može da razgraniči kada je minus znak za oduzimanje, a kada označava negativan broj.

### 4. Problem sa memorijom i ponovljenim računanjem (Statičko stanje)
* **Zapažanje:** Pošto kalkulator koristi jednu istu statičku promenljivu `finalResult` za skladištenje rezultata kroz rekurziju, ako se kalkulator pokrene više puta zaredom unutar iste sesije bez potpunog resetovanja memorije, postoji veliki rizik da će stari rezultat uticati na sledeće računanje i dati netačan broj.

---

## Jedinični test (Unit Test) po ugledu na lekciju

Napravljena je test klasa `CalculatorTest` koja testira funkcionisanje metode `Calculate` pomoću `assertTrue` provere:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;
import java.util.List;
import java.util.ArrayList;

public class CalculatorTest {

    @Test
    public void testCalculate() {
        // 1. Pripremamo ulazne podatke za izraz (5 + 5)
        List<Float> numbers = new ArrayList<>();
        numbers.add(5.0f);
        numbers.add(5.0f);

        List<String> operations = new ArrayList<>();
        operations.add("+");

        // 2. Pozivamo metodu za računanje iz klase Calculator
        Calculator.Calculate(numbers, operations);

        // 3. Proveravamo da li je dobijeni rezultat jednak očekivanom (10.0)
        assertTrue(Calculator.finalResult == 10.0f, "Rezultat bi morao biti 10");
    }
}
