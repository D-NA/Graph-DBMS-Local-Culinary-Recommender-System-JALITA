# Graph-DBMS-Local-Culinary-Recommender-System-JALITA
Rancang bangun Graph DBMS untuk sistem rekomendasi kuliner lokal dengan nama aplikasi JALITA

## Fundamental: Konsep graf dalam menyimpan data

- Node: Rekaman data graf
- Relationship: Untuk menyambungkan antar node (punya arah dan tipe hubungan)
- Property: Menyimoan data dalam pasangan key-value dalam node and relationship
- Label: Kelompok node and relationship (opsional)

## Query untuk load data CSV
```
LOAD CSV WITH HEADERS FROM "file:///'LOKASI DIRECTORY'/namafile.csv" AS row
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
LIMIT 5
```

## Query untuk memberikan rekomendasi menggunakan pendekatan Content-Based
> Berdasarkan transaksi pembelian mereka sebelumnya, apakah kita bisa merekomendasikan menu yang belum mereka beli? Oleh karena itu untuk setiap produk yang dibeli pelanggan kita, mari kita lihat apa yang juga dibeli oleh pelanggan lain. Setiap :Menu terkait dengan :Category sehingga kita dapat menggunakannya untuk lebih mempersempit daftar produk yang akan direkomendasikan.

```
MATCH (c:Customer)-[:PURCHASED]->(o:Order)-[:HAS_PRODUCER]->(r:Restoran)
<-[:HAS_PRODUCER]-(o2:Order)-[:CONTAINS]->(m2:Menu)-[:CATEGORY_OF]->(:Category)<-[:PART_OF]-(m)
WHERE c.customerID = 'C0001' and NOT( (c)-[:PURCHASED]->(:Order)-[:CONTAINS]->(m2) )
RETURN c.restoName, m.makananName AS has_purchased, m2.makananName AS has_also_purchased, count(DISTINCT o2) AS occurrences
ORDER BY occurrences DESC
LIMIT 5
```

## Query untuk melihat data history Customer
```
MATCH (n:Order) 
WHERE n.customerID = "C0001" 
RETURN n
```

## The Cypher to show cosine and Gower distance in graph database
```
match (n1:Product {pro_id:'Pro7'}) -[rel:GOWER]- (n2:Product 
{sub_districts:'Kelapa Gading'})-[r:COSINE]-(n1:Product)
return n1.pro_id as src, n2.pro_id as des, r.cosine_distance as cosine, 
rel.gower_distance as gower
```

> *Beberapa query disadur dari: 
- https://neo4j.com/graphgist/northwind-recommendation-engine
- https://gist.github.com/DaniSancas/1d5265fc159a95ff457b940fc5046887
