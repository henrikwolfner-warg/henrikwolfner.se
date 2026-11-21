# The Wolfins – familjekalender

Webbplatsen henrikwolfner.se med familjekalendern på /kalender. Henrik är inte utvecklare: svara på
svenska, kort och rakt, och förklara bara det som behövs.

## Så hänger det ihop

- **Hosting:** GitHub Pages från grenen `main`, rotmappen. Domänen pekas hit via DNS hos One.com
  (e-posten ligger kvar hos One.com – rör aldrig MX/TXT). En push till `main` är live inom ~10 minuter.
- **Lagring:** aktiviteter och inställningar ligger i Google Kalkylarket "The Wolfins kalender" i Henriks
  privata Google Drive. Ett Apps Script (webbapp) kontrollerar familjekoden och läser/skriver arket.
  Adressen till webbappen ligger i `kalender/config.js`.

| Fil | Vad |
| --- | --- |
| `index.html` | Startsidan: "Henrik Wolfner." där punkten är en gömd länk till /kalender/, plus appmenyn (nio prickar uppe till höger). Kalendern ska **inte** finnas i appmenyn. Nya appar läggs till som en `<li>` i menyn |
| `quiz/index.html` | Fredagsquiz: fristående quiz, spelarna kopplas ihop via PeerJS publika server |
| `kalender/index.html` | Hela kalenderappen (HTML, CSS och JS i en fil) |
| `kalender/config.js` | Webbappens adress. **Ändra aldrig** |
| `kalender/manifest.webmanifest` | App-inställningar för hemskärmen |
| `icons/` | Favicon och app-ikoner (WOLF/INS, vitt på korall #FF5A36, Bricolage Grotesque 800) |
| `CNAME` | `henrikwolfner.se`. **Ändra aldrig** |

## Regler vid ändringar i koden

1. Ändra i `kalender/index.html` (eller `index.html` för startsidan). Rör aldrig `config.js` eller `CNAME`.
2. Höj `APP_VERSION` i `kalender/index.html` vid varje ändring (format `ÅÅÅÅ-MM-DD` eller `ÅÅÅÅ-MM-DD.N`).
   Versionen visas under Inställningar så att Henrik kan se att uppdateringen är ute.
3. Kontrollera att skriptet går att tolka innan du pushar.
4. Committa direkt till `main` med ett kort svenskt meddelande, och berätta för Henrik vad som ändrats och
   att det syns inom ~10 minuter.

## Familjen

| id | Namn | Not |
| --- | --- | --- |
| `jennie` | Jennie | |
| `henrik` | Henrik | |
| `harry` | Harry | Hos familjen jämna veckor (ISO-veckor, mån–sön) |
| `doris` | Doris | Hos familjen jämna veckor |
| `ake` | Åke | |

## Kategorier (id)

`traning` Träning · `vard` Vård & sjukhus · `semester` Semester & ledigt · `skola` Skola & förskola ·
`socialt` Kalas & socialt · `gaster` Gäster & besök · `annat` Annat · `fodelsedag` Födelsedagar

## Lägga till aktiviteter

Aktiviteter läggs till som rader i fliken **Aktiviteter** i kalkylarket (via Google Sheets-koppling om den
finns, annars som en CSV som Henrik importerar: Arkiv → Importera → Lägg till i aktuellt blad, avgränsare
komma, *utan* "Konvertera text till siffror, datum och formler").

Kolumner: `ID | Titel | Datum | Till och med | Tid | Kategori | Vem | Upprepning | Data (rör ej)`.
Kalendern läser **bara** kolumnen Data (JSON); de andra kolumnerna är läsbara kopior. Varje ID måste vara unikt.

```json
{"id":"unikt-id","title":"Fotboll · Norremark","cat":"traning","people":["harry"],
 "start":"2026-11-23","end":"","allDay":false,"time":"19:00","endTime":"",
 "repeat":{"type":"kidsweeks","days":[1,3,4],"until":""},"exceptions":[],"note":""}
```

- `people: []` = hela familjen.
- `allDay: true` ger heldag; `time`/`endTime` ska då vara tomma. Flerdagars: sätt `end` (bara utan upprepning).
- `repeat.type`: `none`, `weekly`, `biweekly`, `kidsweeks` (bara veckor då Harry & Doris är hos familjen),
  `otherweeks` (veckor de inte är det), `monthly`, `yearly`. `days`: 1 = måndag … 7 = söndag.
  `until`: sista datum (valfritt). `exceptions`: datum som hoppas över.
- Plats skrivs i titeln, t.ex. "Gymnastik · Sandsbro skola".
- Befintliga aktiviteter: skolslut varje vardag barnveckor, barnens träningar (barnveckor), födelsedagar (årligen).
  Kolla arket innan du lägger till något som kan finnas redan.

Ändra eller ta bort en aktivitet: ändra JSON i Data-kolumnen (och de läsbara kolumnerna), eller ta bort raden.
