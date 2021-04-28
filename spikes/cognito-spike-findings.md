# Cognito spike findings

Date: 28/04/2021

## Requirements

* Must be able to build our own UI
* Hand-off ownership of credentials to a third party (de-risk)
* Customisation points available

## What we did

* Set Up a Cognito UserPool with a single App Client
* Implemented:
    * User Sign-up
    * Account verification using code sent by email
    * User Sign-in
    * Integrated with both the di-auth-oidc-provider and di-auth-stub-relying-party party applications to create a working UI application

## Thoughts

1. SDK ok and api documentation was good and usable
2. Easy to set up in the AWS Console (there will be Terraform resource)
3. The CognitoIdentity bit is not useful for us
4. Will we have to pay for lots of stuff that we won’t use?
    * Will we have to pay for lots of stuff that we won’t use?
5. Easy to set up and get email verification working. Though OOTB template configuration is basic and we couldn’t do exactly what we wanted without creating a Lambda
6. Quite easy to get setup and working.  Had login flow built within 1 ½ days
7. Using it as a credential-store only (not using the built-in screens) is a valid way to use Cognito
8. Difficulty understanding whether a separate IAM user was required to call the api
9. PaaS
    * Cognito is not an existing PaaS service
    * This means specific config required (VPC Peering or Endpoint) and PaaS would need to build this for us (if we wanted PaaS + Cognito in the same AWS account)
    * It will still be possible to use Cognito, it just means that traffic to Cognito would go over the internet
    * Could look to lockdown our Cognito instance so that it can only be called by our applications alone
    * We may have to build a PaaS Cognito Service Broker