# Web Identity Federation & Cognito

Allows you to grant users access to AWS resources **after** they have successfully authenticated with a web-based identity provider (e.g. Amazon, Facebook, Google, etc.). Any Open ID Connect provider can be used.

After successful authentication users are given an authentication code from the Web ID provider that can be exchanged for temporary AWS credentials.

Amazon Cognito is AWS' Web Identity Federation service and has the following features:
- Sign up/sign in to your apps
- Access for guest users
- Acts as an identity broker between your app and Web ID providers (so you don't have to write more code)
- Synchronizes user data for multiple devices
- Recommended for all mobile applications AWS services.

Temporary credentials map to an IAM role. 

User Pools are user directories to manage sign-up and sign-in functionality for applications. Users can sign in directly to the User Pool (e.g. stored credentials) or with an Web ID service (e.g. facebook, google, etc.). Successful authentication generates a JSON Web Token (JWT).

Identity Pools provide a **Cognito ID** and temporary AWS credentials to access AWS services like S3 or DynamoDB, the actual store for temporary AWS credentials.

![cognito](../img/Cognito%20Authentication.svg)

Cognito tracks the association between user identity and the devices they use. In order to provide a seamless user experience, Cognito uses SNS to synchronize user data across multiple devices. Whenever data in the cloud changes, SNS sends a notification to all devices.

