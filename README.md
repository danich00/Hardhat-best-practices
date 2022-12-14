# Setup hardhat project

1. Create a new folder for the project.

2. cmd the folder and write on the terminal: npm install --save-dev hardhat

3. To set up a project: npx hardhat

4. Choose create a JavaScript project

5. Create a new Contract file and write the code.

6. Modify 'deploy.js' file:
  
  ```
  const HelloWorld = await hre.ethers.getContractFactory("HelloWorld");
  const contract = await HelloWorld.deploy();
  await contract.deployed();
  console.log(`Contract deployed to ${contract.address}`);
  ```
  
7. Add network to the hardhat.config.js. ADVISE: don't write the private key. INSTEAD: npm install --save-dev dotenv

	1. Now on top of 'hardhat.config.js': require("dotenv").config();
  
	2. Inside the network config: 
  
    ```
    url: process.env.RPC_URL, 
    accounts: [proccess.env.PRIVATEKEY]
    ```
    
	3. Create a .env file and write:
  
    ```
		RPC_URL = YourNodeProviderURL
		2. PRIVATE_KEY=c YourPrivateKey
    ```
    
	4. Write on 'deploy.js': we use the following code because etherscan is a third party and may take some time to etherscan to now that a new SC has been deployed.
  
    ```
    async function sleep(ms) {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve();
        }, ms);
      });
    ```
    
    Inside the function main where the contract is deployed write: 
    
      ```
        //Delay  
        await sleep(15*1000); 
        await hre.run("verify:verify", { 
          address: contract.address, 
          //our function don´t have a constructor so we leave it empty 
          constructorArguments:[], 
        })
      ```

  5. We also need to include the API Key to etherscan:

    ```
    etherscan: {
        apiKey: proccess.env.ETHERSCAN_KEY,
      }
    ```
  6. And add in the .env:
    
     ```
     ETHERSCAN_KEY= YOURAPI
     ```

9. Check if the '.env' file is included in the '.gitignore' file to protect the keys
10. Deploy the contracts on goerli:
	1. npx hardhat run scripts/deploy.js --network goerli

