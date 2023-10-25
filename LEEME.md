# LACPass-LACChain-Component 

## Resumen

El componente de software LACPass-LACChain permite a los Ministerios u Organizaciones de Salud a gestionar su incorporación en la red de confianza LACPass, y los habilita para emitir y enviar certificados de salud a pacientes o individuos. Este manual describe los pasos para ejecutar el componente LACPass-LACChain y especifica como usar los endpoints del servicio.


### Requerimientos 
- Contar con una instancia en ejecución del componente LACPass-LACChain parte del `IPS-national-backend` disponible en https://github.com/lacchain/IPS-national-backend
- Acceso al script ejecutable `client-helper` disponible en https://github.com/lacchain/IPS-national-backend#lacchain-setup-and-onboard-helper
- Acceso a Internet
- Software [Postman](https://www.postman.com/) para interactuar con APIs

### Verificar la disponibilidad del servicio
1. Al ejecutar el componente lacpass-lacchain desde `IPS-national-backend` el servicio queda expuesto en el puerto 3010. 
2. Para verificar la ejecución del componente lacpass-lacchain puede revisar el log o ejecutar el comando el comando telnet en un shell bash con el URL apropiado:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Proceso de configuración

Para crear el identificador descentralizado [DID](https://w3c.github.io/did-core) de su organización de salud y algunas llave requeridas ejecute el CLI (script ejecutable `client-helper`).
1. Una vez verificada la disponibilidad del servicio segín la sección previa [Verificar la disponibilidad del servicio](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability)
2. Previo a ejecutar el CLI, debe ejecutar estos comandos en un shell linux bash:

```
$ chmod +x client-helper.sh
$ bash client-helper.sh
```

3. Ahora ingrese el URL del servicio lacpass-lacchain verificado en la sección previa [Verificar la disponibilidad del servicio](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) como se muestra en el siguiente prompt:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/apiURL.png)

4. A continuación se presenta el siguiente Menú Principal del CLI:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLIMainMenu.png)


## Incorporación de organizaciones de Salud en la red de confianza LACPass 

Para incorporar la organización de salud de su país debe verificar si su organización de salud cuenta con una Infraestructura de Llave Pública implementada, y revise las siguientes opciones.

### Países sin Infraestructura de Llave Pública (PKI)
Si la organización de salud de su País no tiene implementada una Infraestructura de Llave Pública, se le pedirá la creación de un certificado auto-firmado (en inglés Self-Signed Certificate [SSC]) con los siguientes pasos.

En el Menú Principal del CLI tipee 'SSC' e ingrese la información requerida: 

1. El primer paso es ingresar el [Código de País](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) para el certificado auto-firmado de su organización de salud:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509CountryCode.png)

2. Ahora puede ingrear el [Código de Estado](https://en.wikipedia.org/wiki/ISO_3166-2) del país seleccionado (por ejemplo: https://en.wikipedia.org/wiki/ISO_3166-2:BR si el país es Brasil), o puede presionar enter para saltar este paso:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509StateCode.png)

3. A continuación, ingrese el nombre de su organización de Salud (por ejemplo: Ministerio de Salud de Peru Demo Brasil) como se muestra:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganization.png)

4. El próximo paso, debe ingresar un nombre común (Common Name) para su organización de Salud (por ejemplo: BrasilDemo_MoH), o puede presionar enter y omitir este paso:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509HealthOrganizationCommonName.png)

5. Una vez completados estos pasos, los datos especificados para el certificado auto-firmado serán desplegados solicitando su confirmación. **Nota:** Tenga en mente que si no se especificó un Código de País válido, probablemente tenga un error.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509DataConfirmation.png)

6. Una vez confirmado, la creación de certificado auto-firmado y los datos son desplegados:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509Creation.png)

7. Un subdirectorio con nombre `/certs` es creado dentro del directorio donde está ejecutando el script, con dos subdirectorios **Document Signer Certificates** `/DSC` y **Signing Certificate Authority** `/SCA` como se muestra:

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

## Enviando certificados de Salud contenidos en Credenciales Verificables

In this section you will learn how to use the endpoint exposed by the lacpass-lacchain component to send health certificates as verifiable credentials. If you successfully followed the previous steps, you are ready to send health certificates to your users. 

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
* **bundle:** FHIR bundle, just copy and paste the full FHIR bundle
* **IssuerDid:** Issuer DID, this is the decentralized identifier you created in the section Running Setup/Onboard steps in step 6 and that is available in the lacchain-setup-helper/did.txt file
* **receiverDid:** This is the Receiver DID (the patient/individual receiving the certificate) will share with you to receive the issued credential in their wallet. Patients/individuals can easily get their unique decentralized identifier (DID) after setting up their digital wallet available at https://lacpass-openprotest-wallet.lacchain.net/

**NOTE:** A full example with the required payload is available at https://github.com/lacchain/LACPass-client/blob/master/docs/Credential-Sending.md

### LACPass Verifier

LACPass Verifier is the last component used to verify DDCC-compliant health certificates. This component is made up of two subcomponents:

1. LACPass-front-verifier: This is a full front-end component that needs to be connected to LACPass-trusted-List to check the validity of health certificates. The repository is available at https://github.com/lacchain/LACPass-front-verifier
2. LACPass-trusted-list: This is the backend API component which cryptographically verifies certificate issuers and decodes data returning it alongside the certificate health validity. Access to this repository is available at https://github.com/lacchain/LACPass-trusted-list

**Note:** A fully runnning instance of LACPass Verifier can be found at https://lacpass.lacchain.net/

