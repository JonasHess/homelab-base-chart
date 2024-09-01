# Setup OIDC Traefik-Forward-Auth with AWS Cognito

This guide will walk you through the process of setting up OIDC Traefik-Forward-Auth with AWS Cognito.

## Prerequisites

- AWS account with access to Cognito service.
- Domain names configured: `auth.your-domain.com`


## Steps

1. **Create a Cognito User Pool**

   Navigate to the AWS Cognito service and create a new User Pool. Take note of the User Pool ID, it will be used later.

2. **Create a Cognito App Client**

   Within the User Pool, create a new App Client. Make sure to AppType as  `Trusted Client`.
   Check the OAuth scopes and make sure to enable "Profile".

   Set the callback URL to `https://auth.your-domain.com/_oauth`. Take note of the App Client ID and Client Secret, it will be used later.

3. **Create a user in the User Pool**

   Create a new user in the User Pool.

4. **Configure OIDC in `values.yaml`**

   Update the `values.yaml` file with the following values:

   ```yaml
   traefik_forward_auth:
     oidc_issuer_url: "https://cognito-idp.<aws-region>.amazonaws.com/<user-pool-id>/.well-known/jwks.json"
     oidc_client_id: "<app-client-id>"
   ```

   Replace `<aws-region>` with your AWS region, `<user-pool-id>` with your User Pool ID, and `<app-client-id>` with your App Client ID.


5. **Set Client Secret**

   Create a Secret in Akeyless with the Client Secret value under the key `{{.Values.akeyless.path}}/oidc/traefik-forward-auth/client_secret`

