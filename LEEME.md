# LACPass-LACChain-Component 

## Resumen

El componente de software LACPass-LACChain permite a los Ministerios u Organizaciones de Salud gestionar su incorporación en la red de confianza LACPass, y los habilita para emitir y enviar certificados de salud a pacientes o individuos. 

Este manual describe los pasos para ejecutar el componente LACPass-LACChain y especifica como usar los endpoints del servicio.

### Requerimientos
- Acceso a Internet
- Contar con una instancia en ejecución del componente LACPass-LACChain parte del `IPS-national-backend` disponible en https://github.com/RACSEL/IPS-national-backend
- Acceso al script ejecutable `client-helper.sh` disponible en https://github.com/lacchain/IPS-national-backend/blob/master/lacchain-setup-helper/client-helper.sh
- Software [Postman](https://www.postman.com/) 

### Verificar la disponibilidad del servicio
1. Al ejecutar el componente lacpass-lacchain desde `IPS-national-backend` el servicio queda expuesto en el puerto 3010. 
2. Para verificar la ejecución del servicio lacpass-lacchain puede revisar el log o ejecutar el comando telnet en un shell bash con el URL apropiado:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Configuración del servicio

A continuación debe ejecutar el menú principal del componente LACPass-LACChain el CLI Main Menu, que le permitirá crear un identificador decentralizado [DID](https://w3c.github.io/did-core) para su organización de Salud y algunas llave requeridas para firmar los certificados de Salud. 

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


## Proceso de configuración para incorporar organizaciones de salud

Por favor siga los siguientes pasos para incorporar su organización de Salud en la red de confianza LACPass

1. Crear un identificador descentralizado [DID](https://w3c.github.io/did-core) para su organización de salud, tipeando 'CD' en el menú CLI Main Menu:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLICreateDID.png) 

se creará un DID y será almacenado en el archivo `did.txt`: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didtxtfile.png)

2. A continuacion debe crear un gestor para el DID, tipee 'CM' en el menú CLI Main Menu e ingrese el número de días por el cual será válido el gestor del DID, por ejemplo: 1000 días. No ingrese un número menor a 365 días.

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingManager.png)

se desplegará una respuesta exitosa: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/didManagerCreation.png)

3. Para finalizar el proceso de configuración para la incorporación, tipee 'exit'.
 
![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/exit.png)

Una vez completado el proceso de configuración para la incorporación, la siguiente información estará en el directorio `lacchain-setup-helper`:

- Un archivo `did.txt` que contiene el identificador decentralizado (DID) de su organización, manténgalo cerca por si requiere acceder al DID.

## Compartir la información para incorporación en la red de confianza LACPass

Ahora puede compartir con el comité la información para incorporación de su organización de salud en la red de confianza, por favor siga los siguientes pasos:

1. Inicie el CLI nuevamente.
2. Ahora ingrese el URL del servicio lacpass-lacchain (como lo hizo initialmente)
3. Tipee 'GCM' (Get Current Manager) **para obtener los detalles de la entidad y el gestor** como se muestra:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/GCM.png)

Copie el contenido en un archivo de texto y nómbrelo similar a `Entity-Manager-Details.txt`. 

4. Incluya la siguiente información en un archivo comprimido (zip):

a) El archivo `Entity-Manager-Details.txt`

b) Información de identificación de la organization de Salud en un archivo de texto:

```
  i. Nombre legal
 ii. FHIR-URL
iii. Código de País/Estado
``` 

5. Enviar el archivo comprimido (zip) vía correo electrónico a epacheco@iadb.org y antoniole@iadb.org

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
* **bundle:** Bundle FHIR DDCC, únicamente copiar y pegar el Bundle completo.
* **IssuerDid:** DID del Emisor, este es el DID creado en la sección [Proceso de configuración para incorporar organizaciones de salud](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/LEEME.md#proceso-de-configuraci%C3%B3n-para-incorporar-organizaciones-de-salud) en el paso 1 y se encuentra disponible en el archivo `lacchain-setup-helper/did.txt`.
* **receiverDid:** DID del Receptor (el paciente/individuo que recibe el certificado) que debe compartir con Usted para recibir la credencial emitida en su billetera. Los individuos/pacientes pueden obtener su identificador decentralizado (DID) luego de registrar su billetera digital disponible en https://lacpass-openprotest-wallet.lacchain.net/.

**Nota:** Un ejemplo completo con el payload requerido está disponible en https://github.com/lacchain/LACPass-client/blob/master/docs/Credential-Sending.md

## Configurando la billetera digital para pacientes/individuos

### Requerimientos

- Acceso a Internet
- Web browser (Chrome, Firefox, Opera) desktop o móvil
- Acceso a la configuración de la billetera digital disponible en https://lacpass-openprotest-wallet.lacchain.net/

Por favor seguir los siguientes pasos para configurar su billetera digital y recibir sus certificados de salud.

1. Al acceder por primera vez a la billetera digital LACPass (https://lacpass-openprotest-wallet.lacchain.net/) la vista será la siguiente:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletLanding.png)

(En caso que se encuentre registrado puede ingresar su correo y password como credenciales de login en la billetera).

2. De no estar registrado, por favor haga click en el enlace [register](https://lacpass-openprotest-wallet.lacchain.net/register) que le lleva al formulario de Creación de cuenta (registro):

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletRegistration.png)

3. Por favor ingrese la siguiente información de **Usuario: primer nombre, primer apellido, segundo apellido** y **Cuenta: email (correo) y contraseña**

**Nota:** Una vez completada la información de registro y hacer click en `Create account` los datos de la cuenta serán encriptados en el almacenamiento local del browser (navegador).

Al hacer click en `Create account` se muestran las siguientes operaciones:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/creatingNewAccountDID.png)

a. Generando un nuevo DID

b. Registrando las Llaves Públicas

c. Modificando el Controlador

d. Firmando la Credencial LACChain ID

e. Auto-emitiendo la Credencial LACChain ID en la billetera

4. Una vez la billetera esta configurada, contiene la siguiente Credencial LACChain ID con la información registrada por el usuario:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassWalletUserLACChainIDHome.png)

5. Haga click sobre la Credencial LACChain ID:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredential.png)

6. La siguiente información de la Credencial LACChain es desplegada como `Claims`: **Id, Nombre, Apellido y correo electrónico**. 

- El **Id** es el identificador decentralizado [DID](https://w3c.github.io/did-core) del usuario (paciente/individuo) en su billetera digital, en el siguiente ejemplo el valor del DID es: 
`did:lac1:1iT5kyRRuZHYjW1HJfGyNgGbCcZqhuDJLco5N4PemXBjkFSzkgP3YdLRd5BHHGT7LccZ`

- El valor del DID de su billetera digital debe ser compartido con su organización de Salud, para emitir y recibir las credenciales de salud en su billetera digital. 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDID.png)

7. Ahora, por favor haga click sobre el botón superior derecho (con el óvalo azul) que muestra el nombre del usuario de la billetera:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightButton.png)

8. El siguiente menú se despliega: 

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightMenu.png)

- Debajo del nombre de usuario (el cual está borroso en la imagen) es el valor del identificador DID, el cual puede ser copiado haciendo click en los dos botones azules a la derecha.

- El valor del DID de su billetera digital debe ser compartido con su organización de Salud, para que puedan emitir y entregar las credenciales de salud a su billetera digital. 

- Para sincronizar su billetera digital y recibir la credenciales de salud emitidas haga click en el botón **Sync** como se muestra:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/lacpassLACChainIDCredentialDIDUpperRightMenuSync.png)

## Verificador LACPass

El componente verificador LACPass permite verificar los certificados de salud conformes a la especificación DDCC. Este componente contiene dos subcomponentes:

1. LACPass-front-verifier: Componente front-end que requiere conectarse con el componente `LACPass-trusted-List` para verificar la validez de los certificados de salud. El repositorio de este componente está disponible en https://github.com/lacchain/LACPass-front-verifier
2. LACPass-trusted-list: Componente del backend API que criptográficamente verifica los emisores de certificados y decodifica data retornando la data y la validez del certificado de salud. El repositorio de este componente está disponible en https://github.com/lacchain/LACPass-trusted-list

**Nota:** Una instancia en ejecución del Verificador LACPass está disponible en https://lacpass.lacchain.net/

