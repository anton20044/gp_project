После прокатки роли необходимо создать каталог minio, по пути nano $PXF_BASE/servers/minio

В каталог скопировать файл minio-site.xml (pxf-base/servers/minio/minio-site.xml)

Указать в нем URL и креды

После этого можно создавать внешние таблицы 

CREATE EXTERNAL TABLE ext_airports (
    airport_code varchar(3),
	airport_name varchar(40)
)
LOCATION ('pxf://postgres/airports?PROFILE=s3:csv&SERVER=minio&FILE_HEADER=IGNORE&S3_SELECT=ON'
)
FORMAT 'CSV' (DELIMITER ',');
