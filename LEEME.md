# LACPass-LACChain-Component 

## Resumen

El componente de software LACPass-LACChain permite a los Ministerios u Organizaciones de Salud a gestionar su incorporación en la red de confianza LACPass, y los habilita para emitir y enviar certificados de salud a pacientes o individuos. Este manual describe los pasos para ejecutar el componente LACPass-LACChain y especifica como usar los endpoints del servicio.

### Requerimientos 
- Contar con una instancia en ejecución del componente LACPass-LACChain parte del `IPS-national-backend` disponible en https://github.com/RACSEL/IPS-national-backend
- Acceso al script ejecutable `client-helper` disponible en https://github.com/lacchain/IPS-national-backend/blob/master/lacchain-setup-helper/client-helper.sh
- Acceso a Internet
- Software [Postman](https://www.postman.com/) para interactuar con APIs

### Verificar la disponibilidad del servicio
1. Al ejecutar el componente lacpass-lacchain desde `IPS-national-backend` el servicio queda expuesto en el puerto 3010. 
2. Para verificar la ejecución del componente lacpass-lacchain puede revisar el log o ejecutar el comando telnet en un shell bash con el URL apropiado:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Proceso de configuración

A continuación ** CONTINUACI
ON Para crear el identificador descentralizado [DID](https://w3c.github.io/did-core) de su organización de salud y algunas llave requeridas ejecute el script ejecutable `client-helper`.
1. Una vez verificada la disponibilidad del servicio según la sección previa [Verificar la disponibilidad del servicio](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability)
2. Previo a ejecutar el CLI, debe ejecutar estos comandos en un shell linux bash:

```
$ chmod +x client-helper.sh
$ bash client-helper.sh
```

3. Ahora ingrese el URL del servicio lacpass-lacchain verificado en la sección previa [Verificar la disponibilidad del servicio](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability) como se muestra en el siguiente prompt:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/apiURL.png)

4. A continuación se presenta el Menú Principal del CLI (CLI Main Menu):

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLIMainMenu.png)

## Incorporación de organizaciones de Salud en la red de confianza LACPass 

Para incorporar la organización de Salud de su país en la red de confianza debe verificar una de las siguientes opciones:

1. Su organización no cuenta con una Infraestructura de Llave Pública (PKI) implementada debe continuar con la siguiente sección https://github.com/lacchain/LACPass-LACChain-Component/blob/main/LEEME.md#pa%C3%ADses-sin-infraestructura-de-llave-p%C3%BAblica-pki

2. Su organización Si cuenta con una Infraestructura de Llave Pública (PKI) implementada debe continuar
Si su organización de Salud y revise las siguientes opciones.



### Países sin Infraestructura de Llave Pública (PKI)
Si la organización de Salud de su País no tiene implementada una Infraestructura de Llave Pública, se le pedirá la creación de un certificado auto-firmado (en inglés Self-Signed Certificate [SSC]) con los siguientes pasos.

En el Menú Principal del CLI tipee 'SSC' e ingrese la información requerida: 

1. El primer paso es ingresar el [Código de País](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) para el certificado auto-firmado de su organización de Salud:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509CountryCode.png)

2. Ahora puede ingrear el [Código de Estado](https://en.wikipedia.org/wiki/ISO_3166-2) del país seleccionado (por ejemplo: https://en.wikipedia.org/wiki/ISO_3166-2:BR si el país es Brasil), o puede presionar enter para omitir este paso:

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

El subdirectorio Document Signer Certificates `/DSC` contiene tres archivos:

a) `DSC.crt` es el certificado X.509.

b) `DSC.csr` es el Certificate Signing Request que contiene la llave pública y el nombre común requerido por una Autoridad Certificadora.

c) `DSC.key` es la llave privada usada para firmar los Certificados de Salud, si su organización de Salud usará esta llave (opcional), por favor complete estos pasos:
- Copie el archivo `DSC.key` dentro del directorio `cert-data` localizado en el directorio raíz de su repositorio `IPS-national-backend`.
- Renombre la nueva copia de `DSC.crt` a `priv.pem`

El subdirectorio Signing Certificate Authority `/SCA` contiene dos archivos:

a) `SCA.crt` es el certificado X.509.

b) `SCA.key` es la llave privada de la Autoridad Certificadora que firma.

Una vez completado estos pasos habrá creado exitosamente un certificado auto-firmado para la organización de Salud de su País.

### Países con Infraestructura de Llave Pública (PKI)
Si la organización de Salud de su País tiene una Infraestructura de Llave Pública implementada, el proceso de incorporación a la red de confianza LACPass require que use su certificado X.509 prra completar el proceso. 

## Proceso de configuración para incorporar organizaciones de salud

Usando los certificados PKI X.509 de la organización de Salud de su País, por favor siga los siguientes pasos para incorporar su organización de Salud en la red de confianza LACPass

1. Crear un identificador descentralizado [DID](https://w3c.github.io/did-core) para su organización de salud, tipeando 'CD' en el menú CLI Main Menu:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLICreateDID.png) 

se creará un DID y será almacenado en el archivo `did.txt`: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didtxtfile.png)

2. El próximo paso es asociar el DID creado en el paso previo con el certificado X.509 que será usado para firmar los certificados de Salud, tipee 'AX' en el menú CLI Main Menu e ingrese la ruta donde se encuentra el certificado X.509. (Si creó un certificado auto-firmado la ruta sería como la indicada en el paso 7 de los países sin PKI):
  
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/X509path.png)

Una vez ingresada la ruta apropiada debe recibir un mensaje indicando que el certificado X.509 fue asociado exitosamente con el DID: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didx509association.png)

**Nota:** el DID debe permanecer igual para que la asociación con el certificado X.509 sea válida. 

3. A continuacion debe crear un gestor para el DID, tipee 'CM' en el menú CLI Main Menu e ingrese el número de días por el cual será válido el gestor del DID, por ejemplo: 1000 días. No ingrese un número menor a 365 días.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingManager.png)

se desplegará una respuesta exitosa: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didManagerCreation.png)

4. Para finalizar el proceso de configuración para la incorporación, tipee 'exit'.
 
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/exit.png)

Una vez completado el proceso de configuración para la incorporación, la siguiente información estará en el directorio `lacchain-setup-helper`:

- Un archivo `did.txt` que contiene el identificador decentralizado (DID) de su organización, manténgalo cerca por si requiere acceder al DID.
- Un directorio `/certs` que contiene los subdirectorios `/DSC` y `/SCA`.

## Compartir la información para incorporación en la red de confianza LACPass

Ahora puede compartir con el comité la información para incorporación de su organización de salud en la red de confianza, por favor siga los siguientes pasos:

1. Inicie el CLI nuevamente.
2. Ahora ingrese el URL del servicio lacpass-lacchain (como lo hizo initialmente)
3. Tipee 'GCM' (Get Current Manager) **para obtener los detalles de la entidad y el gestor** como se muestra:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/GCM.png)

Copie el contenido en un archivo de texto y nómbrelo similar a `Entity-Manager-Details.txt`. 

4. Incluya la siguiente información en un archivo comprimido (zip):

a) El archivo `Entity-Manager-Details.txt`

b) El archivo `SCA.crt`, ubicado en el directorio `lacchain-setup-helper/certs/SCA/SCA.crt`  **Nota:** Si su Organización de Salud ya posee una llave publica provista via un PKI omita este paso.

c) Información de identificación de la organization de Salud en un archivo de texto:

```
  i. Nombre legal
 ii. FHIR-URL
iii. Código de País/Estado
``` 

5. Enviar el archivo comprimido (zip) vía correo electrónioco a epacheco@iadb.org y antoniole@iadb.org

## Sending Health certificates wrapped as Verifiable Credentials
## Enviando certificados de Salud contenidos en Credenciales Verificables

En esta sección aprenderá a usar el endpoint expuesto por el componente lacpass-lacchain para enviar certificados de salud como [credenciales verificables](https://www.w3.org/TR/vc-data-model/). De haber completado satisfactoriamente los pasos previos, está listo para enviar certificados de salud a sus usuarios.

La sección [Verificar la disponibilidad del servicio](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/LEEME.md#verificar-la-disponibilidad-del-servicio) describe que el componente `lacpass-lacchain` se ejecuta en el puerto 3010 por defecto. 

Para enviar certificados de salud DDCCCoreDataSet puede usar la herramienta [Postman](https://www.postman.com/) con los siguientes parámetros:

* Method: POST
* http://localhost:3010/api/v1/verifiable-credential/ddcc/send (en caso que no pueda acceder usando localhost por favor actualice apropiadamente)
* El payload requerido para el envío de la credencial tiene la siguiente estructura:

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

Donde:
* **bundle:** bundle FHIR, únicamente copiar y pegar el bundle FHIR completo.
* **IssuerDid:** DID del Emisor, este es el DID creado en la sección [Proceso de configuración para incorporar organizaciones de salud](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/LEEME.md#proceso-de-configuraci%C3%B3n-para-incorporar-organizaciones-de-salud) en el paso 1 y se encuentra disponible en el archivo `lacchain-setup-helper/did.txt`.
* **receiverDid:** DID del Receptor (el paciente/individuo que recibe el certificado) que debe compartir con Usted para recibir la credencial emitida en su billetera. Los individuos/pacientes pueden obtener su identificador decentralizado (DID) luego de registrar su billetera digital disponible en https://lacpass-openprotest-wallet.lacchain.net/.

**Nota:** Un ejemplo completo con el payload requerido está disponible en https://github.com/lacchain/LACPass-client/blob/master/docs/Credential-Sending.md

### Verificador LACPass

El componente verificador LACPass permite verificar los certificados de salud conformes a la especificación DDCC. Este componente contiene dos subcomponentes:

1. LACPass-front-verifier: Componente front-end que requiere conectarse con el componente `LACPass-trusted-List` para verificar la validez de los certificados de salud. El repositorio de este componente está disponible en https://github.com/lacchain/LACPass-front-verifier
2. LACPass-trusted-list: Componente del backend API que criptográficamente verifica los emisores de certificados y decodifica data retornando la data y la validez del certificado de salud. El repositorio de este componente está disponible en https://github.com/lacchain/LACPass-trusted-list

**Nota:** Una instancia en ejecución del Verificador LACPass está disponible en https://lacpass.lacchain.net/

