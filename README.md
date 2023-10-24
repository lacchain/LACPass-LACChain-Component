# LACPass-LACChain-Component 

## Summary

The LACCPass-LACChain component enables Health Ministries or Health Organizations to manage their onboarding on the LACPass trust network, empowering them to issue and deliver health certificates to patients or individuals. This manual describes the steps to run the LACPass-LACChain component, and specifies how to use the endpoints.

### Requirements
- Make sure you have up and running and instance of the LACPass Client component available at https://github.com/lacchain/IPS-national-backend
- Access to the client-helper executable script available at https://github.com/lacchain/IPS-national-backend#lacchain-setup-and-onboard-helper
- Internet access
- [Postman](https://www.postman.com/) software to interact with APIs

### Verify Service availability
1. Running the lacpass-lacchain component from IPS-national-backend will expose the service at port 3010
2. Verify the lacpass-lacchain component is running either checking the logs or just running a telnet command in a bash shell:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Running Setup

Now run the CLI (Client helper executable script) to setup your decentralized identifier [DID](https://w3c.github.io/did-core) and set some keys.
1. Make sure you have verified the service availability as described in the previous section [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability)
2. Before running the CLI make sure to execute this in a linux bash terminal:

`$ ./client-helper.sh`

3. Now enter the URL of the lacpass-lacchain verified in the previous section [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) as shown in the following prompt:


![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/apiURL.png)

4. The following menu is presented:


![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLIMainMenu.png)


## Health organizations onboard to the LACPass trust network 

To onboard your country's health organization in the LACPass trust network please verify if your health organization has a Public Key Infrastructure in place, and review the following options. 

### Countries without Public Key Infrastructure (PKI)
If the Health organization of your Country doesn't have a Public Key Infrastructure in place, you will be asked to create an X.509 self-signed certificate (SSC) as follows:

In the CLI Main Menu type 'SSC' and enter the requested information: 

1. The first step is entering the [Country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for the Health organization self-signed certificate:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509CountryCode.png)

2. Now you may enter the [State code](https://en.wikipedia.org/wiki/ISO_3166-2) from the selected Country (for example: https://en.wikipedia.org/wiki/ISO_3166-2:BR if the country were Brazil), or you may press enter to skip this step:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509StateCode.png)

3. Next, enter your Health organization name (for example: Ministry of Health of Peru Demo Brasil) as shown:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganization.png)

4. Next step, you will be asked to enter a common nane for your Health organization (for example: BrasilDemo_MoH), or you may press enter to skip this step:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganizationCommonName.png)

5. After completing these steps, the data specified for the self-signed certificate will be displayed asking you to confirm. **Note:** Keep in mind that if a valid Country code wasn't specified you will probably get an error.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509DataConfirmation.png)

6. Once confirmed, the self signed certificate creation with the data is displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509Creation.png)

7. A subdirectory named `/certs` is created inside the directory you are running the script, with two subdirectories Document Signer Certificates `/DSC` and Signing Certificate Authority `/SCA` as displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/certsDirSubdirs.png)

The Document Signer Certificates `/DSC` subdirectory contains three files:

a) `DSC.crt` is the X.509 certificate.

b) `DSC.csr` is the Certificate Signing Request contains the public key and common name required by a Certificate Authority.

c) `DSC.key` is the private key used to sign Health Certificates, if your Health organization will use this key (optional), please complete these steps:
- Copy the `DSC.key` file into the `cert-data` directory located in the root directory of your `IPS-national-backend` repository.
- Rename the new copy of `DSC.crt` to `priv.pem`

The Signing Certificate Authority `/SCA` subdirectory contains two files

a) `SCA.crt` is the X.509 certificate.

b) `SCA.key` is the Signing Certificate Authority private key.

Once completed these steps You have successfully created a self-signed certificate (SSC) for your Country's Health organization.

### Countries with Public Key Infrastructure (PKI)
If the Health organization of your Country has a Public Key Infrastructure in place, the onboarding process will require to use your existing X.509 certificate to complete the onboarding process. 

## Onboard health organizations setup process

Using your Country Health Organization PKI X.509 certificates, please follow this steps for the onboard setup of your Health organization in the LACPass trust network:

1. Create a decentralized identifier [DID](https://w3c.github.io/did-core) typing 'CD' in the CLI Main Menu:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLICreateDID.png) 

and a DID will be created and saved in a `did.txt` file: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didtxtfile.png)

2. The next step is to associate the DID created in the previous step with the X.509 Certificate that will be used to sign Health certificates, type 'AX' in the CLI Main Menu and enter the path where the X.509 certificate is located. (If you created a self-signed certficate the path would be as in step 7): 
    
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509path.png)

After entering the appropriate path you should get a successful message, inidcating the X.509 certificate was successfully associated with the DID:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didx509association.png)

**Note:** the DID must remain the same for the association with the X.509 certificate to be valid.

3. Afterwards, a DID manager must be created, type 'CM' in the CLI Main Menu and enter the number of days in which the manager will be considered valid, for example: 1000 days. Do not enter a number less than 365 days.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingManager.png)

and a successful response will be displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didManagerCreation.png)

4. And, type 'exit' to end the onboard setup process.
 
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/exit.png)

After completing the onboard setup process, the following information will be in the `lacchain-setup-helper` directory:

- A `did.txt` file containing the decentralized identifier (DID) of your organization, have it handy in case you need to access the DID.
- The `/certs` directory containing the `\DSC` and `\SCA` subdirectories.

## Sharing the information for onboarding the trust network

Now you are ready to share the onboarding information with the committee, please follow these steps:

1. Start the CLI again.
2. Type 'GCM' (Get Current Manager) **to fetch the entity and manager details** as shown:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/GCM.png)

Copy the content in a text file and name the file something like `Entity-Manager-Details.txt`.

3. Pack the following infromation in a zip file:

a) `Entity-Manager-Details.txt` file

b) The `SCA.crt` file, located in the directory `lacchain-setup-helper/certs/SCA/SCA.crt` 

c) Organization identifying information in a text file:

  i. Legal name
 ii. FHIR-URL
iii. Country/State code

4. Send the zip file via e-mail to epacheco@iadb.org and antoniole@iadb.org

## Sending Health certificates wrapped as Verifiable Credentials

In this section you will learn how to use the endpoint exposed by the lacpass-lacchain component to send health certificates as verifiable credentials. If you successfully followed the previous steps, you are ready to send health certificates to your users. 

As explained in [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) section lacpass-lacchain runs on port 3010 by default. To send DDCCCoreDataSet health certificates you can use the [Postman](https://www.postman.com/) tool with the following parameters:

* Method: POST
* http://localhost:3010/api/v1/verifiable-credential/ddcc/send (update the host properly in case you are not accessing via localhost)
* The required payload to send has the following structure:


Where:
* bundle: FHIR bundle, just copy and paste the full FHIR bundle
* IssuerDid: Issuer DID, this is the decentralized identifier you created in the section Running Setup/Onboard steps in step 6 and that is available in the lacchain-setup-helper/did.txt file
* receiverDid: This is the Receiver DID (the patient/individual receiving the certificate) will share with you to receive the issued credential in their wallet. Patients/individuals can easily get their unique identifier (DID) after setting up the wallet available at https://lacpass-openprotest-wallet.lacchain.net/

NOTE: A full example with the required payload is available at https://github.com/lacchain/LACPass-client/blob/master/docs/Credential-Sending.md

### LACPass Verifier

LACPass Verifier is the last component used to verify DDCC-compliant health certificates. This component is made up of two subcomponents:

1. LACPass-front-verifier: This is a full front-end component that needs to be connected to LACPass-trusted-List to check the validity of health certificates. The repository is available at https://github.com/lacchain/LACPass-front-verifier
2. LACPass-trusted-list: This is the backend API component which cryptographically verifies certificate issuers and decodes data returning it alongside the certificate health validity. Access to this repository is available at https://github.com/lacchain/LACPass-trusted-list

NOTE: A fully runnning instance of LACPass Verifier can be found at https://lacpass.lacchain.net/



















