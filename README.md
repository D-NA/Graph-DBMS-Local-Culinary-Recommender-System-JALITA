# Graph-DBMS-Local-Culinary-Recommender-System-JALITA
Rancang bangun Graph DBMS untuk sistem rekomendasi kuliner lokal dengan nama aplikasi JALITA

# Query untuk load data CSV
LOAD CSV WITH HEADERS FROM "file:///'LOKASI DIRECTORY'" AS row
CREATE (n:'NAMA NODE')
SET n = row
