# LACPass-LACChain-Component 

## Summary

The LACCPass-LACChain component enables Health Ministries or Health Organizations to manage their onboarding on the LACPass trust network, empowering them to issue and deliver health certificates to patients or individuals using digital wallets. 

This manual describes the steps to run the LACPass-LACChain component, and specifies how to use the endpoints.

### Requirements
- Internet access
- Make sure you have up and running and instance of the LACPass-LACChain component part of `IPS-national-backend` available at https://github.com/RACSEL/IPS-national-backend
- Access to the `client-helper.sh` executable script available at https://github.com/lacchain/IPS-national-backend/blob/master/lacchain-setup-helper/client-helper.sh
- [Postman](https://www.postman.com/) software

### Verify Service availability
1. Running the lacpass-lacchain component from `IPS-national-backend` will expose the service at port 3010.
2. Verify the lacpass-lacchain service is running either checking the logs or running a telnet command with the proper URL in a bash shell:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Configuring the service

Now run the CLI Main Menú (Client helper executable script) that will enable you to setup the decentralized identifier [DID](https://w3c.github.io/did-core) for your organization and set some keys to sign the Health certificates.

1. Make sure you have verified the service availability as described in the previous section [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability)
2. Before running the CLI make sure to execute this in a linux bash terminal:

```
$ chmod +x client-helper.sh
$ bash client-helper.sh
```
3. Now enter the URL of the lacpass-lacchain verified in the previous section [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) as shown in the following prompt:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/apiURL.png)

4. The CLI (Command Line Interface) Main Menu is presented:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLIMainMenu.png)


## Onboard health organizations setup process

Please follow these steps for the onboard setup of your Health organization in the LACPass trust network:

1. Create a decentralized identifier [DID](https://w3c.github.io/did-core) typing 'CD' in the CLI Main Menu:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLICreateDID.png) 

and a DID will be created and saved in a `did.txt` file: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didtxtfile.png)

2. Afterwards, a DID manager must be created, type 'CM' in the CLI Main Menu and enter the number of days in which the manager will be considered valid, for example: 1000 days. Do not enter a number less than 365 days.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingManager.png)

and a successful response will be displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didManagerCreation.png)

4. And, type 'exit' to end the onboard setup process.
 
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/exit.png)

After completing the onboard setup process, the following information will be in the `lacchain-setup-helper` directory:

- A `did.txt` file containing the decentralized identifier (DID) of your organization, have it handy in case you need to access the DID.

## Sharing the information for onboarding the LACPass trust network

Now you are ready to share the onboarding information with the committee, please follow these steps:

1. Start the CLI again.
2. Now enter the URL of the lacpass-lacchain service (as you did previously)
3. Type 'GCM' (Get Current Manager) **to fetch the entity and manager details** as shown:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/GCM.png)

Copy the content in a text file and name the file something like `Entity-Manager-Details.txt`.

4. Pack the following infromation in a zip file:

a) `Entity-Manager-Details.txt` file

b) Organization identifying information in a text file:

```
  i. Legal name
 ii. FHIR-URL
iii. Country/State code
``` 

5. Send the zip file via e-mail to epacheco@iadb.org and antoniole@iadb.org

## Sending Health certificates wrapped as Verifiable Credentials

In this section you will learn how to use the endpoint exposed by the lacpass-lacchain component to send health certificates as [verifiable credentials](https://www.w3.org/TR/vc-data-model/). If you successfully followed the previous steps, you are ready to send health certificates to your users. 

As explained in [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) section lacpass-lacchain runs on port 3010 by default. To send DDCCCoreDataSet health certificates you can use the [Postman](https://www.postman.com/) tool with the following parameters:

* Method: POST
* http://localhost:3010/api/v1/verifiable-credential/ddcc/send (update the host properly in case you are not accessing via localhost)
* The required payload to send has the following structure:

```javascript
 { 
  "bundle":
          {
            "entry": 
             ["string"]		
          },
   "issuerDid": "string",
   "receiverDid": "string"
 }
```

Where:
* **bundle:** DDCC FHIR Bundle, just copy and paste the full FHIR Bundle
* **IssuerDid:** Issuer DID, this is the decentralized identifier you created in the section Running Setup/Onboard steps in step 6 and that is available in the lacchain-setup-helper/did.txt file
* **receiverDid:** This is the Receiver DID (the patient/individual receiving the certificate) will share with you to receive the issued credential in their wallet. Patients/individuals can easily get their unique decentralized identifier (DID) after setting up their digital wallet available at https://lacpass-openprotest-wallet.lacchain.net/

**NOTE:** A full example with the required payload is available at https://github.com/lacchain/LACPass-client/blob/master/docs/Credential-Sending.md

## Setting up the digital wallet for patients/individuals

### Requirements

- Internet access
- Web browser (Chrome, Firefox, Opera) desktop or mobile
- Access to the digital wallet setup available at https://lacpass-openprotest-wallet.lacchain.net/

Please follow this steps to setup your LACPass digital wallet and receive your health certificates.

1. On your first access to [LACPass digital wallet](https://lacpass-openprotest-wallet.lacchain.net/) you will  be presented with a screen like this:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletLanding.png)

(In case you already registered you may enter your email and password as the wallet credentials to login).

2. If not registered, please click on the [register](https://lacpass-openprotest-wallet.lacchain.net/register) link that takes you to Create new account (registration) form:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletRegistration.png)

3. Please enter the following **User information: first name, family name, last name** and **Account information: email and password**. 

**Note:** After completing the registration information and clicking on `Create account` all account data will be encrypted in the browser's local storage.

After clicking Create account the following operations are displayed as shown:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingNewAccountDID.png)

a. Generating a new DID

b. Registering Public Keys

c. Changing Controller

d. Signing LACChain ID Credential

e. Auto-issuing the LACChain ID Credential in the wallet

4. Once the user wallet is setup, the following LACChain ID Credential is displayed with the information registered by the user:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletUserLACChainIDHome.png)

5. Click on the LACChain ID Credential:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredential.png)

6. And the following LACChain ID Credential information is presented as `Claims`: **Id, givenName, familyName, and email**. 

- The **Id** is the (patient/individual) wallet user decentralized identifier [DID](https://w3c.github.io/did-core), as seen in this example the DID value is: 
`did:lac1:1iT5kyRRuZHYjW1HJfGyNgGbCcZqhuDJLco5N4PemXBjkFSzkgP3YdLRd5BHHGT7LccZ`

- The DID value of your digital wallet must be shared with your Health organization, in order to issue and deliver the health credentials to your digital wallet. 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDID.png)

7. Now, please click on the upper right button (with the blue oval) that displays the wallet user name:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightButton.png)

8. The following drop-down menu is presented on the upper right side: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightMenu.png)

- Below the user name (which is blurred in the image) is the DID value, which can also be copied clicking on the two blue buttons on the right side.

- The DID value of your digital wallet must be shared with your Health organization, in order to issue and deliver the health credentials to your digital wallet. 

- To synchronize your digital wallet to receive issued credentials click the **Sync** button as displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightMenuSync.png)

9. Once you click on the **Sync** button the digital wallet will retrieve the health credentials issued to your DID. As displayed the digital wallet now contains besides the LACChain ID Credential, a Vaccination Certificate.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialVaccinationCertificate.png)

10. Please click on the Vaccination Certificate.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialVaccinationCertificateClick.png)

11. The following information of the Vaccination Certificate is presented. **Note:** the health certificate is a LACPass-WHO-Vaccination Credential.

On the left hand is the printed version of the Digital Vaccination Certificate with all related data and its QR code to scan the Vaccination Certificate.

On the right hand under `LACPass-WHO-Vaccination Credential` there are three options of the Vaccination Certificate available to click on:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassVaccinationCertificateInformation.png)

a) `Proofs` indicates the DID of the Ministry of Health - demo- LACPass who issued the Vaccination Certificate is a valid issuer.

b) `Root of Trust` displays the Root of Trust from the Issuer of the Vaccination Certificate (Ministry of Health - demo- LACPass) up to the chain root, in this case RACSEL.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassVaccinationCertificateRootOfTrust.png)

c) `Raw` displays the raw vaccination certificate data in a json structure. 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassVaccinationCertificateRaw.png)


## LACPass Verifier

LACPass Verifier is the last component used to verify DDCC-compliant health certificates. This component is made up of two subcomponents:

1. LACPass-front-verifier: This is a full front-end component that needs to be connected to LACPass-trusted-List to check the validity of health certificates. The repository is available at https://github.com/lacchain/LACPass-front-verifier
2. LACPass-trusted-list: This is the backend API component which cryptographically verifies certificate issuers and decodes data returning it alongside the certificate health validity. Access to this repository is available at https://github.com/lacchain/LACPass-trusted-list

**Note:** A fully runnning instance of LACPass Verifier can be found at https://lacpass.lacchain.net/

