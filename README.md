# TypeQL_learning
Repository for recording TypeQL learning

# Documentation
Introduction: https://docs.vaticle.com/docs/general/introduction

Quickstart: https://docs.vaticle.com/docs/general/quickstart

# Install and start the server
Instructions: https://docs.vaticle.com/docs/running-typedb/install-and-run#system-requirements

To start the server directly in the unzipped folder, `cd` into the unzipped folder and run:
```
.\typedb.bat server
```

Once the server is running, open a console in another cmd window in the same path, using the command:
```
.\typedb.bat console
```

# Console
## Quickstart
See supporting files in [Quickstart/](Quickstart/)

To create the database and define the schema, follow these steps in the console:
```
> database create social_network
> transaction social_network schema write
social_network::schema::write> source C:\<path>\Quickstart\schema.tql
social_network::schema::write*> commit
```
To add the data to the database, follow the steps:
```
> transaction social_network data write
social_network::data::write> source C:\<path>\Quickstart\data.tql
social_network::data::write*> commit
```
To query the data:
```
> transaction social_network data read
social_network::data::read> match $tra (traveler: $per) isa travel; (located: $tra, location: $loc) isa localisation; $loc has name "French Lick"; $per has full-name $fn; get $fn;
```

Output:
```
{ $fn "Miriam Morton" isa full-name; }
{ $fn "Solomon Tran" isa full-name; }
{ $fn "Julie Hutchinson" isa full-name; }
answers: 3, total (with concept details) duration: 239 ms
```

Exit:
```
social_network::data::read> exit
```

## Commands in the console
```
Welcome to TypeDB Console. You are now in TypeDB Wonderland!
Copyright (C) 2022 Vaticle

> help

database list                                                    List the databases on the server
database create <db>                                             Create a database with name <db> on the server
database delete <db>                                             Delete a database with name <db> on the server
database schema <db>                                             Print the schema of the database with name <db>
transaction <db> schema|data read|write [transaction-options]    Start a transaction to database <db> with schema or data session, with read or write transaction
transaction-options                                              Transaction options
--infer true|false                                               Enable or disable inference
--trace-inference true|false                                     Enable or disable inference tracing
--explain true|false                                             Enable or disable inference explanations
--parallel true|false                                            Enable or disable parallel query execution
--batch-size 1..[max int]                                        Set RPC answer batch size
--prefetch true|false                                            Enable or disable RPC answer prefetch
--session-idle-timeout 1..[max int]                              Kill idle session timeout (ms)
--transaction-timeout 1..[max int]                               Kill transaction timeout (ms)
--schema-lock-acquire-timeout 1..[max int]                       Acquire exclusive schema session timeout (ms)
help                                                             Print this help menu
clear                                                            Clear console screen
exit                                                             Exit console
```

# TypeDB Studio
Donwload and installation: https://docs.vaticle.com/docs/studio/overview

To connect, enter `localhost:1729`.

Quickstart: https://docs.vaticle.com/docs/studio/quickstart


# TypeQL Language
Read here: https://docs.vaticle.com/docs/schema/overview

# TypeDB vs Semantic Web
https://docs.vaticle.com/docs/comparisons/semantic-web-and-typedb

Summarizing, the differences are:
1. TypeDB is less complex than Semantic Web, as Semantic Web  encompasses different standards (RDF, RDFS, OWL, SACHL, SPARQL). Both maintain a high degree of expressivity. The barrier to entry to TypeDB is, therefore, lower.
2. RDF models the data in triples, at a low data level. TypeDB includes an entity-relation concept level schema, creating higher order relationships and higher level of abstraction. Therefore, TypeDB makes it easier to work with complex data.
3. Semantic Web Standards are designed for linked data on an open web. TypeDB is designed to work in closed world systems with private data, like a traditional db.

Conclusions about TypeDB: it facilitates both the integration of the data and the access (query), as well as being able to explore the hierarchy. In addition, new ontologies and reasoning rules can be added in a simplified way. It can be a useful tool for the future of bioinformatics and the integration of different ontologies.

# TypeDB usage in Life Sciences
| Company | Description | Contact | URL |
| ------- | ----------- | ------- | --- |
| Astrazeneca | Social Graphs for Drug Development | Paul Agapow | https://www.youtube.com/watch?v=9yU8aLfJ9bM& |
| Vaticle | Enabling the Computational Future of Biology | Tomás Sabat | https://www.youtube.com/watch?v=XJDr_prOp9g&list=PLtEF8_xCPklY3P5NLSQb1SyIYLhQssxfY&index=2 |
| Astrazeneca | Disease ontologies for knowledge graphs | Natalja Kurbatova | https://github.com/natacourby/Disease_ontologies_for_knowledge_graphs |
| Roche | BioGrakn Covid: a Biomedical Knowledge Graph | Konrad Myśliwiec | https://www.youtube.com/watch?v=e-3BITuDgu8&list=PLtEF8_xCPklY3P5NLSQb1SyIYLhQssxfY&index=24 |

I am currently trying to replicate the Astrazeneca "Disease ontologies for knowledge graphs" article. However, the schema file schema.gql is no longer compatible with TypeDB (formerly named Grakn). Therefore, I am rewritting the schema in the file [schema_rewritten.tql](LifeSciences/schema_rewritten.tql) to solve the compatibility issue. Currently, there are some errors in the rules `preferred-compound-id-chembl-id-rule`, `preferred-compound-id-pubchem-id-rule`, `determine-best-mesh-id-1` and `determine-best-mesh-id-2`. The error message is:
```
## Error> [TQL32] TypeQL Error: Rule 'preferred-compound-id-chembl-id-rule' 'then' '$c has preferred-compound-id $cid' tries to assign type 'preferred-compound-id' to variable 'cid', but this variable already had a type assigned by the rule 'when'. Try omitting this type assignment.
```
