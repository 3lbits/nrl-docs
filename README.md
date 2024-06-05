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
