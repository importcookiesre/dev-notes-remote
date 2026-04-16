
> [!summary] **Data Models**
> It is the way how you represent the data and the relationship between data in a system.
> Different data models have different structures of representation and query which reduces the complexity of data. Ex. relational, document, graph, vector models.

1. sql vs nosql
2. relational vs document (json)
3. Object relational mismatch :
4. what are ORM and what are the problems with ORM.
5. normalization , denormalization and joins in relational and document

> [!PDF|yellow] [[Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk).pdf#page=98&selection=0,24,3,16&color=yellow|Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk), p.74]]
> > normalized data is usually faster to write (since there is only one copy) but slower to query (since it requires joins); denormalized data is usually faster to read (fewer joins) but more expensive to write (more copies to update, more disk space used)


> [!PDF|note] [[Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk).pdf#page=98&selection=12,37,15,9&color=note|Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk), p.74]]
> > OLTP systems, where both reads and updates need to be fast; analytical systems often fare better with denormalized data, since they perform updates in bulk and the performance of read-only queries is the dominant concern. 


> [!PDF|yellow] [[Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk).pdf#page=103&selection=43,2,43,27&color=yellow|Designing Data-Intensive Applications, 2nd Edition (Martin Kleppmann, Chris Riccomini) (z-library.sk, 1lib.sk, z-lib.sk), p.79]]
> > star or snowflake schema 
> 
> star vs snowflake schema

