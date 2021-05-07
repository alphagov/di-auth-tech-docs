# DynamoDB Spike Findings

Date: 07/05/2021

## Requirements

https://trello.com/c/EAyKo7wA/48-spike-confidential-privacy-respecting-storage

* We will need a data store to store user information.
* Maintains the privacy of the individual, reduces risk/threat of data leakage.
* Support encryption in-transit and encryption at rest.

## What we did

https://github.com/alphagov/di-auth-oidc-provider/tree/EAyKo7wA/dynamo-db-spike

* Created a test DynamoDB instance with a single user_info table.
* Added stub data to the table manually using the AWS console.
* Added code to connect to DynamoDb and retrieve the stub data.
* Added code to add additional records to the data store.
* Tried both the [DynamoDBMapper](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/java-examples.html#java-example-dynamodb-mapper) and [DynamoDBEncrypter](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/java-examples.html#java-example-ddb-encryptor) approaches.
* Created an encryption key in KMS.
* Added code to sign and encrypt the record using the key in KMS using the [DynamoDB Encryption Client for Java](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/java-using.html).
* Integrated the code into the OP userinfo endpoint and the stub RP to prove that an encrypted user can be added to the data store and then read back.

## Conclusions

* Relatively easy to do using the examples provided by AWS and the [DynamoDB Encryption client](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/java-using.html).
* The DynamoDBMapper approach was easiest to use, but would not necessarily work for our use case (less easy to add dynamic attributes).  We chose to use the DynamoDBEncrypter approach instead.
* Doing partial updates of records proved to be more difficult to implement.
* It is not possible to search on encrypted fields, you can only search fields that are signed-only (user_id or email for example).

##  Questions

* Can we use a different encryption key per record?  So if a key is compromised an attacker would only have access to a single record.
* Need to understand more about exactly how KMS works.
* How easy is it to do key rotations?
* Is DynamoDB the right thing to use?  Is similar encryption available for other data stores (RDS for example)?  What are the benefits of using DynamoDB?