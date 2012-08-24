tCouchbaseOperation
===================

A Talend component which executes various operations to a Couchbase cluster.
It requires a connection to a Couchbase cluster predefined by a [tCouchbaseConnection](https://github.com/ijokarumawak/tCouchbaseConnection).
This component is compatible with both Couchbase Server 1.8 and 2.0.

Please also refer [Wiki](https://github.com/ijokarumawak/tCouchbaseOperation/wiki). There are example jobs using this component.

How To Use it
==============

Basic Settings
---------------
Fill out required information to connect to a Couchbase Server cluster :

- **Connection**: Set up [tCouchbaseConnection](https://github.com/ijokarumawak/tCouchbaseConnection) before using this component. You can choose a connection instance from this pulldown menu.

- **Operation**: Currently, this component supports operations bellow:
  - Get: Retrieve documents from the database
  - Set: Set documents in the database
  - Delete: Delete documents in the database

- **ID**: Specify the target document ID to operate. You can use a string literal to specify a single document using quotations like **"someDocument"**, or you can use the flow connection to pass the ID like **row3.id** . 

- **Expiration** (only available with Set) : Specify how long the document will be alive. 0 means forever. Please check [Couchbase Server manual](http://www.couchbase.com/docs/couchbase-sdk-java-1.1/couchbase-sdk-java-set-set.html) for further information.

- **Value** (only available with Set) : Specify the value to be stored in the database. Similar to the ID, you can use string literal like **"someValue"** or flow connection like **row3.value**. To store complex JSON documents, I recommend you to use tWriteJSONField and [tParseJSON](https://github.com/ijokarumawak/tParseJSON).

- **Check operation result** (only available with Set and Delete) : When it is checked (as default), this component waits until the operation result is returned from Couchbase Server. When it is unchecked, this component doesn't wait the result of the operation (***fire-and-forget style***), it will increase the performance when you need to load a large number of rows, however it doesn't know if the operation is completed successfully. The **result** column will always be **true** and the **message** column is null if it is not checked.

- **Schema**: You need to specify the output schema of this component. If you want to put the command result into the output rows, please add these columns:
  - id (String) : The document id which was manipulated by this component
  - value (Object) : The value of the document. It will be the stored content in the database when you execute **Get** command or it will be the same content when you perform a **Set**. 
  - result (boolean) : It represents whether the command was executed successfully (true) or not (false).
  - message (String) : It holds additional message when the execution failed.
