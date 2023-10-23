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


## Onboarding Health organizations 

To onboard your country's health organization in the LACPass-LACChain trust network please follow the following steps.

### Countries with Public Key Infrastructure (PKI)
If the Health organization of your Country has a Public Key Infrastructure in place, the onboarding steps will require to use your existing X.509 certificate to complete the onboarding process. 

### Countries without Public Key Infrastructure (PKI)
If the Health organization of your Country doesn't have a Public Key Infrastructure in place, you will be asked to create an X.509 self-signed certificate (SSC) as follows:

In the CLI Main Menu type 'SSC' and enter the requested information: 

1. The first step is entering the [Country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for the Health organization self-signed certificate:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509CountryCode.png)

2. Now you may enter the [State code](https://en.wikipedia.org/wiki/ISO_3166-2) from the selected Country (for example: https://en.wikipedia.org/wiki/ISO_3166-2:BR if the country were Brazil), or you may press enter to skip this step:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509StateCode.png)

3. Next step, enter your Health organization name (for example: Ministry of Health of Peru Demo Brasil) as shown:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganization.png)

4. Once completed the previous step, you will be asked to enter a common nane for your Health organization (for example: BrasilDemo_MoH), or you may press enter to skip this step:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganizationCommonName.png)

5. After completing these steps, the data specified for the self-signed certificate will be displayed asking you to confirm. Note: Keep in mind that if a valid Country code wasn't specified you will probably get an error.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509DataConfirmation.png)

6. Once confirmed, the self signed certificate creation with the data is displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509Creation.png)

7. A subdirectory named `/certs` is created inside the directory you are running the script, with two subdirectories Document Signer Certificates `/DSC` and Signing Certificate Authority `/SCA` as displayed:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/certsDirSubdirs.png)

The Document Signer Certificates `/DSC` subdirectory contains three files: 
`DSC.crt` is the X.509 certificate.
`DSC.csr` is the Certificate Signing Request contains the public key and common name required by a Certificate Authority.
`DSC.key` is the private key used to sign Health Certificates, to complete this step please:
Copy the `DSC.key` file into the `cert-data` directory located in the root directory of your `IPS-national-backend` repository.
Rename the new copy of DSC.crt to priv.pem

The Signing Certificate Authority  `/SCA` subdirectory contains two files:

`SCA.crt` is 
`SCA.key` is the 







