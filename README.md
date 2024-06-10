# nrl-docs
NRL documentation

## Oppsett i Altinn
Nettselskapet må delegere tilgang til scopet kartverk:nrl.rapportering til Elbits i Altinn. Dette må kun gjøres en gang.

### Delegering av scope
Fremgangsmåte:
1. Daglig leder (eller en person som daglig leder har deleger rollen “Programmeringsgrensesnitt (API)” til, se under) logger inn i Altinn. Velg å representere nettselskapet, ikke personen.
1. Velg “Profil” øverst.
1. Trykk “Tilgang til programmeringsgrensesnitt - API” og så “Gi og fjern API tilganger”
![image](https://github.com/3lbits/nrl-docs/assets/143094107/66d71683-6ba1-4c74-ad38-9e8510804410)
1. Velg “+ Deleger nytt API”
1. Søk opp “NRL Rapportering” fra Kartverket og trykk “+”
1. Trykk Neste
1. Søk opp Elbits (931264079) og trykk “+”
1. Trykk Neste
1. Bekreft

### Delegering av tilgangsstyring
Daglig leder kan delegere rettigheter til å gi API-tilganger. Dette er kanskje mest relevant hvis nettselskapet har flere API-tilganger som skal administreres. NB! Rollen er knyttet til personnummer og forsvinner ikke når arbeidsforholdet avsluttes. Daglig leder (eller noen andre med rettighet til å styre roller) må fjerne rollen manuelt.

Fremgangsmåte:
1. En person med tilgang til rollestyring (feks daglig leder) logger inn i Altinn. Velg å representere nettselskapet, ikke personen.
1. Velg “Profil” øverst.
1. Velg “Andre med tilgang til virksomheten” → “+ Legge til ny person eller virksomhet”
![image](https://github.com/3lbits/nrl-docs/assets/143094107/ae82eca7-6c9f-4ff0-9bb1-7f90248ebadd)
1. Ekspander “Har disse X rollene” og trykk “+Legg til ny rolle”
1. Trykk på “Programmeringsgrensesnitt (API)”
1. Bekreft
#### For å fjerne
1. Fra Profil → “Andre med tilgang til virksomheten”, velg “Gi eller fjern tilgang” på personen
1. Velg “Fjern en eller flere rettigheter”
![image](https://github.com/3lbits/nrl-docs/assets/143094107/f7191d64-90bb-4fc3-b038-578fdb3d6b15)
1. Fjern rollen og bekreft

## nrl-uploader

The NRL-Uploader is a web application that the user can provide a spesific file and send it Kartverket

### How to use the application

If the input file is Trimble NIS Excel it will be converted to CIM and the CIM message will be stored if that is chosen. Then the CIM message will be sent to ElBits. ElBits will then send this message to NRL and return the validation message.
If the input file is NRL GeoJSON it will be stored if that is chosen. Then the NRL GeoJSON message will be sent to ElBits. ElBits will then send this message to NRL and return the validation message

**Mark: The validation response is async which means it might take some seconds before it shows in the Log**

#### How to upload a file
1. The user must spesify which input file it will provide by choosing input format. Options: Trimble NIS Excel or NRL GeoJSON
2. The user must choose a file that are spesified by the chosen format
3. The user must choose if the input data should be stored or not. The validation data will be stored anyway
4. The user must push the Upload button
5. The application will provide some status comments during the operation to tell what happens. It will also tell if something failed

#### How to use the Upload User Log

The application will create a log spesified to each user. This log will include som message data like TimeStamp and the the data that is sent to ElBits and the validation response from NRL. The data sent to ElBits will only be stored if the user have spesified that it should be stored. **Mark that you might have some issues linking the Excel file and the validation response if you have not provided the uuids (See Trimble NIS Excel data format documentation section)**

- The Log have 3 columns
  - TimeStamp (Sorting is based on this column)
  - Post Data: Link to the data you sent to ElBits (Will only be available if the user has spesified to store it)
  - Response Data: Link to the validation response data

### Trimble NIS Excel data format documentation

The Excel file is based on a Trimble export format with some addons. The Excel format will at this point in time only support NRL Mast and NRL Linje as objects. Comming soon: NRL Flate.

The Excel file conists of 2 sheets that needs to be named correctly. You can provide only one of the two if needed or both.

**Marks:**
- **Optional values can be empty, but the column must exist**
- **For NRL Linje we will use Topp as Høydereferanse even if you have provided Fot. If Fot is provided we will convert the z coordinate by adding vertikalavstand**
- **Coordinate systems supported at this moment is EPSG::25832 (2 dimensions) and EPSG::5972 (3 dimensions). These are based on UTM32 with heigh above sea level for z (3 dimension)**

#### Sheet1: Mast
This will be converted to NRL Mast

| ID  | Tabell ID | x | y | z | Betegnelse | Masttype (NRL) | Hoydereferanse (NRL) | Luftfartshinderlyssetting (NRL) | Luftfartshindermerking (NRL) | Vertikalavstand meter (NRL) | Verifisert nøyaktighet (NRL) | Status (NRL) | OverheadStructure_uuid | StructureDeployment_uuid | Name_sourceId_uuid |
|-----|-----------|---|---|---|------------|----------------|----------------------|---------------------------------|-----------------------------|-----------------------------|-----------------------------|--------------|------------------------|--------------------------|---------------------|
| Internal id. Converts to Komponentkodeverdi. If you do not provide your own uuid this will be mapped to a generated uuid (Required) | Application spesific. Must be: 109 (Required) | x coordinate in UTM32 (Required) | y coordinate in UTM32 (Required) | z coordinate (Optional) | Name of the "Mast" (Required) | Options: Mast, høyspent or Mast, lavspent (Required) | Options: Topp or Fot (Required) | Options: Lyssatt or Mellomintensitet, type A or Mellomintensitet, type B or Mellomintensitet, type C (Optional) | Options: Fargemerking or Markør (Optional) | The heighest distance from the ground to the top of the Mast | Options: FOR-2020-10-16-2068, §5(1) (Required) | Options: Eksisterende or Fjernet or Planlagt fjernet or Planlagt oppført (Required) | The main uuid for the "Mast" (Optional: A uuid will be created if not provided here) | The deployment uuid of the "Mast" (Optional: A uuid will be created if not provided here) | The Name uuid of the "Mast" (Optional: A uuid will be created if not provided here) |

#### Sheet2: Trase Element
This will be converted to NRL Linje

| ID  | Tabell ID | x1 | y1 | z1 | x2 | y2 | z2 | Lengde (m) | Betegnelse | Luftspenntype (NRL) | Luftfartshinderlyssetting (NRL) | Luftfartshindermerking (NRL) | Hoydereferanse (NRL) | Vertikalavstand meter (NRL) | Verifisert nøyaktighet (NRL) | Status (NRL) | ACLineSegmentSpan_uuid | ACLineSegmentSpanDeployment_uuid | Name_sourceId_uuid |
|-----|-----------|----|----|----|----|----|----|------------|------------|---------------------|---------------------------------|-----------------------------|----------------------|-----------------------------|-----------------------------|--------------|-------------------------|----------------------------------|---------------------|
| Internal id. Converts to Komponentkodeverdi. If you do not provide your own uuid this will be mapped to a generated uuid (Required) | Application spesific. Must be: 261 (Required) | x starting coordinate in UTM32 (Required) | y starting coordinate in UTM32 (Required) | z starting coordinate (Optional) | x ending coordinate in UTM32 (Required) | y ending coordinate in UTM32 (Required) | z ending coordinate (Optional) | Length of the line i meter (Required) | Name of the line (Required) | Options: Ledning, høyspent or Ledning, lavspent (Required) | Options: Lyssatt or Mellomintensitet, type A or Mellomintensitet, type B or Mellomintensitet, type C (Optional) | Options: Fargemerking or Markør (Optional) | Options: Topp (Optional) | The heighest distance from the ground to the line in meters | Options: FOR-2020-10-16-2068, §5(1) (Required) | Options: Eksisterende or Fjernet or Planlagt fjernet or Planlagt oppført (Required) | The main uuid for the line (Optional: A uuid will be created if not provided here) | The deployment uuid of the line (Optional: A uuid will be created if not provided here) | The Name uuid of the line (Optional: A uuid will be created if not provided here) |

### NRL GeoJSON data format documentation

This format is spesified by Kartverket and NRL. See documentation here: [Link](https://nrl-test.kartverket.no/nrl/swagger-ui/index.html?urls.primaryName=ekstern)

If this format is chosen the application will not do any conversion at this moment in time
