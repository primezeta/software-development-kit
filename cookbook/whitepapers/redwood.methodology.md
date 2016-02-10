The Redwood Methodology is an application architecture for building next-gen EMR applications using document-oriented databases and web technologies.  Unlike application architectures which focus on simply emulating companies like Facebook which host millions of concurrent users, the Redwood Methodology is more of a multi-tenant Wordpress model, focusing on managing billions and trillions of historical documents that must be systematically accessed and reviewed. The purpose of the following document is to provide a methodology and convention for developers to use that allows them to focus on clinical problems rather than technical ones.

The Redwood Methodology began with a simple diagram documenting the architecture of Meteor, an isomorphic application framework that uses JavaScript on the server, client, and database; and which happens to use a document-oriented database.

![Meteor Architecture](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/Meteor%20Architecture%20-%20Basic.jpg)

Besides the breakthrough of creating an application framework that uses JavaScript on both the server and client, Meteor does a particularly noteworthy thing, in that it provides a technology called 'minimongo' which acts as a light-weight, client-side replica set and datastore for webapps.  

![Collections & Models](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/CollectionsAndModels.PNG)

Traditionally, Replica Sets are part of production Mongo systems, and involve writing data to disk in triplicate.  

![Replica Set](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/ReplicaSet.PNG)

Some organizations clear HL7 logs after a month, to keep them within the terabyte range.  But others might wish to keep long-term backups.  Assuming an organization has obligations to store data for 7 years (21 years in the case of pediatric data), it's quite reasonable to assume that someday an organization will exceed its capacity to scale vertically with a single server, and will need to scale horizontally with a database cluster.  In such situations, not only do we write the data in triplicate, but we write it to multiple servers.

![Sharded Mongo Cluster](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/ShardedMongoCluster.PNG)

We can also represent this database architecture vertically, which will offer some benefits as we introduce the Redwood Methodology.  Compare the above data model with the following, which layers on some application-level CRUD components (Create, Read, Update, Delete, List).  We essentially define a Microservice as a convention by which data can be persisted between client, server, and database, has some user interface components, and can be consistently rendered across devices.  

![ActiveCardArchitecture](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/ActiveCardArchitecture.PNG)

This architecture is a 'big data' architecture, and defines a data warehousing architecture, rather than a database architecture.  Important features include that it a) has before/after hooks, which allow one Microservice to connect to another, and b) it avoids joins between collections, which keeps it performant.  When chained together, these form a cascading data pipeline, which is the basis for workflow design.  

![Cascading Data Pipeline](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/CascadingDataPipeline.PNG)

For example, the California Kids Cancer Comparison app uses 8 collections in the following architecture.  Three different user roles might touch such an application (clinical coordinator, data analyst, and patient).  And we would discuss this app as having upwards of 8 microservices associated with it.

![Redwood Methodology](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/RedwoodMethodology.PNG)]

Colloquially, this architecture has sometimes been referred to as the 'Redwood Methodology' or the 'Santa Cruz Methodology'.  

When asking about HL7 FHIR integration, the basic idea is that we need to normalize the collection schemas using internationally recognized ontologies agreed upon by the major EMR vendors.  Consider the above architecture, which was designed in isolation by researchers unfamiliar with clinical standards such as HL7.  They happily modeled their problem domain according to their own terminology and ontologies.  If we were to normalize the architecture using HL7 resources, we would generate the following architecture which is HL7 compliant.  Doing so allows us to generate an HL7 conformance statement.

![HL7 Architecture](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/Hl7FhirArchitecture.PNG)

It should also be noted that inbound/outbound HL7 requests are enabled through the use of HL7 logs, which provide the dev/ops data that clinical administrators require.  

![HL7 Inbound/Outbound](https://raw.githubusercontent.com/clinical-meteor/cookbook/master/images/whitepapers/redwood/Hl7InboundOutbound.PNG)
