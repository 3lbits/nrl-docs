# Testing
Det er ikke mulig å bruke ekte organisasjoner i testmiljøet. Derfor brukes fiktive fra Tenor Testdata.

## Organisasjoner
Test-organisasjonene i tabellen under er konfigurert opp til at Elbits kan sende inn data til NRL på vegne av dem.

| Orgnisasjonsnummer | Nåværende daglig leder | Brukes av |
| --------- | ----------- | ----------- |
| 310671304 | 27915994771 | Elbits/Lede |
| 314007735 | 25847198322 | Elbits      |
| 314121287 | 14826295792 | lnett       |
| 312753332 | 04834599991 | BKK         |
| 312822318 | 11897897174 | Glitre      |

## Se data i kart
1. Gå til https://nrl-test.kartverket.no/ og trykk Logg inn
2. Velg “TestID (Lag din egen testbruker)”
3. Fyll ut personnummer for daglig leder fra tabellen over for aktuelt selskap i “Personidentifikator (syntetisk)”. Trykk Enter eller “Autentiser”. NB! Det er nærliggende å trykke på nærmeste knapp (“Hent tilfeldig person”), men det overskriver personnummeret du nettopp fylte ut. Hvis du trykker denne må du fylle ut riktig personnummer igjen
4. Hvis det kommer spørsmål om tilgang, trykk Godta
5. Hvis dette bildet dukker opp, følg lenken til Altinn sitt testmiljø. Logg inn der på samme måte, gå tilbake til NRL og trykk Logg inn
![image](https://github.com/3lbits/nrl-docs/assets/143094107/cdc56453-0965-4b8c-b365-fb740b849e7b)
6. Velg å representere virksomheten. NB! Tenor testdata er "levende", så det er mulig at daglig leder endres. I så fall vil ikke virksomheten dukke opp. Se instruks under for å finne nåværende daglig leder.
7. Trykk Neste på spørsmål om kontaktinformasjon. Fyll ut en falsk epostadresse dersom siden krever det
8. Velg Virksomhetens hindre
![image](https://github.com/3lbits/nrl-docs/assets/143094107/11465894-4d25-4aaf-9191-648872264fd0)
9. Finn hindre i kartet eller søk i menyen til venstre 

## Finne daglig leder for organisasjon
Siden Tenor testdata er levende kan det hende at daglig leder endres (feks ved "død"). For å finne nåværende daglig leder:
1. Gå til https://testdata.skatteetaten.no/web/testnorge/soek/. Logg inn (med din ekte identitet, ikke testbruker)
2. Velg "Virksomhet" til venstre. Utvid "Enhetsregisteret og Foretaksregisteret" og fyll ut organisasjonsnummeret. Trykk Enter
3. Velg fanen Kildedata. Finn daglig leder under rollegrupper->roller. Knappen "Åpne kildedata" kan brukes for å gjøre det lettere å lete
<img width="1006" alt="image" src="https://github.com/3lbits/nrl-docs/assets/143094107/ac83eea6-bfeb-4ed7-8a36-3125de231088">
4. Meld gjerne fra om at deglig leder er endret, slik at denne siden kan oppdateres
