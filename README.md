# Graph-DBMS-Local-Culinary-Recommender-System-JALITA
Rancang bangun Graph DBMS untuk sistem rekomendasi kuliner lokal dengan nama aplikasi JALITA

## Query untuk load data CSV
```
LOAD CSV WITH HEADERS FROM "file:///'LOKASI DIRECTORY'" AS row
CREATE (n:'NAMA NODE')
SET n = row
```

## Query untuk membuat relationships
```
MATCH ('n1':'NAMA NODE'),('n2':'NAMA NODE')
WHERE 'n1'.'NAMA PROPERTIES YANG UNIK' = 'n2'.'NAMA PROPERTIES YANG UNIK'
CREATE ('n1')-[:'NAMA RELATIONSHIPS']->('n2')
```
> *hilangkan tanda apostrophe ('')

## Query untuk menemukan produk paling populer di dataset
```
MATCH (c:Customer)-[:PURCHASED]->(o:Order)-[:CONTAINS]->(m:Menu)
RETURN "c.companyName, p.productName, count(o) AS orders"
ORDER BY orders DESC
```
- LIMIT 5

## Query untuk memberikan rekomendasi menggunakan pendekatan Content-Based
> Berdasarkan transaksi pembelian mereka sebelumnya, apakah kita bisa merekomendasikan menu yang belum mereka beli? Oleh karena itu untuk setiap produk yang dibeli pelanggan kita, mari kita lihat apa yang juga dibeli oleh pelanggan lain. Setiap :Menu terkait dengan :Category sehingga kita dapat menggunakannya untuk lebih mempersempit daftar produk yang akan direkomendasikan.

```
MATCH (c:Customer)-[:PURCHASED]->(o:Order)-[:PRODUCT]->(p:Product)
<-[:PRODUCT]-(o2:Order)-[:PRODUCT]->(p2:Product)-[:PART_OF]->(:Category)<-[:PART_OF]-(p)
WHERE c.customerID = 'ANTON' and NOT( (c)-[:PURCHASED]->(:Order)-[:PRODUCT]->(p2) )
RETURN c.companyName, p.productName AS has_purchased, p2.productName AS has_also_purchased, count(DISTINCT o2) AS occurrences
ORDER BY occurrences DESC
LIMIT 5
```

> *Beberapa query disadur dari https://neo4j.com/graphgist/northwind-recommendation-engine
