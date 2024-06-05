# nrl-docs
NRL documentation

## nrl-uploader

The NRL-Uploader is a web application that the user can provide a spesific file and send it Kartverket

### How to use the application

If the input file is Trimble NIS Excel it will be converted to CIM and the CIM message will be stored if that is chosen. Then the CIM message will be sent to ElBits. ElBits will then send this message to NRL and return the validation message.
If the input file is NRL GeoJSON it will be stored if that is chosen. Then the NRL GeoJSON message will be sent to ElBits. ElBits will then send this message to NRL and return the validation message

**Mark: The validation response is async which means it might take some seconds before it shows in the Log**

#### How to upload a file
1. The user must spesify which input file it will provide by choosing input format. Options: Trimble NIS Excel, NRL GeoJSON
2. The user must choose a file that are spesified by the chosen format
3. The user must choose if the input data should be stored or not. The validation data will be stored anyway
4. The user must push the Upload button
5. The application will provide som status comments during the operation to tell what happens. It will also tell if something failed

#### How to use the Upload User Log

The application will create a log spesified to each user. This log will include som message data like TimeStamp and the the data that is sent to ElBits and the validation response from NRL. The data data is sent to ElBits will only be stored if the user have spesified that it should be stored. **Mark that you might have some issues linking the Excel file and the validation response if you have not provided the uuids (See Trimble NIS Excel data format documentation section)**

- The Log have 3 columns
  - TimeStamp (Sorting is based on this column)
  - Post Data: Link to the data you sent to ElBits (Will only be available if the user has spesified to store it)
  - Response Data: Link to the validation response data

### Trimble NIS Excel data format documentation

The Excel file is based on a Trimble export format with some addons. The Excel format will at this point in time only support NRL Mast and NRL Linje as objects. Comming soon: NRL Flate
The Excel file conists of 2 sheets that needs to be named correctly. You can provide only one of the two if needed or both.
Mark: Optional values can be empty, but the column still must exist

#### Sheet1: Mast
This will be converted to NRL Mast



#### Sheet2: Trase Element
This will be converted to NRL Linje

| ID  | Tabell ID | x1 | y1 | z1 | x2 | y2 | z2 | Lengde (m) | Betegnelse | Luftspenntype (NRL) | Luftfartshinderlyssetting (NRL) | Luftfartshindermerking (NRL) | Hoydereferanse (NRL) | Vertikalavstand meter (NRL) | Verifisert nøyaktighet (NRL) | Status (NRL) | ACLineSegmentSpan_uuid | ACLineSegmentSpanDeployment_uuid | Name_sourceId_uuid |
|-----|-----------|----|----|----|----|----|----|------------|------------|---------------------|---------------------------------|-----------------------------|----------------------|-----------------------------|-----------------------------|--------------|-------------------------|----------------------------------|---------------------|
| Internal id. Converts to Komponentkodeverdi. If you do not provide your own uuid this will be mapped to a generated uuid (Required) | Application spesific. Must be: 261 (Required) | x starting coordinate in UTM32 (Required) | y starting coordinate in UTM32 (Required) | z starting coordinate in UTM32 (Optional) | x ending coordinate in UTM32 (Required) | y ending coordinate in UTM32 (Required) | z ending coordinate in UTM32 (Optional) | Length of the line i meter (Required) | Name of the line (optional) | Options: Ledning, høyspent or Ledning, lavspent (Required) | Options: Lyssatt or Mellomintensitet, type A or Mellomintensitet, type B or Mellomintensitet, type C (Optional) | Options: Fargemerking or Markør (Optional) | Options: Topp (Optional) | The heighest distance from the ground to the line in meters | Options: FOR-2020-10-16-2068, §5(1) (Required) | Options: Eksisterende or Fjernet or Planlagt fjernet or Planlagt oppført (Required) | The main uuid for the line (Optional: A uuid will be created if not provided here) | The deployment uuid of the line (Optional: A uuid will be created if not provided here) | The Name uuid of the line (Optional: A uuid will be created if not provided here) |

### NRL GeoJSON data format documentation
