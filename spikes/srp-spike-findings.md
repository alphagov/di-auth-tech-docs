# SRP spike findings

Date: 28/04/2021

## Requirements

* Not to save passwords or hashes in the database

## What we did

* Used the Nimbus SRP library to implement a server side SRP flow
* Refactored the user service to store SRP Salt and Verifier alongside the users email
* We attempted to implement SRP with Cognito but to no success

## Thoughts

1. Nimbus SRP library isn’t updated that frequently (Last updated November 2019)
2. The Server side implementation using the Nimbus library was straight forward
3. As we implemented this all server side it meant that it didn’t strictly fit SRPs goal of keeping the password purely on the client side
4. The documentation around the use of SRP with Cognito is not great and made it very challenging to implement although it is possible. Although we were trying to implement it all server side which again isn’t in the true spirit of SRP
5. The AWS java SDK examples of SRP and Cognito were out of date
6. The AWS javascript libraries do it all for us although we didn’t try and implement it