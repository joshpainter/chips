CHIP Number   | < Creator must leave this blank. Editor will assign a number.>
:-------------|:----
Title         | DID1 Standard
Description   | A standard for implementing decentralized identifiers (DIDs) on Chia's blockchain
Author        | [todo]
Comments-URI  | < Creator must leave this blank. Editor will assign a URI.>
Status        | < Creator must leave this blank. Editor will assign a status.>
Category      | Standards Track
Sub-Category  | Chialisp
Created       | 2022-06-05
Requires      | [Singleton Standard](https://chialisp.com/docs/puzzles/singletons "Chia's Singleton Standard (pre-CHIP)")
Replaces      | None
Superseded-By | None

## Abstract
Chia Decentralized Identifiers (DIDs) enable on-chain proof of identity. The owner of a Chia DID can be any entity, such as a person, a company, an electronic device, etc. Chia DIDs can be associated with the owner's wallet, an NFT marketplace or any other aspect of Chia's ecosystem. They are stored on Chia's decentralized blockchain. They are also programmable, transferable, recoverable and discoverable. Anyone using Chia's blockchain may create a Chia DID to represent themselves or their organization.

## Definitions
Throughout this document, we'll use the following terms:
* **Must, required, shall** – These words indicate an absolute requirement of the specification
* **Must not, shall not** – These phrases indicate an absolute prohibition of the specification
* **Should, recommended** – These words indicate something that is not a requirement of the specification, but the implications of not following it should be carefully considered beforehand
* **Should not, not recommended** – These phrases indicate something that is not a prohibition of the specification, but the implications of following it should be carefully considered beforehand
* **May** - This word indicates something that is optional. Interoperability between implementations must not be broken because of the choice to implement, or not to implement, this feature

## Motivation
Humans, organizations and even devices should have total control over their personal information. This includes ownership of data, as well as the ability to decide which aspects of their identities are shared, and with whom. Unfortunately, we live in a world where personal information is routinely shared without the owner's knowledge or consent. Chia DIDs attempt to solve this.

Most people and organizations possess multiple forms of identification. Examples include passports, national ID cards, company registration numbers, social security numbers, email addresses, phone numbers and barcodes. These identifiers are globally unique -- they cannot be reused by anyone else. They are also issued by a central authority, such as a government or a company; the data contained within is controlled using opaque, closed-source algorithms.

Because of their centralized issuance and control, the identifiers listed above come with many downsides, such as:
* Their owners have little control over the private or sensitive information they contain. This information could be shared without the owner's consent, such as when a credit bureau sells a customer's information
* Their owners are often denied access to their own data, creating an asymmetry of information
* They can be revoked against their owner's will
* They might cease to exist if their issuing entity fails
* They are limited in scope
  * A national ID card has little value outside of the country from which it was issued
* They are not interoperable
  * Alice cannot log into two competing social networks using the same identifier. Even if she uses the same email address, username and password for both networks, the underlying identifiers will be different. Each network will store a different history and reputation for Alice

Some identifiers, such as usernames on accounts, can be reused, even by different people. They also exist in walled gardens. For example, identities are not typically transferable between competing social networks.

All of these downsides are solved by Chia DIDs:
* They are not issued by central authorities, nor are any aspects of Chia DIDs controlled by anyone but their owner
* Their owners have total control over all information contained within, including any private or sensitive details. This information may not be shared without their owner's consent
* They cannot be revoked against their owner's will
* They cannot be destroyed, as long as Chia's blockchain is operational
* They can have unlimited scope
  * Chia DIDs can be used by any marketplace that implements the open-source standard laid out in this document
* They can be interoperable
  * Different companies or entities could accept Chia DIDs, even if they were competitors

Use cases for Chia DIDs include, but are not limited to:
* Cryptographically proving identity online
* Enabling marketplaces that are not walled gardens
* Enabling self-custody of digital and physical assets
* Enabling the designation of trusted entities for M-of-N wallet recovery in cases where the owner loses their original credentials
* Grouping multiple assets together into collections
* Enabling identifiers that any blockchain can reference, thus allowing for interoperability between networks
* Supporting rate-limited wallets, as well as claw backs
* Tracing provenance of NFTs and other digital assets

Chia DIDs require singletons, which have already been implemented and are documented on [chialisp.com](https://chialisp.com/docs/puzzles/singletons/).

## Backwards Compatibility
DID1 does not introduce any breaking changes to Chia.

## Rationale
Chia's blockchain is public and open source. It features strong privacy controls, as well as the sandboxing of data. These features make Chia's blockchain a natural platform on which to implement DIDs.

This section will discuss alternative designs for DIDs and why they were not chosen. It will also discuss the influence for the structure of Chia DIDs, as well as the design decisions that were made.

### Alternative designs  
Thus far, every existing DID has come from a company or a consortium. Almost all of these DIDs are deployed on private blockchains. These implementations defeat the purpose of DIDs:
* By definition, private blockchains are under the control of a central authority
* A private blockchain's controlling authority has the power to terminate the chain, and any data contained within, including DIDs
* If a private blockchain's controlling authority either goes bankrupt or is sold, their blockchain can also be terminated
* A DID on a private blockchain could be censored, even for mundane reasons. For example, if a DID's owner opens an account on a competing blockchain, the DID could be revoked
* Existing DID implementations only provide authentication and authorization. Certain desirable features of DIDs, such as rate-limited wallets and claw backs, are not congruent with a private blockchain

Chia's blockchain does not suffer any of the centralization issues outlined above. The blockchain is public and decentralized; no single entity has control over it.

### Design influence

Several aspects of Chia's blockchain and programming environment influenced our design decisions for Chia DIDs:

* **The coin set model:** Most blockchains with a strong on-chain programming environment use an account model, which implements a centralized design pattern: Multiple users interact with a fixed smart contract on the blockchain.

  Chia's [coin set model](https://docs.chia.net/docs/04coin-set-model/intro/) is decentralized by design -- each user can only spend their own smart coins. Coins can only interact with other coins by using announcements. These design patterns create strong sandboxing, while maintaining each of the central tenets of DIDs
 
* **Chialisp:** [Chialisp](https://chialisp.com/) is Chia's Turing-complete on-chain programming language. Like all Lisp variants, Chialisp can be represented as a binary tree. This design allows for an arbitrary amount of data to be used with each program, contained in a single tree hash. A DID's owner can therefore choose whatever information they want to be included in their DID. Chialisp also enables strong composability and interoperability between coins, creating the potential for nearly limitless customizable features
 
* **BLS signatures:** Chia uses [BLS signatures](https://docs.chia.net/docs/09keys/keys-and-signatures/), which have built-in capability for M of N multisig. This will allow Chia DIDs to be shared between multiple parties, and only allow transactions to occur if a threshold of signatures has been reached. BLS signatures also enable recovery of a lost key with M of N trusted signatures
 
* **Offers:** Requests for credentials could be incorporated into [offers](https://chialisp.com/docs/puzzles/offers/), which are partially signed transactions with no central intermediary required for completion

### Design decisions
* Each DID that follows the DID1 standard is required to be implemented as a singleton
* Because DIDs are singletons, a spend of at least 1 mojo is required to register a DID on-chain
* Each DID must contain the owner's public key
* For recovery, each DID should contain a hash of a list of trusted DIDs. If recovery is not a desired feature, then a placeholder may be used instead
* The trusted DID list may contain an unbounded number of entries
* DIDs must support transaction fees upon transferal
* DIDs should not contain any personally identifiable data
* DIDs must comply with the JSON-LD format

## Specification
### Key Features
* Usability
  * A DID must contain a hash of the owner’s public key
  * A DID owner may create more than one DID using the same public/private key pair
  * A DID shall not have exclusive control over a public/private key pair
  * A DID shall not own another DID
  * DIDs may own multiple NFTs
  * The DID hash may be shared as a means to verify identity
  * The DID hash must be made discoverable by blockchain explorers
  * DIDs must be able to discover the assets they own
  * DID owners may provide their DIDs to issuers to be signed as Verifiable Credentials (VCs), thus allowing VCs to be issued to those DIDs
  * DID owners must be able to view and copy their DID ID
  * DID owners must be able to view the metadata associated with their DIDs
  * DID owners must be able to view all DIDs owned by their root key

* Wallets
  * Wallets that access Chia's blockchain may implement DID1
  * Wallets that implement DID1 must allow users to create DIDs
  * Wallets that implement DID1 must be able to display DIDs for users to view, copy and share
  * DID owners must be able to add their DID(s) to any wallets that implement DID1
  * DID owners must be able to customize the names of their DIDs to be shown by the wallet UI. These custom names are not stored on chain
  * DIDs must be made discoverable by wallets that implement DID1
  * Wallets that implement DID1 are recommended to support fees for transactions made with DIDs
  * Wallets that implement DID1 must support showing a list of all NFTs owned by the DID to the DID’s owner

* Offers
  * DID owners may include their credentials when acting as the Maker in an offer of XCH,Chia Asset Tokens (CATs) or NFTs
  * Chia offers may include a demand for DID credentials from any potential Takers

* Transferal
  * A DID's owner must be able to transfer their DID to another public key
  * Transfer must occur by updating the public key associated with the DID
  * All assets owned by a DID must be transferable
  * Upon a DID's transfer, all assets owned by that DID must also be transferred

* Recovery
  * A DID's owner must be able to update the trusted DID list at any time. The list is an optional feature; a placeholder may be used instead. However, in order for recovery to occur, the list is required
  * Of the N DIDs in the trusted list, the DID's owner must be able to set M DIDs for recovery. For example, if the list contains five DIDs, the owner may require three of the DIDs for recovery
  * A DID's owner must be able to set and trigger the recovery process for their DID
  * A DID's owner must be able to recover their DID on a different computer than the one on which it was created

* Upgradeability
  * DIDs using the DID1 specification must be upgradeable to include features of future DID specifications

* Marketplace custody
  * Marketplaces may custody DIDs on their owners' behalf
  * Marketplaces must obtain permission from a DID's owner before custody is transferred
  * This consent may not be withdrawn once given. The owner must trust the marketplace to return the DID upon request

### Inner Puzzle
This puzzle sits inside the singleton layer and provides the functionality related to being an identity. It is located in GitHub, under [chia-blockchain/blob/main_dids/chia/wallet/puzzles/did_innerpuz.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/did_innerpuz.clvm).

Each DID's inner puzzle must curry the following arguments into its solution:
* **INNER_PUZZLE:** A standard "pay-to" (p2) puzzle, used to record ownership of the DID
* **RECOVERY_DID_LIST_HASH:** The hash of the list of DIDs that can be used to recover this DID by sending this DID a message. A hash is stored instead of the full list in order to avoid revealing the whole list every time the singleton is spent
* **NUM_VERIFICATIONS_REQUIRED:** The number of DIDs from the above list that are required for recovery
* **SINGLETON_STRUCT:** Defines the singleton used with this DID. Contains the following structure
  * ((SINGLETON_MOD_HASH, (LAUNCHER_ID, LAUNCHER_PUZZLE_HASH))), where:
    * **SINGLETON_MOD_HASH** is the tree hash of [singleton_top_layer_v1_1.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/chia/wallet/puzzles/singleton_top_layer_v1_1.clvm) without any curried arguments
    * **LAUNCHER_ID** is the coin_id of the singleton used with this DID
    * **LAUNCHER_PUZZLE_HASH** is the tree hash of [singleton_launcher.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/chia/wallet/puzzles/singleton_launcher.clvm), which creates the singleton coin and announcement
  * **METADATA:** Customizable metadata, e.g. Know Your Customer (KYC) info

Additionally, each DID's inner puzzle must include the following solution upon initiating a message creation, recovery or self-destruct action:
* **mode:** The type of spend. Choose from
  * 0 = Recovery
  * 1 = Run INNER_PUZZLE with p2_solution
* **my_amount_or_inner_solution:**
  * In mode 0, this is an amount. Recover this coin and assert its amount
  * In mode 1, this is the solution to the inner p2 puzzle
* **new_inner_puzhash:** In recovery mode, this will be the new wallet DID puzzle hash
* **parent_innerpuzhash_amounts_for_recovery_ids:** A list of DIDs, each of which contains a complete list of arguments, to be used for recovery
* **pubkey:** The new public key used for a recovery
* **recovery_list_reveal:** This is the list of DIDs that had previously been approved for recovery. The hash of this list must equal RECOVERY_DID_LIST_HASH
* **my_id:** This coin's coin_id

### RPC Calls

**did_set_wallet_name**

Functionality: Set the name of a DID wallet

Usage: chia rpc wallet [OPTIONS] did_set_wallet_name [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data
| -h            | --help       | None | False    | Show a help message and exit

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet on which to set the name |
| name      | True     | The new name of the DID wallet |

**did_get_wallet_name**

Functionality: Given a DID wallet's ID, retrieve the name of that wallet

Usage: chia rpc wallet [OPTIONS] did_get_wallet_name [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet on which to get the name |


**did_update_recovery_ids**

Functionality: Append one or more IDs to be used for recovery of a DID wallet. The current list can be obtained with the did_get_recovery_list endpoint

Usage: chia rpc wallet [OPTIONS] did_update_recovery_ids [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter                  | Required | Description |
|:---------------------------|:---------|:------------|
| wallet_id                  | True     | The Wallet ID of the DID wallet for which to update the recovery IDs |
| new_list                   | True     | The new recovery ID list. Each item from this list will be appended to the existing list |
| num_verifications_required | False    | Optionally set the number of IDs required for wallet recovery. If not set, then the entire updated list will be required by default |


**did_update_metadata**

Functionality: Update the metadata for a DID wallet. The current metadata can be obtained with the did_get_metadata endpoint

Usage: chia rpc wallet [OPTIONS] did_update_metadata [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet for which to update the metadata |
| metadata  | True     | The updated metadata |


**did_get_did**

Functionality: Fetch the my_did and coin_id (if applicable) settings for a given wallet

Usage: chia rpc wallet [OPTIONS] did_get_did [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet for which to get the DID info |


**did_get_recovery_list**

Functionality: For a given wallet, fetch the recovery list, as well as the number of IDs required for recovery

Usage: chia rpc wallet [OPTIONS] did_get_recovery_list [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet for which to get the recovery list |


**did_get_metadata**

Functionality: Fetch the metadata for a given wallet

Usage: chia rpc wallet [OPTIONS] did_get_metadata [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet for which to get the metadata list |


**did_recovery_spend**

Functionality: Recover a DID [todo explain better]

Usage: chia rpc wallet [OPTIONS] did_recovery_spend [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter   | Required | Description |
|:------------|:---------|:------------|
| wallet_id   | True     | The Wallet ID of the DID wallet to recover |
| attest_data | True     | A list of attest files to be used for recovery |
| pubkey      | False    | The public key of the wallet to recover. If this is not provided, a temporary public key will be used instead |
| puzhash     | False    | The puzzle hash of the wallet to recover. If this is not provided, a temporary puzzle hash will be used instead |


**did_get_pubkey**

Functionality: Get the public key for a DID

Usage: chia rpc wallet [OPTIONS] did_get_pubkey [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet from which to obtain the public key |


**did_create_attest**

Functionality: Create an attest for a DID, to be used for recovery. This command will output the attest data, which can then be added or redirected to a file

Usage: chia rpc wallet [OPTIONS] did_create_attest [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID for which to create the attest |
| coin_name | True     | The coin to use for the attest |
| pubkey    | True     | The public key to use for the attest |
| puzhash   | True     | The puzzle hash to use for the attest |


**did_get_information_needed_for_recovery**

Functionality: Display all relevant information needed to recover a given DID

Usage: chia rpc wallet [OPTIONS] did_get_information_needed_for_recovery [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet from which to obtain the recovery information |


**did_get_current_coin_info**

Functionality: Get the current coin info (parent coin, puzzle hash, amount) for a DID wallet

Usage: chia rpc wallet [OPTIONS] did_get_current_coin_info [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet from which to obtain the coin info |


**did_create_backup_file**

Functionality: Output the backup data of a DID wallet's metadata. This output can then be saved or redirected to a file

Usage: chia rpc wallet [OPTIONS] did_create_backup_file [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data |
| -h            | --help       | None | False    | Show a help message and exit |

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet from which to obtain the coin info |


**did_transfer_did**

Functionality: Transfer a DID

Usage: chia rpc wallet [OPTIONS] did_transfer_did [REQUEST]

Options:

| Short Command | Long Command | Type | Required | Description |
|:--------------|:-------------|:-----|:---------|:------------|
| -j            | --json-file  | TEXT | False    | Instead of REQUEST, provide a json file containing the request data
| -h            | --help       | None | False    | Show a help message and exit

Request Parameters:

| Parameter | Required | Description |
|:----------|:---------|:------------|
| wallet_id | True     | The Wallet ID of the DID wallet to transfer |
| inner_address | True | The address of the inner puzzle to which to transfer the DID |


## Test Cases
Test cases for Chia DIDs are located in the main_dids branch of the chia-blockchain GitHub repository, in the /tests/wallet/did_wallet folder:
* [test_did.py](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/tests/wallet/did_wallet/test_did.py)
* [test_did_rpc.py](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/tests/wallet/did_wallet/test_did_rpc.py)

Note: The main_dids branch will eventually be merged into main

## Reference Implementation
The reference implementation for Chia DIDs is located in the main_dids branch of the chia-blockchain GitHub repository, under [chia/wallet/did_wallet](https://github.com/Chia-Network/chia-blockchain/tree/main_dids/chia/wallet/did_wallet).

DIDs require the use of [singleton_launcher.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/chia/wallet/puzzles/singleton_launcher.clvm), [singleton_top_layer_v1_1.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main_dids/chia/wallet/puzzles/singleton_top_layer_v1_1.clvm) and [did_innerpuz.clvm](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/did_innerpuz.clvm).

## Security
[A security review is currently being conducted, the results of which will be posted here]

## Additional Assets
[Additional diagrams or assets not already referenced should be listed here]