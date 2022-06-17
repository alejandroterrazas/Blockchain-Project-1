# Private Blockchain Application

This project demonstrates the following fundamentals of a Blockchain platform:
    - Block
    - Blockchain
    - Wallet
    - Blockchain Identity
    - Proof of Existance

## General Concept

Develop a Blockchain application for registering stars.  The application includes ownership and naming rights for the stars.

1. The application will create a Genesis Block when we run the application.
2. The user will request the application to send a message to be signed using a Wallet and in this way verify the ownership over the wallet address. The message format will be: `<WALLET_ADRESS>:${new Date().getTime().toString().slice(0,-3)}:starRegistry`;
3. Once the user has the message the user can use a Wallet to sign the message.
4. The user will try to submit the Star object for that it will submit: `wallet address`, `message`, `signature` and the `star` object with the star information.
    The Star information in this format:
    ```json
        "star": {
            "dec": "68° 52' 56.9",
            "ra": "16h 29m 1.0s",
            "story": "Testing the story 4"
		}
    ```
5. The application will verify if the time elapsed from the request ownership (the time is contained in the message) and the time when you submit the star is less than 5 minutes.
6. If everything is okay the star information will be stored in the block and added to the `chain`
7. The application will allow us to retrieve the Star objects belong to an owner (wallet address). 


## Tools used

- This application is created using Node.js and the Javascript programming language. The architecture will use ES6 classes to help organize the code and facilitate the maintnance of the code.

- Some of the libraries or npm modules used are:
    - "bitcoinjs-lib": "^4.0.3",
    - "bitcoinjs-message": "^2.0.0",
    - "body-parser": "^1.18.3",
    - "crypto-js": "^3.1.9-1",
    - "express": "^4.16.4",
    - "hex2ascii": "0.0.3",
    - "morgan": "^1.9.1"
    


Libraries Used:

1. `bitcoinjs-lib` and `bitcoinjs-message`. Verify the wallet address ownership and signature.
2. `express` The REST Api (created using Express.js framework).
3. `body-parser` this library will be used as middleware module for Express and decoding url requests.
4. `crypto-js` Contain the cryotographic methods for creating the block hash.
5. `hex2ascii` For **decoding** the data saved in the body of a Block.

## Understanding code

The code is a simple architecture for a Blockchain application, it includes a REST API application to expose the Blockchain application methods.

1. `app.js`. Contains the configuration and initialization of the REST API.
2. `BlockchainController.js`. Contains the routes of the REST API; that is, the methods that expose the urls to call when a request is made to the application.
3. `src` folder. Contains`block.js` and `blockchain.js` that contain `Block` and `BlockChain` classes.

You can check in your terminal the the Express application is listening in the PORT 8000


## Testing the Code

Start the code `node app.js`

Curl was used to test the code using five tests-
From a terminal, change directories into the Blockchain-project-1 folder. Run your application using the command `node app.js`  The terminal will respond with a message indicating that the server is listening in port 8000: 

The following commands can be run in curl or postman.  A screen shot is included for each command.  

##Request the genesis block:  
```
curl --location --request GET 'http://localhost:8000/block/height/0' | json_pp
```

![] (./screenshots/Block0.png)
    
##Request ownership using wallet address:

```
curl --location --request POST 'http://localhost:8000/requestValidation' \
--header 'Content-Type: application/json' \
--data-raw '{
    "address" : "1Asc2Yi4ETvucexs2TtFqNUcSWEL3njkyt"
}' | json_pp
```

![] (./screenshots/RequestValidation.png)

Copy the message for the next step.  


##Sign the message with the Electrum Wallet:
Important!! Use non-segwit addresses.  The messagejs-block module will not run properly with Electrum segwit addresses.  Use an older version of Electrum.  

![] (./screenshots/ElectrumSignMessage.png)

Copy the signature for the next step.

##Submit a Star

```
curl --location --request POST 'http://localhost:8000/submitstar' \
--header 'Content-Type: application/json' \
--data-raw '{
    "address" : "1Asc2Yi4ETvucexs2TtFqNUcSWEL3njkyt",
    "signature" :"INxnxsn7g/2kAd7IR20/mV7uDauj3tyfF0X59elZbK0QbdpjPAV+xU825EO3+tNg6n666KEKLNnJzDpNKCyUmoM=",
    "message" : "1Asc2Yi4ETvucexs2TtFqNUcSWEL3njkyt:1655499229:starRegistry",
    "star" : {
        "dec" : "68 52 56.9",
        "ra" : "16h 29m 1.0s",
        "story" : "Testing submitStar 4"
    }
}'

```

![] (./screenshots/SubmitStar.png)

##Retrieve Stars at a Wallet Addess.
```
curl --location --request GET 
'http://localhost:8000/blocks/1Asc2Yi4ETvucexs2TtFqNUcSWEL3njkyt'

```

![] (./screenshots/GetStarsByWallet.png)


