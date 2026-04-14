# Web3Notary™ API Integration 🚀


**Web3Notary™** is a seamless middleware API designed to bridge legacy Remote Online Notarization (RON) platforms with decentralized Web3 infrastructure. Enhance your existing software stack with immutable, cryptographic proof-of-existence without needing to hire smart contract engineers or manage blockchain wallets.

## 📖 Overview

The transition to blockchain-backed records doesn't require rebuilding your platform from scratch. Web3Notary™ provides standard RESTful endpoints that handle all the heavy lifting of interacting with the blockchain (wallet signing, gas fee management, network routing) and decentralized storage networks (IPFS).

### Key Features

  * **Privacy-First Hashing:** Anchor documents to the blockchain using cryptographic hashes (SHA-256). We never see your clients' raw legal documents.
  * **Immutable Timestamps:** Provide absolute mathematical proof of when a document was finalized and notarized.
  * **Decentralized Storage (Optional):** Pin encrypted documents to the InterPlanetary File System (IPFS) for high-availability decentralized hosting.
  * **Universal Verification:** Easily retrieve on-chain transaction receipts to generate "Verified on Web3" badges or QR codes for your users.

-----

## 🛠 Architecture & Workflow

1.  **Finalize Document:** Your RON platform completes the remote notarization process, generating a final PDF.
2.  **Client-Side Hashing:** Your backend computes a SHA-256 hash of the final PDF.
3.  **Anchor via API:** Your system sends the hash to the `POST /api/v1/notarize` endpoint.
4.  **Web3 Execution:** Web3Notary™ packages the hash into a smart contract transaction, signs it, pays the gas fees, and deploys it to the blockchain.
5.  **Verification:** End-users can scan a QR code or click a link on your site, which queries `GET /api/v1/verify/{hash}` to prove the document's authenticity via an immutable block explorer link.

-----

## 🚀 Quick Start

### Prerequisites

  * An active Web3Notary™ API Key (Get one from the [Developer Dashboard](https://ubitquity.io/web3notary)).
  * A basic HTTP client in your language of choice.

### Base URLs

  * **Sandbox (Testnet):** `https://ubitquity.io/web3notary/api/sandbox`
  * **Production (Mainnet):** `https://ubitquity.io/web3notary/api/mainnet`

-----

## 💻 Code Examples

Here are examples of how to generate a document hash locally and send it to the Web3Notary API.

### Node.js Example

```javascript
const crypto = require('crypto');
const fs = require('fs');
const axios = require('axios');

async function notarizeDocument(filePath) {
    // 1. Generate SHA-256 Hash of the file locally
    const fileBuffer = fs.readFileSync(filePath);
    const hashSum = crypto.createHash('sha256');
    hashSum.update(fileBuffer);
    const documentHash = hashSum.digest('hex');

    console.log(`Computed Hash: ${documentHash}`);

    // 2. Send the hash to Web3Notary
    try {
        const response = await axios.post('https://ubitquity.io/web3notary/api/sandbox/v1/notarize', {
            documentHash: documentHash,
            notaryId: "RON-98765-TX",
            metadata: {
                documentType: "Deed of Trust",
                jurisdiction: "Texas"
            }
        }, {
            headers: {
                'Authorization': `Bearer YOUR_API_KEY_HERE`,
                'Content-Type': 'application/json'
            }
        });

        console.log('Success! Transaction Receipt:', response.data);
    } catch (error) {
        console.error('Error anchoring document:', error.response ? error.response.data : error.message);
    }
}

notarizeDocument('./signed_contract.pdf');
```

### Python Example

```python
import hashlib
import requests
import json

def notarize_document(file_path, api_key):
    # 1. Generate SHA-256 Hash of the file locally
    sha256_hash = hashlib.sha256()
    with open(file_path, "rb") as f:
        for byte_block in iter(lambda: f.read(4096), b""):
            sha256_hash.update(byte_block)
            
    document_hash = sha256_hash.hexdigest()
    print(f"Computed Hash: {document_hash}")

    # 2. Send the hash to Web3Notary
    url = "https://ubitquity.io/web3notary/api/sandbox/v1/notarize"
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }
    payload = {
        "documentHash": document_hash,
        "notaryId": "RON-98765-TX",
        "metadata": {
            "documentType": "Deed of Trust",
            "jurisdiction": "Texas"
        }
    }

    response = requests.post(url, headers=headers, data=json.dumps(payload))
    
    if response.status_code == 200:
        print("Success! Transaction Receipt:", response.json())
    else:
        print("Error:", response.json())

notarize_document("./signed_contract.pdf", "YOUR_API_KEY_HERE")
```

-----

## 📚 API Reference

For detailed endpoint documentation, please refer to our [API Reference Guide](https://ubitquity.io/web3notary/API_Reference).

  * `POST /api/v1/notarize` - Anchor a document hash.
  * `GET /api/v1/verify/{documentHash}` - Verify a document hash and retrieve the blockchain transaction.
  * `POST /api/v1/ipfs/store` - (Optional) Store an encrypted payload on IPFS.

-----

## 🔒 Security & Best Practices

1.  **Never send unencrypted PII or legal documents** directly to the API. Only send cryptographic hashes.
2.  If utilizing the IPFS endpoint, ensure the document is encrypted locally using a strong standard (e.g., AES-256) before uploading. The IPFS network is public by nature.
3.  Store your API keys securely in environment variables. Do not hardcode them into front-end clients.

## 🤝 Support

Having trouble integrating? Our developer success team is here to help.

  * **Support Ticket:** [Support Ticket](https://support.ubitquityx.com)
  * **Telegram:** Join our [Developer Community](https://t.me/ndaoofficial/13225)
