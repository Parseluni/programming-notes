## Schemas ##  

GraphQL schemas are used to describe data and present our objects globally as a graph structure, so we need to show which type of object will be shown in the graph. It is the core of any GraphQL server implementation, and defines the functionality available to the client applications that consumes the API. GraphQL has its own language (GraphQL Schema Definition Language) that is used to write the schema. The schema determines what resources the clients can query and update.

**Query-driven schema design**   
A GraphQL schema is most powerful when it's designed for the needs of the clients that will execute operations against it. Although you can structure your types so they match the structure of your back-end data stores, you don't have to! A single object type's fields can be populated with data from any number of different sources. Design your schema based on how data is used, not based on how it's stored.

If your data store includes a field or relationship that your clients don't need yet, omit it from your schema. It's easier and safer to add a new field to a schema than it is to remove an existing field that some of your clients are using.

## Mutations ##

Mutations are used to create, update or delete records from the database. Mutations will help us to be able to add books or users to our Book Store API. 
