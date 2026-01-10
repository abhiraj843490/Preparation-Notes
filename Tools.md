pentaho, flink-> data etl (data streaming)
aws baffel -> data sync

on s3 bucket lots of data are available in csv format, we need to transform it in json and encrypit the sensitive fields using baffel and transform it and pass to the down service to process the data and load the ui.


data-sync -> inbound -> outbound -> data export
             \                 /
        creating cfls(common file layout)



