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

