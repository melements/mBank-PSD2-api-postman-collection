[# About 

This is a simple Postman collection for mBank PSD2 API.
Collection, after small configurations, allows you to create a basic PIS, AIS and CAF requests in mBank PSD2 Sandbox environment.
]
# How to use

## Preparing the environment

1. Download collection mBank API `PSD2 PIS-AIS-CAF.postman_collection.json` and environment variable `bank-simulator-pl-corpo-sandbox.postman_environment.json`.
2. Import them into your Postman.
3. Open Postman editor and edit imported environment variables.
4. Edit tppJwsPrivateKey variable and replace:
   ```
    -----BEGIN PRIVATE KEY-----
    {INSERT PRIVATE KEY HERE}
    -----END PRIVATE KEY-----
   ```
    with the value of your JWS certificate (QSEALC) private key. 
5. Edit tppJwsCertUrl variable and replace `{INSERT HERE YOUR JWS CERTIFICATE URL}` with URL where you have stored your public JWS certificate. For our Sandbox, you can use the link provided by us in the registration e-mail.
6. Edit tppJwsCertFingerprint variable and replace `{INSERT HERE YOUR JWS CERTIFICATE FINGERPRINT}` with your JWS certificate fingerprint. 
    You can get this fingerprint using the following command: 
    ```
    openssl x509 -in [JWS CERTIFICATE FILE (PEM-FORMATTED)] -outform der | openssl dgst -sha256 -binary | openssl enc -base64 | sed 's/=//' | sed 's/+/-/g' |    sed 's/\//_/g'
    ```
7. Edit Client-ID variable and replace `{INSERT HERE YOUR CLIENT ID}` with Client-ID that you received after registration.
8. Edit TPP-ID variable and replace `{INSERT HERE YOUR TPP ID}`  with your TPP-ID that is correlated with your certificates.
9. Add your TLS certificates to Postman configuration and add the following hostname: `sandbox.api.mbank.pl` in Settings -> Certificates.
10. If you use test certificates please disable SSL certificate verification in Postman Settings. Go to Settings -> General -> SSL Certificate verification -> OFF.
11. If you want to see created JWS signatures containing a payload, please monitor Postman console.

## Creating requests

For more details on how to test our mBank PSD2 Sandbox, please visit the website: 
https://developer.api.mbank.pl/documentation/sandbox

To see the full description of our mBank PSD2 API please refer to the following website:
https://developer.api.mbank.pl/documentation/api

### PIS

The attached collection is prepared for Domestic payments initiation only. On this basis, you can create tax, EEA and Non-EEA transfers from PSU account.

#### Immediate transfer

1. Send `[DOMESTIC] Retrieve authorization URI`.
2. Open returned authorization URL in your web browser and give a consent.
3. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
4. Open `[DOMESTIC] Retrieve accessToken for payment initiation`, replace {{authorizationCode}} with value from step 3 and send the request.
5. Send `[DOMESTIC] Initiate domestic payment`.
6. Send `[DOMESTIC] Retrieve accessToken for payment status`.
7. Send `[DOMESTIC] Retrieve payment status`.

#### Recurring transfer + cancellation

1. Send `[DOMESTIC RECURRING] Retrieve authorization URI`.
2. Open returned authorization URL in your web browser and give a consent.
3. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
4. Open `[DOMESTIC RECURRING] Retrieve accessToken for payment initiation`, replace {{authorizationCode}} with value from step 3 and send the request.
5. Send `[DOMESTIC RECURRING] Initiate domestic payment`.
6. Send `[DOMESTIC RECURRING] Retrieve accessToken for payment status`.
7. Send `[DOMESTIC RECURRING] Retrieve payment status`.
8. Send `[CANCELLATION] Retrieve authorization URI for payment cancellation`.
9. Open returned authorization URL in your web browser and give a consent.
10. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
11. Open `[CANCELLATION] Retrieve accessToken for payment cancellation`, replace {{authorizationCode}} with value from step 10 and send the request.
12. Send `[CANCELLATION] Cancel payment`.
13. Send `[DOMESTIC RECURRING] Retrieve payment status`.

#### Payment bundle + cancellation

1. Send `[DOMESTIC BUNDLE] Retrieve authorization URI`.
2. Open returned authorization URL in your web browser and give a consent.
3. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
4. Open `[DOMESTIC BUNDLE] Retrieve accessToken for bundle payments inititation`, replace {{authorizationCode}} with value from step 3 and send the request.
5. Send `[DOMESTIC BUNDLE] Initiate domestic bundle payments`.
6. Send `[DOMESTIC BUNDLE] Retrieve accessToken for bundle payments status`.
7. Send `[DOMESTIC BUNDLE] Retrieve bundle payments status`.
8. Send `[CANCELLATION] Retrieve authorization URI for bundle payments cancellation`.
9. Open returned authorization URL in your web browser and give a consent.
10. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
11. Open `[CANCELLATION] Retrieve accessToken for bundle payments cancellation`, replace {{authorizationCode}} with value from step 10 and send the request.
12. Send `[CANCELLATION] Cancel bundle payments`.
13. Send `[DOMESTIC BUNDLE] Retrieve bundle payments status`.

#### Split payment

1. Send `[SPLIT PAYMENT] Retrieve authorization URI`.
2. Open returned authorization URL in your web browser and give a consent.
3. After redirection to https://tpp-redirect-uri.com copy the UUID value from `code` query parameter.
4. Open `[SPLIT PAYMENT] Retrieve accessToken for payment inititation`, replace {{authorizationCode}} with value from step 3 and send the request.
5. Send `[SPLIT PAYMENT] Initiate domestic payment`.
6. Send `[SPLIT PAYMENT] Retrieve accessToken for payment status`.
7. Send `[SPLIT PAYMENT] Retrieve payment status`.

### AIS

1. Send `Retrieve authorization URI`.
2. Open returned authorization URL in your web browser and give a consent.
3. After redirection to https://tpp-redirect-uri.com copy UUID value from `code` query parameter.
4. Open `Retrieve accessToken for account list retrieval`, replace {{authorizationCode}} with the value from step 3 and send request.
5. Send `Get account list`.
6. Send `Exchange token to AIS scope`.
7. Send `Get account details`.
8. Send `Get transaction details`.
9. Send `Get transaction list`.
10. Send `Delete consent` and try to send any requests from steps 7 to 9.

### CAF

1. Send `Confirmation of the Availability of Funds`

### AS

1. Send `[REGISTER] Register an app using jwks keys`

## See also

1. [jsrsasign](https://github.com/kjur/jsrsasign) by Kenji Urushima
2. [node-uuid](https://github.com/kelektiv/node-uuid) by Robert Kieffer, AJ ONeal, Christoph Tavan, Vincent Voyer, Roman Shtylman

