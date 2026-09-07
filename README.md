# Sistem za procenu kvaliteta vazduha

Ekspertski sistem zasnovan na znanju, namenjen proceni i praćenju kvaliteta vazduha na osnovu podataka o koncentracijama zagađujućih materija i meteorološkim uslovima. Sistem primenom pravila definiсанih u Drools bazi znanja određuje kategoriju kvaliteta vazduha, generiše upozorenja i preporuke za korisnike i omogućava analizu promena kvaliteta vazduha kroz vreme.

Sistem podržava različite mehanizme zaključivanja, uključujući zaključivanje unapred (forward chaining), zaključivanje unazad (backward chaining), obradu kompleksnih događaja (CEP) i dinamičko generisanje pravila korišćenjem Drools template mehanizma.

## Arhitektura sistema

Sistem se sastoji od tri osnovne celine:

- **Klijentska aplikacija** — realizovana u Angular-u i predstavlja korisnički interfejs preko kog korisnici mogu da pregledaju informacije o kvalitetu vazduha, upozorenja i rezultate različitih analiza. Administratorskim korisnicima omogućene su i dodatne funkcionalnosti za pregled podataka i generisanje pravila.
- **Serverska aplikacija** — realizovana korišćenjem Spring Boot framework-a i organizovana kroz više slojeva:
  - `controller` — REST kontroleri preko kojih klijentska aplikacija komunicira sa serverom.
  - `service` — poslovna logika aplikacije i komunikacija sa Drools bazom znanja.
  - `repository` — pristup podacima korišćenjem Spring Data JPA.
  - `config` — konfiguracija aplikacije, bezbednosti i Drools komponenti.
- **Drools KJAR modul** — sadrži bazu znanja i pravila koja se koriste za procenu kvaliteta vazduha, generisanje upozorenja i preporuka, kao i sprovođenje različitih tipova zaključivanja.

Serverska aplikacija je organizovana slojevito, sa jasno razdvojenim odgovornostima:

```text
Angular Client
      ↓
REST Controller
      ↓
Service Layer
      ↓
Repository  →  PostgreSQL
      ↓
Drools (KJAR)
      ↓
Rules / CEP / Forward & Backward Chaining
