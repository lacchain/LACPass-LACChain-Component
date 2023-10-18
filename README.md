# LACPass-LACChain-Component 

## Manual Summary

The LACCPass Client component allows Ministries of Health to handle the onboarding, as well as the issuance and delivery of health certificates to patients. This manual describes the steps to run the LACPass-LACChain component, and specifies how to use the endpoints.

### Requirements
- Make sure you have up and running and instance of the LACPass Client component available at https://github.com/lacchain/IPS-national-backend
- Access to the client-helper executable script available at https://github.com/lacchain/IPS-national-backend#lacchain-setup-and-onboard-helper
- Internet access
- [Postman](https://www.postman.com/) software to interact with APIs

### Verify Service availability
1. Running the lacpass-lacchain component from IPS-national-backend will expose the service at port 3010
2. Verify the lacpass-lacchain component is running either checking the logs or just performing a telnet command in a bash shell:

![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/telnet3010.png)

### Running Setup

Now run the CLI (Client helper executable script) to setup your decentralized identifier [DID](https://w3c.github.io/did-core) and set some keys.
1. Make sure you have verified the service availability as described in the previous section [Verify service availability](https://github.com/lacchain/LACPass-LACChain-Component#verify-service-availability)
2. Before running the CLI make sure to execute this in a linux bash terminal:

`$ ./client-helper.sh`

3. Now enter the URL of the lacpass-lacchain verified in the previous section as shown in the following prompt


![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/apiURL.png)

4. The following menu is presented:


![](https://github.com/lacchain/LACPass-LACChain-Component/blob/main/examples/CLIMainMenu.png)


Onboard steps