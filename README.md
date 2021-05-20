# Graph-DBMS-Local-Culinary-Recommender-System-JALITA
Rancang bangun Graph DBMS untuk sistem rekomendasi kuliner lokal dengan nama aplikasi JALITA

## Query untuk load data CSV
- LOAD CSV WITH HEADERS FROM "file:///'LOKASI DIRECTORY'" AS row
- CREATE (n:'NAMA NODE')
- SET n = row

## Query untuk membuat data relationships
- MATCH ('n1':'NAMA NODE'),('n2':'NAMA NODE')
- WHERE 'n1'.'NAMA PROPERTIES YANG UNIK' = 'n2'.'NAMA PROPERTIES YANG UNIK'
- CREATE ('n1')-[:'NAMA RELATIONSHIPS']->('n2')
> *hilangkan tanda apostrophe ('')
