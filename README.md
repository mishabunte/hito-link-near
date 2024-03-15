# Hito Link Near - hardware wallet interface for Near Protocol

This document describes of how to communicate with Hito wallet from an app

1. [Linking app with Hito wallet](#link)
1. [Signing NEAR transaction](#near)
1. [Verifying named NEAR account](#named-near) 
1. [Signing ethereum transaction](#ethereum)
1. [Signing secp256k1 and ed25519](#expertsign)
1. [Get pubkey/address for a BIP44 path](#bip44) 
1. [Sample code](#sample)
1. [Libraries](#libs)


## <a name="link"/>1. Linking app with Hito wallet

To link an app with Hito wallet the app need to obtain a public address 
from a Hito wallet via QR-code and save it on our side 

### Display the public address on Hito wallet 
1. Unlock Hito wallet with entering the passcode
2. Tap "Menu" on top left corner
3. Choose "Pair with the App" 
4. Choose "Near Protocol" - device will display public key as a qrcode 

### Scan the Qr-code with the App

Hito provides a NEAR public key as a Qr-Code in base58 format:
```
CWEMFDimUuyPcBnS5PM3FUnvXceCXvEF1L72BGHTyZt1
```

![Device with the ethereum address displayed](https://raw.githubusercontent.com/mishabunte/hito-link-near/main/img/address.jpg?1)

### API public endpoints

TODO: add api endpoints here

## <a name="near"/>2. Signing NEAR transaction 

Signing is done via NFC. The app should transfer the unsigned signature  
as NDEF message to the device. Payload format is below.

![](https://raw.githubusercontent.com/mishabunte/hito-link-near/main/img/send.jpg?1)

1. Prepare device - Unlock and Tap "SEND"
2. Send NDEF message from app to device (NFC antenna is under the screen)
3. Device will parse the transaction and displays the confirmation
4. After signing Hito device will display the signature in 
the hex format as a qr-code

##### NDEF Payload format
```
near.sign:0x<unsigned_transaction>
<unsigned_transaction>      - unsigned transaction serialized in borsh in hex format with 0x prefix
```

### Near transfer 

#### NDEF Payload

`near.signe:0x400000006537653337316633633831353266613035356536626661643037653233656537386635383466616266393835393663666164333061316333313761393837346600e7e371f3c8152fa055e6bfad07e23ee78f584fabf98596cfad30a1c317a9874f05a768a29e900000100000006869746f746573742e746573746e65742a094849434e1117af66aec954e58d9483bee3e44863bafc0b45618b4510115a0100000003000000a1edccce1bc2d3000000000000`

![Confirmation](https://raw.githubusercontent.com/mishabunte/hito-link-near/main/img/confirm.jpg?1)


#### Completed transaction
https://sepolia.etherscan.io/tx/0xdbdb3ed9f6f65706568bbcb5cfa75ea4998f94f7dec68fb58bcc656c8e58ba27

![Signed transaction on device](https://raw.githubusercontent.com/mishabunte/hito-link-near/main/img/signed.jpg?1)

## <a name="named-near"/>3. Verifying named NEAR account

TODO: we will implement a protocol to verify a part of the near 
chain between signing and confirming transaction for AddKey, CreateAccount


## <a name="ethereum"/>4. Signing ethereum transaction 

Signing is done via NFC. The app should transfer the unsigned signature  
as NDEF message to the device. Payload format is below.

![](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/send.jpeg)

1. Prepare device - Unlock and Tap "SEND"
2. Send NDEF message from app to device (NFC antenna is under the screen)
3. Device will parse the transaction and displays the confirmation
4. After signing Hito device will display the signed transaction in 
the hex format as a qr-code

##### NDEF Payload format
```
eth.send:<address>:<unsigned_transaction>
<address> - ethereum address in hex format with 0x prefix
<tx>      - unsigned transaction in hex format with 0x prefix
```



### Eth transfer on Sepolia 

#### NDEF Payload

`eth.send:0x56DCE5b7A8656b1aE45a0FbfCe504CE59196C7b8:0xee818d840b766ae082520894feed146aa5f20bc991a994a1ac2fe0bb75df2bb087038d7ea4c680008083aa36a78080`

![Confirmation](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/eth_confirm.jpeg)


#### Completed transaction
https://sepolia.etherscan.io/tx/0xdbdb3ed9f6f65706568bbcb5cfa75ea4998f94f7dec68fb58bcc656c8e58ba27

![Signed transaction on device](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/signed_transaction.jpeg)


### Erc20 token transfer on Sepolia 

#### NDEF Payload
`eth.send:0x56DCE5b7A8656b1aE45a0FbfCe504CE59196C7b8:0xf86c818e840c674b5982b40e94b2d6bf8aed4db22e29e29a57724395ad05669a3380b844a9059cbb000000000000000000000000feed146aa5f20bc991a994a1ac2fe0bb75df2bb00000000000000000000000000000000000000000000000000000000005f5e10083aa36a78080`

![Confirmation](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/erc20_confirm.jpeg)


#### Completed transaction
https://sepolia.etherscan.io/tx/0xb0732708bd1cc483e48bdc379be101b537267eab2d606cd9c51fa28800f31165

![Signed transaction on device](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/signed_erc20.jpeg)



## <a name="expertsign"/>5. Signing Secp256k1 and Ed25519 

![Hito Device with expert mode enabled](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/expertmode.jpeg)

1. Prepare device - Unlock and Tap "SEND"
2. Send NDEF message from app to device (NFC antenna is under the screen)
3. Device will parse the transaction and displays the confirmation
4. After signing Hito device will display the signed transaction in 
the hex format as a qr-code

#### NDEF Payload - signing bitcoin output hash using bitcoin testnet address  
`
hito.sign:secp256k1:raw:hex:m/44'/0'/0'/0/0:000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f:callback_url?
`

## <a name="bip44"/>6. Get pubkey/address for a BIP44  

![Hito Device with expert mode enabled](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/expertmode.jpeg)

1. Prepare device - Unlock and Tap "SEND"
2. Send NDEF message from app to device (NFC antenna is under the screen)
3. Device will parse the transaction and displays the confirmation
4. After signing Hito device will display the signed transaction in 
the hex format as a qr-code

#### NDEF Payload example - display bitcoin testnet address as a qr-code
```
hito.pubkey:secp256k1:ripemd160:bech32:m/44'/1'/0'/0/0::
````

![Bitcoin address confirmation](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/bitcoin_confirm.jpeg)


#### Bitcoin address after confirmation screen
```
bitcoin:tb1q4k345qwkhss6x0cz0fwcsurw2qngmvwl9tf3sk?bip32=m/44'/1'/0'/0/0
```

![Bitcoin address](https://raw.githubusercontent.com/mishabunte/hito-link/main/img/bitcoin_qr.jpeg)

## <a name="sample"/>7. Sample code

### WebNFC Javascript

```javascript
const ndef = new NDEFReader();

let nfcPayloadTransferNear = 
  'near.sign:400000006537653337316633633831353266613035356536626661643037653233656537386635383466616266393835393663666164333061316333313761393837346600e7e371f3c8152fa055e6bfad07e23ee78f584fabf98596cfad30a1c317a9874f05a768a29e900000100000006869746f746573742e746573746e65742a094849434e1117af66aec954e58d9483bee3e44863bafc0b45618b4510115a0100000003000000a1edccce1bc2d3000000000000';

await ndef.write({ 
    records: [ {                                
        data: nfcPayloadTransferNear,
        recordType: "text",·                                            
        lang: 'en',                                                     
    }],
});      

```


### CoreNFC Swift 

```swift
import CoreNFC

class NFCController: NSObject, NFCNDEFReaderSessionDelegate {

   // ..................

   func readerSession(_ session: NFCNDEFReaderSession, didDetect tags: [NFCNDEFTag]) {
        
        let payload = NFCNDEFPayload(format: NFCTypeNameFormat.nfcWellKnown,
                                     type: Data(_: [0x54]), identifier: Data(),
                                     payload: hitoNfcRequest.payload.data(using: .utf8)!)

        guard tags.count == 1 else {
            session.invalidate(errorMessage: "Hito Device protocol is invalid.")
            return
        }
        let currentTag = tags.first!

        session.connect(to: currentTag) { error in

            guard error == nil else {
                session.invalidate(errorMessage: "Could not connect to Hito Wallet.")
                return
            }

            currentTag.queryNDEFStatus { status, capacity, error in
                guard error == nil else {
                    session.invalidate(errorMessage: "Could not query status of Hito Wallet.")
                    return
                }

                switch status {
                case .notSupported:
                    session.invalidate(errorMessage: "Protocol is not supported.")
                case .readOnly:
                    session.invalidate(errorMessage: "Protocol is only readable.")
                case .readWrite:
                    let message = NFCNDEFMessage.init(records: [payload])
                    currentTag.writeNDEF(message) { error in
                        if error != nil {
                            session.invalidate(errorMessage: "Failed to write message.")
                        } else {
                            session.alertMessage = "Scan to Transmit"
                            self.hitoNfcRequest.isDataTransmitted = true
                            session.invalidate()
                        }
                    }
                @unknown default:
                    session.invalidate(errorMessage: "Unknown status of device.")
                }
            }
        }
    }
}
```

https://github.com/mishabunte/hito-link/tree/main/swift

## <a name="libs"/>6. Libraries

TODO: add links

### Qr code
### NFC 


### Sample App for debug purposes
https://testflight.apple.com/join/kDWUnQGq


