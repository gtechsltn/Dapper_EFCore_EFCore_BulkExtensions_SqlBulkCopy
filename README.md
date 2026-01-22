# Dapper vs EFCore vs EFCore.BulkExtensions vs SqlBulkCopy

| Method | Size | Speed |
| --- | --- | --- |
| EF_OneByOne | 100 | 19.800 ms |
| EF_OneByOne | 1000 | 259.870 ms |
| EF_OneByOne | 10000 | 8,860.790 ms |
| EF_OneByOne | 100000 | N/A |
| EF_OneByOne | 1000000 | N/A |
| Dapper_Insert | 100 | 10.650 ms |
| Dapper_Insert | 1000 | 113.137 ms |
| Dapper_Insert | 10000 | 1,027.979 ms |
| Dapper_Insert | 100000 | 10,916.628 ms |
| Dapper_Insert | 1000000 | 109,064.815 ms |
| EF_AddAll | 100 | 2.064 ms |
| EF_AddAll | 1000 | 17.906 ms |
| EF_AddAll | 10000 | 202.975 ms |
| EF_AddAll | 100000 | 2,129.370 ms |
| EF_AddAll | 1000000 | 21,557.136 ms |
| EF_AddRange | 100 | 2.035 ms |
| EF_AddRange | 1000 | 17.857 ms |
| EF_AddRange | 10000 | 204.029 ms |
| EF_AddRange | 100000 | 2,111.106 ms |
| EF_AddRange | 1000000 | 21,605.668 ms |
| BulkExtensions | 100 | 1.922 ms |
| BulkExtensions | 1000 | 7.943 ms |
| BulkExtensions | 10000 | 76.406 ms |
| BulkExtensions | 100000 | 742.325 ms |
| BulkExtensions | 1000000 | 8,333.950 ms |
| BulkCopy | 100 | 1.721 ms |
| BulkCopy | 1000 | 7.380 ms |
| BulkCopy | 10000 | 68.364 ms |
| BulkCopy | 100000 | 646.219 ms |
| BulkCopy | 1000000 | 7,339.298 ms |
