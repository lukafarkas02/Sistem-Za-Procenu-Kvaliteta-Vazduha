# Sistem za procenu kvaliteta vazduha

Ekspertski sistem zasnovan na znanju, namenjen proceni i praćenju kvaliteta vazduha na osnovu podataka o koncentracijama zagađujućih materija i meteorološkim uslovima. Sistem primenom pravila definisanih u Drools bazi znanja određuje kategoriju kvaliteta vazduha, generiše upozorenja i preporuke za korisnike i omogućava analizu promena kvaliteta vazduha kroz vreme.

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
```

## Tehnologije

| Sloj | Tehnologija |
|---|---|
| Klijent | Angular 19 |
| Server | Spring Boot 2.7.9 |
| Programski jezik servera | Java 11 |
| Baza podataka | PostgreSQL |
| Baza znanja / pravila | Drools (KIE) |
| Autentifikacija | Spring Security + JWT |
| ORM / pristup podacima | Spring Data JPA |
| Build alat | Maven |

## Funkcionalnosti sistema

Sistem omogućava:

1. Registraciju i prijavu korisnika.
2. Autentifikaciju i autorizaciju korišćenjem JWT tokena.
3. Unos i čuvanje podataka o kvalitetu vazduha.
4. Procenu kategorije kvaliteta vazduha na osnovu koncentracija zagađujućih materija.
5. Korekciju procene na osnovu meteoroloških uslova.
6. Generisanje personalizovanih preporuka za korisnike.
7. Generisanje upozorenja kada se detektuju opasni uslovi.
8. Analizu događaja u realnom vremenu korišćenjem CEP pravila.
9. Zaključivanje unapred i unazad korišćenjem Drools mehanizama.
10. Dinamičko generisanje pravila korišćenjem Drools Rule Template mehanizma.
11. Administratorski pregled merenja i upozorenja.

## Drools baza znanja

Drools modul predstavlja centralni deo ekspertskog sistema i sadrži pravila za analizu podataka o kvalitetu vazduha.

Implementirana pravila obuhvataju:

- kategorizaciju kvaliteta vazduha na osnovu koncentracije PM2.5 i drugih zagađujućih materija,
- određivanje ukupne kategorije kvaliteta vazduha,
- korekciju rezultata na osnovu meteoroloških uslova,
- generisanje personalizovanih preporuka,
- detekciju naglog porasta koncentracije PM2.5,
- detekciju korelisanog rasta više zagađujućih materija,
- detekciju kontinuirano opasnog kvaliteta vazduha,
- detekciju dugotrajnog smoga,
- rano upozorenje usled pada brzine vetra,
- proveru bezbednosti aktivnosti korišćenjem backward chaining mehanizma.

Sistem takođe podržava generisanje novih pravila korišćenjem Drools template mehanizma na osnovu parametara kao što su tip zagađujuće materije, kategorija kvaliteta vazduha i minimalna i maksimalna vrednost koncentracije.

## CEP obrada događaja

Complex Event Processing (CEP) koristi se za analizu niza merenja kroz određeni vremenski period.

Na ovaj način sistem može da prepozna obrasce koji se ne mogu utvrditi analizom samo jednog merenja, kao što su:

- nagli skok koncentracije PM2.5,
- istovremeni rast više zagađujućih materija,
- dugotrajno prisustvo opasnih koncentracija,
- produženi period smoga,
- potencijalno pogoršanje kvaliteta vazduha usled promene meteoroloških uslova.

## Korisničke uloge

Sistem razlikuje dve vrste korisnika:

- **CITIZEN** — standardni korisnik koji može da prati kvalitet vazduha, upozorenja i preporuke.
- **ADMIN** — administratorski korisnik sa dodatnim mogućnostima pregleda podataka i generisanja pravila sistema.

## Pokretanje

Za pokretanje serverske aplikacije potrebno je imati instalirane Java 11, Maven i PostgreSQL.

Prvo se izgrađuje Drools KJAR modul:

```bash
mvn clean install
```

Nakon toga se pokreće Spring Boot serverska aplikacija:

```bash
mvn spring-boot:run
```

Klijentska Angular aplikacija pokreće se instalacijom zavisnosti i pokretanjem development servera:

```bash
npm install
npm start
```

## Struktura projekta

```text
├── client/                         # Angular klijentska aplikacija
│   ├── src/
│   └── ...
│
├── server/                         # Serverska aplikacija
│   ├── kjar/                       # Drools baza znanja
│   │   └── src/main/resources/
│   │       └── rules/              # DRL, CEP i template pravila
│   │
│   ├── model/                      # Domenski model
│   │
│   └── service/                    # Spring Boot aplikacija
│       └── src/main/java/
│           ├── controller/
│           ├── service/
│           ├── repository/
│           └── config/
│
└── README.md
```

## Autor

**Luka Farkaš**  
**SV63/2021**  
Diplomski rad

*Tema: Sistem za procenu kvaliteta vazduha*
