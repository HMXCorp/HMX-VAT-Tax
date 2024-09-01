1. Example Syntax to Add a Excel Record to the Control Table

```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "fileName": "%.xlsx", "folderPath": "%/%", "container": "landing" }'
--SourceConnectionSettingsName
,''
--CopySourceSettings
,'[{ "SheetName": "Sheet1", "HeaderRow": "1"}, { "SheetName": "Sheet2", "HeaderRow": "1"}]'
--SinkObjectSettings
,'{ "fileName": null, "folderPath": null, "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_2_Process_Landed_Files_Step2'
--TriggerName
,'TR_blobCreatedEvent'
--DataLoadingBehaviorSettings
,'{ "dataLoadingBehavior": "Extract_Excel_Sheets" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```

2. Example Syntax to Add a CSV Record to the Control Table
    - Allowed values for compression for *None*, *bzip2*, *gzip*, *deflate*
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "fileName": "%.csv", "folderPath": "%/%", "container": "landing" }'
--SourceConnectionSettingsName
,''
--CopySourceSettings
--Allowed values for compression for "None", "bzip2", "gzip", "deflate"
,'{ "fileType": "delimitedText", "delimiter": ",", "compression": "None"}'
--SinkObjectSettings
,'{ "fileName": null, "folderPath": "Department/Datasource/Dataset/", "container": "raw" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_2_Process_Landed_Files_Step2'
--TriggerName
,'TR_blobCreatedEvent'
--DataLoadingBehaviorSettings
,'{ "dataLoadingBehavior": "Copy_to_Raw", "loadType": "full" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```

3. Example Syntax to Add a Parquet Record to the Control Table
    - Leave compression as blank

```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "fileName": "%.parquet", "folderPath": "%/%", "container": "landing" }'
--SourceConnectionSettingsName
,''
--CopySourceSettings
--Leave compression as blank
,'{ "fileType": "parquet", "compression": ""}'
--SinkObjectSettings
,'{ "fileName": null, "folderPath": "Department/Datasource/Dataset/", "container": "raw" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_2_Process_Landed_Files_Step2'
--TriggerName
,'TR_blobCreatedEvent'
--DataLoadingBehaviorSettings
,'{ "dataLoadingBehavior": "Copy_to_Raw", "loadType": "delta" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```

4. Example Syntax to Add a JSON Record to the Control Table
    - Allowed values for compression for *None*, *bzip2*, *gzip*, *deflate*
    - Set multiline to true if a single json record spans multiple lines in the document

```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "fileName": "%.json", "folderPath": "%/%", "container": "landing" }'
--SourceConnectionSettingsName
,''
--CopySourceSettings
--Allowed values for compression for "None", "bzip2", "gzip", "deflate"
--Set multiline to true if a single json record spans multiple lines in the document
,'{ "fileType": "json", "multiline": false, "compression": "None"}'
--SinkObjectSettings
,'{ "fileName": null, "folderPath": "Department/Datasource/Dataset/", "container": "raw" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_2_Process_Landed_Files_Step2'
--TriggerName
,'TR_blobCreatedEvent'
--DataLoadingBehaviorSettings
,'{ "dataLoadingBehavior": "Copy_to_Raw", "loadType": "delta" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```

5. Example Syntax to Add a PDF/Image Record to the Control Table for later form recognizer extraction
    - [Form Recognizer Pre-Built Models](https://learn.microsoft.com/en-us/azure/applied-ai-services/form-recognizer/concept-model-overview?view=form-recog-3.0.0#model-data-extraction)

```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "fileName": "%.PDF/JPEG/JPG/PNG/BMP/TIFF", "folderPath": "%/%", "container": "landing" }'
--SourceConnectionSettingsName
,''
--CopySourceSettings
--model examples include: 'prebuilt-invoice', 'prebuilt-receipt', 'prebuilt-tax.us.w2', 'prebuilt-idDocument', 'prebuilt-businessCard'
,'{ "model": "prebuilt-invoice" }'
--SinkObjectSettings
,'{ "fileName": null, "folderPath": null, "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_2_Process_Landed_Files_Step2'
--TriggerName
,'TR_blobCreatedEvent'
--DataLoadingBehaviorSettings
,'{ "dataLoadingBehavior": "Form_Recognizer_Extraction" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```

6. Example Syntax to Add Oracle Record to the Control Table for Source to Landing in case of
    - Delta Load + watermark column data type: DateTime + No Partition in source table

   Values enclosed in [] to be replaced with actual values as per source setup like "schema": "[INSERT SCHEMA NAME]" can be as "schema": "dbo"
    
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "schema": "[INSERT SCHEMA NAME]", "table": "[INSERT TABLE NAME]", "query": "SELECT * FROM [INSERT SCHEMA NAME].[INSERT TABLE NAME] WHERE [INSERT WATERMARK COLUMN] > TO_TIMESTAMP(''<WATERMARKVALUE>'', ''YYYY-MM-DD HH24:MI:SS.FF'')" }'
--SourceConnectionSettingsName
,'{ "keyVaultSecretName": "[INSERT KV SECRET NAME]" }'
--CopySourceSettings
,'{ "watermark_column": "[INSERT WATERMARK COLUMN NAME]", "watermark_column_data_type": "Datetime", "watermarkValue": "1900-01-01 00:00:00", "partitioningOption": "None"}'
--SinkObjectSettings
,'{ "fileName": "[INSERT CUSTOM FILE NAME PATTERN]_YYYYMMDDHHMMSS.parquet", "folderPath": "[INSERT CUSTOM FOLDER NAME PATTERN]", "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_1_Source_to_Landing_Step1'
--TriggerName
,'[INSERT TRIGGER NAME]'
--DataLoadingBehaviorSettings
,'{ "ingestionPattern": "Oracle", "loadType": "delta" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```


7. Example Syntax to Add Oracle Record to the Control Table for Source to Landing in case of
    - Delta Load + watermark column data type: Integer + No Partition in source table
    
   Values enclosed in [] to be replaced with actual values as per source setup like "schema": "[INSERT SCHEMA NAME]" can be as "schema": "dbo"
    
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "schema": "[INSERT SCHEMA NAME]", "table": "[INSERT TABLE NAME]", "query": "SELECT * FROM [INSERT SCHEMA NAME].[INSERT TABLE NAME] WHERE [INSERT WATERMARK COLUMN] > <WATERMARKVALUE>" }'
--SourceConnectionSettingsName
,'{ "keyVaultSecretName": "[INSERT KV SECRET NAME]" }'
--CopySourceSettings
,'{ "watermark_column": "[INSERT WATERMARK COLUMN NAME]", "watermark_column_data_type": "Integer", "watermarkValue": "-1", "partitioningOption": "None"}'
--SinkObjectSettings
,'{ "fileName": "[INSERT CUSTOM FILE NAME PATTERN]_YYYYMMDDHHMMSS.parquet", "folderPath": "[INSERT CUSTOM FOLDER NAME PATTERN]", "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_1_Source_to_Landing_Step1'
--TriggerName
,'[INSERT TRIGGER NAME]'
--DataLoadingBehaviorSettings
,'{ "ingestionPattern": "Oracle", "loadType": "delta" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```


8. Example Syntax to Add Oracle Record to the Control Table for Source to Landing in case of
    - Full Load + No Partition in source table
    
   Values enclosed in [] to be replaced with actual values as per source setup like "schema": "[INSERT SCHEMA NAME]" can be as "schema": "dbo"
    
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "schema": "[INSERT SCHEMA NAME]", "table": "[INSERT TABLE NAME]", "query": "SELECT * FROM [INSERT SCHEMA NAME].[INSERT TABLE NAME]" }'
--SourceConnectionSettingsName
,'{ "keyVaultSecretName": "[INSERT KV SECRET NAME]" }'
--CopySourceSettings
,'{ "watermark_column": "", "watermark_column_data_type": "", "watermarkValue": "", "partitioningOption": "None"}'
--SinkObjectSettings
,'{ "fileName": "[INSERT CUSTOM FILE NAME PATTERN]_YYYYMMDDHHMMSS.parquet", "folderPath": "[INSERT CUSTOM FOLDER NAME PATTERN]", "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_1_Source_to_Landing_Step1'
--TriggerName
,'[INSERT TRIGGER NAME]'
--DataLoadingBehaviorSettings
,'{ "ingestionPattern": "Oracle", "loadType": "full" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```



9. Example Syntax to Add Oracle Record to the Control Table for Source to Landing in case of
    - Delta Load + watermark column data type: DateTime + Physical Partition in source table
    
   Values enclosed in [] to be replaced with actual values as per source setup like "schema": "[INSERT SCHEMA NAME]" can be as "schema": "dbo"
    
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "schema": "[INSERT SCHEMA NAME]", "table": "[INSERT TABLE NAME]", "query": "SELECT * FROM [INSERT SCHEMA NAME].[INSERT TABLE NAME] WHERE [INSERT WATERMARK COLUMN] > TO_TIMESTAMP(''<WATERMARKVALUE>'', ''YYYY-MM-DD HH24:MI:SS.FF'')" }'
--SourceConnectionSettingsName
,'{ "keyVaultSecretName": "[INSERT KV SECRET NAME]" }'
--CopySourceSettings
,'{ "watermark_column": "[INSERT WATERMARK COLUMN NAME]", "watermark_column_data_type": "Datetime", "watermarkValue": "1900-01-01 00:00:00", "partitioningOption": "PhysicalPartitionsOfTable"}'
--SinkObjectSettings
,'{ "fileName": "[INSERT CUSTOM FILE NAME PATTERN]_YYYYMMDDHHMMSS.parquet", "folderPath": "[INSERT CUSTOM FOLDER NAME PATTERN]", "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_1_Source_to_Landing_Step1'
--TriggerName
,'[INSERT TRIGGER NAME]'
--DataLoadingBehaviorSettings
,'{ "ingestionPattern": "Oracle", "loadType": "delta" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```


10. Example Syntax to Add Oracle Record to the Control Table for Source to Landing in case of
    - Full Load + Physical Partition in source table
    
   Values enclosed in [] to be replaced with actual values as per source setup like "schema": "[INSERT SCHEMA NAME]" can be as "schema": "dbo"
    
```sql
INSERT INTO [dbo].[ControlTable]
VALUES (
--SourceObjectSettings
'{ "schema": "[INSERT SCHEMA NAME]", "table": "[INSERT TABLE NAME]", "query": "SELECT * FROM [INSERT SCHEMA NAME].[INSERT TABLE NAME]" }'
--SourceConnectionSettingsName
,'{ "keyVaultSecretName": "[INSERT KV SECRET NAME]" }'
--CopySourceSettings
,'{ "watermark_column": "", "watermark_column_data_type": "", "watermarkValue": "", "partitioningOption": "PhysicalPartitionsOfTable"}'
--SinkObjectSettings
,'{ "fileName": "[INSERT CUSTOM FILE NAME PATTERN]_YYYYMMDDHHMMSS.parquet", "folderPath": "[INSERT CUSTOM FOLDER NAME PATTERN]", "container": "landing" }'
--SinkConnectionSettingsName
,''
--CopySinkSettings
,''
--CopyActivitySettings
,''
--TopLevelPipelineName
,'PL_1_Source_to_Landing_Step1'
--TriggerName
,'[INSERT TRIGGER NAME]'
--DataLoadingBehaviorSettings
,'{ "ingestionPattern": "Oracle", "loadType": "full" }'
--TaskId
,0
--CopyEnabled
,1
--DataContract
,'{}'
--PurviewScanEnabled
,1
)
```


