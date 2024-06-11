# ElBits NRL API

ElBits NRL API støtter asynkron innsending av NRL data til Kartverket for registrering eller validering. APIet støtter innsending på både CIM JSON-LD format og Karverkets NRL GeoJSON format. Selve prosesseringen av data skjer asynkront slik at man først sender inn data og så etterpå henter ut resultatet av registreringen eller valideringen via et annet endepunkt.

## Bruk av API

Endepunktadresser

| Miljø          | Url   |
| ------------- | ------------- |
| Dev     | https://nrl-api.dev.elbits.no  |
| Prod     | https://nrl-api.elbits.no  |


OpenAPI/Swagger-dokumentasjon for APIet er tilgjengelig i dev-miljø: https://nrl-api.dev.elbits.no/swagger/index.html

## Authentication

APIet er sikret med Bearer token som hentes med standard OAuth2 client credentials flow. Ta kontakt med NRL-teamet for å få opprettet klienter for testing og/eller produksjon. Bruk gjerne et bibliotek til å håndtere tokens, feks Duende.AccessTokenManagement for .NET.

> [!NOTE]
> Husk å gjenbruke access tokens innenfor levetiden istedenfor å hente nytt for hver request.

Se også dokumentasjon for [Altinn-oppsett](../pages/altinn.md)


###Innsending av CIM for registrering

Innsending av data på CIM JSON-LD format gjøres ved bruk av POST mot endepunkt /cim. Endepunktet tar i mot en melding på CIM-format i body.

Kallet returnerer et resultat av operasjonen i form at en melding på følgende format

```
{
  "messageId": "string",
  "publishTime": "2024-06-10T15:15:11.047Z",
  "succeeded": true,
  "errorMessage": "string",
  "statusMessage": "string"
}
```

| Felt          | Beskrivelse   |
| ------------- | ------------- |
| messageId     | Id tildelt meldingen som vil gjenfinnes når man senere henter ut resultatet av prosesseringen  |
| publishTime   | Tidspunktet innsendte data ble sendt videre for asynkron prosessering  |
| succeded      | Flagg som indikerer om selve innsendingen av vellykket eller ikke. Dette indikerer altså ikke om registrering var vellykket, bare om selve innsendingen gikk bra eller ikke.  |
| errorMessage  | En eventuell feilmelding hvis innsendingen feilet av en eller annen grunn  |
| statusMessage | Fritekstinformasjon om resultatet av innsendingen  |

Hvis innsending av data var vellykket vil det igangsettes an asynkron prosessering av innsendte data. Hvert enkelt objekt vil bli prosessert hver for seg og resultatet av
prosesseringen vil være tilgjengelig som en separat melding i endepunkt for *responsemessages*


### Innsending av CIM for validering

### Innsending av NRL GeoJSON for registrering

Se Kartverkets dokumentasjon for informasjon om hvordan GeoJson brukes for NRL:  https://sosi.geonorge.no/produktspesifikasjoner/NRL-rapportering/

### Innsending av NRL GeoJSON for validering

### Uthenting av resultater av registrering og validering
