Using time curl http://localhost:3000/tools/sync_products 
WITH DATA NO INDEX
real	2m0.031s
user	0m0.005s
sys	0m0.007s

Using initializeOrderedBulkOp
WITH DATA NO INDEX
real	1m11.404s
user	0m2.150s
sys	0m0.460s

using initializeUnorderedBulkOp
WITH DATA NO INDEX
real	0m37.688s
user	0m1.840s
sys	0m0.190s


Using time curl http://localhost:3000/tools/sync_products 
WITH NO DATA NO INDEX
real	2m0.037s


using initializeUnorderedBulkOp
WITH DATA WITH INDEX
real	0m36.811s
user	0m1.740s
sys	0m0.230s


WITH DATA WITH INDEX
real	0m8.389s
user	0m1.800s
sys	0m0.170s


db.funder_product.dropIndexes();
db.funder_product.createIndex({fleats_id: 1});
db.funder_product.reIndex();
db.funder.dropIndexes();
db.funder.createIndex({fleats_id: 1});
db.funder.reIndex();

