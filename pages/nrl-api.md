# ElBits NRL API

ElBits NRL API tilbyr asynkron innsending av NRL data til Kartverket for registrering eller validering. APIet støtter innsending på både CIM JSONLD-format og Kartverkets NRL GeoJSON-format. Selve prosesseringen av data skjer asynkront slik at man først sender inn data og så etterpå henter ut resultatet av registreringen og valideringen via et annet endepunkt.

</br>
</br>

## Bruk av API

Endepunktaddresser

| Miljø          | Url   |
| ------------- | ------------- |
| Dev     | https://nrl-api.dev.elbits.no  |
| Prod     | https://nrl-api.elbits.no  |


OpenAPI/Swagger-dokumentasjon for APIet er tilgjengelig i dev-miljø: https://nrl-api.dev.elbits.no/swagger/index.html

Basic example code for getting an access token for authenticating calls to the NRL API.

```
        var formData = new Dictionary<string, string>
        {
            {"grant_type", "client_credentials"},
            {"client_id", "<client id here>" },
            {"client_secret", "<client secret here>"}

        };

        using (var client = new HttpClient())
        {
            var content = new FormUrlEncodedContent(formData);
            var response = await client.PostAsync("https://id-mock.dev.elbits.no/connect/token", content);

            if (response.IsSuccessStatusCode)
            {
                var accessToken = await response.Content.ReadAsStringAsync();
            }
            else
            {
                throw new Exception("Unable to get access token");
            }
        }

```


</br>
</br>

## Authentication

APIet er sikret med Bearer token som hentes med standard OAuth2 client credentials flow. Ta kontakt med NRL-teamet for å få opprettet klienter for testing og/eller produksjon. Bruk gjerne et bibliotek til å håndtere tokens, feks Duende.AccessTokenManagement for .NET. Endepunkter for access tokens vises under.

| Miljø | Url |
| ----- | ------------------------------------------- |
| Dev   | https://id-mock.dev.elbits.no/connect/token |
| Prod  | https://id.elbits.no/connect/token          |

> [!NOTE]
> Husk å gjenbruke access tokens innenfor levetiden istedenfor å hente nytt for hver request.

Se også dokumentasjon for [Altinn-oppsett](../pages/altinn.md)

</br>
</br>

## Bruk av API

NRL-APIet er et HTTP basert API hvor man typisk sender inn data ved bruk av et POST kall og så etterpå henter ut resultatet ved bruk av et GET kall til endepunktet for responsemeldinger. Avhengig av mengden objekter som sendes inn kan det ta litt til før resultatet for alle objektene er tilgjengelig som en responsemelding.

</br>
</br>

### Innsending av CIM for registrering

Innsending av data for registrering på CIM JSON-LD format gjøres ved bruk av POST mot endepunkt `/cim`. Endepunktet tar i mot en melding på CIM-format i body.

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
| succeeded      | Flagg som indikerer om selve innsendingen var vellykket eller ikke. Dette indikerer kun om selve innsendingen gikk bra, og sier ikke noe om hvorvidt prosessering av meldingen i NRL gikk bra |
| errorMessage  | En eventuell feilmelding hvis innsendingen feilet av en eller annen grunn  |
| statusMessage | Fritekstinformasjon om resultatet av innsendingen  |

Hvis innsending av data var vellykket, vil det igangsettes an asynkron prosessering av innsendte data. Hvert enkelt objekt vil bli prosessert hver for seg og resultatet av
prosesseringen vil være tilgjengelig som en separat melding i endepunkt for *responsemessages*

</br>
</br>

### Innsending av CIM for validering

Innsending av data for validering på CIM JSON-LD format gjøres på samme måte som for registrering, men endepunkt som skal brukes her er `/cim/validate`.

Response for innsending for validering er identisk som for innsending for registrering som beskrevet ovenfor.

</br>
</br>

### Innsending av NRL GeoJSON for registrering

Innsending av data for registrering på NRl GeoJSON-format gjøres ved bruk av POST mot endpunkt `/nrlgeojson`. Endpunktet tar imot en melding på GeoJSON-format i body.

Kallet returnerer et resultat av operasjon på samme format som ved innsending på CIM-format. Se [Innsending av CIM for registrering](https://github.com/3lbits/nrl-docs/edit/feature/api_doc/pages/nrl-api.md#innsending-av-cim-for-registrering)

Se ellers Kartverkets [dokumentasjon for NRL-rapportering](https://sosi.geonorge.no/produktspesifikasjoner/NRL-rapportering/) for informasjon om hvordan GeoJson brukes for NRL.

</br>
</br>

###  Innsending av NRL GeoJSON for validering

Innsending av data for validering av data på NRL GeoJSON-format gjøres ved bruk av POST mot endpunkt  `/nrlgeojson/validate`, og fungerer ellers likt som for innsending for registrering av data på NRL GeoJSON-format.

</br>
</br>

### Uthenting av resultater av registrering og validering

Etter hvert som prosesseringen av de innsendte objektene blir ferdig kan resultatene hentes ut ved bruk at HTTP GET mot endepunkt `/responsemessages`. Samme endepunkt brukes for å hente ut resultat for registrering og validering av data for både CIM- og NRL GeoJSON-format.

Response på dette kallet er et array av objekter på følgende format

```
[
  {
    "messageId": "string",
    "objectId": "string",
    "succeeded": true,
    "message": "string"
  }
]
```

| Felt          | Beskrivelse   |
| ------------- | ------------- |
| messageId     | Meldingsid som ble returnert ved innsending av data |
| objectId      | Id til til aktuelt objekt i innsendt fil  |
| succeeded     | Flagg som indikerer om selve registrering/validering var vellykket.  |
| message       | En melding som beskriver resultatet av prosessering. Ved feil vil denne inneholder beskrivelse av feilen  |

Så ved en innsending av en melding med f.eks. 500 objekter vil man her få tilbake en response for hvert av objektene man sendte inn. Responsemeldingen vil inneholde resultat for hvert av objektene som er prosessert så langt, så avhengig av hvor mange objekter som skal prosesseres må man muligens gjøre flere kall til endepunktet før man får ut resultat for alle objektene man  sendte inn.


Eksempel på responsemelding hvor to av objektene har blitt registrert, mens et av objektene har en feil som må utbedres før man forsøker registrering på nytt:
```
[
  {
    "messageId": "1d09bf8b-221d-417b-9f1a-3963c62accb0",
    "objectId": "c2903580-2faa-48b1-8f12-a538f647d9dc",
    "succeeded": true,
    "message": "Sending of message to NRL succeeded!"
  },
  {
    "messageId": "1d09bf8b-221d-417b-9f1a-3963c62accb0",
    "objectId": "d5267c12-32fd-4444-a1ec-c4084af0abb9",
    "succeeded": true,
    "message": "Sending of message to NRL succeeded!"
  },
  {
    "messageId": "1d09bf8b-221d-417b-9f1a-3963c62accb0",
    "objectId": "83c6e3e8-e894-4409-84a3-1197ddd0e7ab",
    "succeeded": false,
    "message": "Fant ikke AssetDeployment.BaseVoltage for structureDeployment med ID c8ab4c82-d11f-49ef-93eb-5f11986d1e83"
  }
]
```
