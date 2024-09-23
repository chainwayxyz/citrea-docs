# RPC Documentation of Citrea Ledger

This documentation explores main ledger RPC endpoints of Citrea that are relevant to batches & proofs:

<details>
<summary><code>citrea_syncStatus</code></summary>

This endpoint retrieves the current synchronization status of your local Citrea node.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "citrea_syncStatus",
        "params": [], 
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[], "id":78}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": {
            "Syncing": {
                "head_block_number": 264052,  // The latest block number known to the node
                "synced_block_number": 27050  // The block number up to which the node has synced
            }
        },
        "id": 31
    }
    ```

### Response Fields Explanation

- `Syncing`: The synchronization status object.
  - `head_block_number`: The latest block number known to the node.
  - `synced_block_number`: The block number up to which the node has synced.

</details>

<br>

<details>
<summary><code>ledger_getSoftBatchByHash</code></summary>

This endpoint retrieves the soft batch data for a given `hash`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.testnet.citrea.xyz`
- **Request Body:** You can change the hash below to the batch hash (a hexadecimal string) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftBatchByHash",
        "params": ["498586268de6f895a5bde5f7fc81ea16452f1ce53b266a2a09f48757046aff91"], 
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftBatchByHash","params":["498586268de6f895a5bde5f7fc81ea16452f1ce53b266a2a09f48757046aff91"], "id":31}'  https://rpc.devnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": {
            "da_slot_height": 7508,  // Data Availability slot height
            "da_slot_hash": "df00a605d3723c5ce827b59547f1d343eea847683dba89ac3fb10397f7000000",  // Hash of the DA slot
            "da_slot_txs_commitment": "0000000000000000000000000000000000000000000000000000000000000000",  // Commitment of DA slot transactions
            "hash": "498586268de6f895a5bde5f7fc81ea16452f1ce53b266a2a09f48757046aff91",  // Hash of the soft batch
            "txs": [
                "884b23b8740cfa84a8d05ce0c5ca454a1d017bf2ab891b212643b2feb8f7d550e201afd8fe2b2abca89b6bc4997c514c21f3d4812973f14c9fe2a2bcfd3c6f0f52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef17405000000010000000000000000000000001200000000000000"
            ],  // List of transactions in the soft batch
            "pre_state_root": "2d3ebe41f2115a2ad81a68e8179b5f0845ca3a948ac6a2f4209f3bcd35f6d0c3",  // Pre-state root hash
            "post_state_root": "c9ff9bc80c5c55a9bcbb08ea2764f9bc6c5d3fb8237e0064caa1412dcea577bf",  // Post-state root hash
            "soft_confirmation_signature": "6a4ea116ac95899720c35e34f5aed46bf6e4cb04ddbb4077aec64c365cd1566278df5f7b2e4e01e1e0fee17fd408ab1e35c7c96ed7b0501ec3eb29f869031b00",  // Signature for soft confirmation
            "pub_key": "52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174",  // Public key associated with the signature
            "deposit_data": [],  // Data related to deposits (empty if none)
            "l1_fee_rate": 5000000000,  // Layer 1 fee rate
            "timestamp": 1717229214  // Timestamp of the batch
        },
        "id": 31
    }
    ```

</details>

<br>

<details>
<summary><code>ledger_getSoftBatchByNumber</code></summary>

This endpoint retrieves the soft batch data for a given `batch_id`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.devnet.citrea.xyz`
- **Request Body:** You can change the number below to the batch ID (a decimal number) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftBatchByNumber",
        "params": [5], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftBatchByNumber","params":[5], "id":1}'  https://rpc.devnet.citrea.xyz
    ```


### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0", 
        "result": {
            "da_slot_height": 7505,  // DA slot height (i.e. block) that the batch has been put into
            "da_slot_hash": "3ad4f4081dfb1af682c5a847b277913c0af602ae6aa7101155989e2dcc020000",  // Hash of the DA slot
            "da_slot_txs_commitment": "0000000000000000000000000000000000000000000000000000000000000000",  // Commitment of DA slot transactions
            "hash": "b3a258802a313dcb4bef61e735c91e63c30635b2850913c056a1c446b5daf6ea",  // Hash of the soft batch
            "txs": [
                "41885bba544e6501b34bc232c11579d3462bc245fc1be492eb74a8ef95370787a3683189fb24c9b5fc921b1439218c5c729ffbb674f6d845230d1fc4bc417c0152f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef17405000000010000000000000000000000000400000000000000"
            ],  // List of transactions in the soft batch
            "pre_state_root": "682c17d29df51d4c4810542296944d4d6700b56b151ff6db2131ec4eb11ccff2",  // Pre-state root hash
            "post_state_root": "6b671281db95fdf5a2570146abb7d80d1a1c8179467d8a914a018e75085afa59",  // Post-state root hash
            "soft_confirmation_signature": "dc21cffadf0852e948431c9ead1063e3b8518ddab285f11edc2de5834a35f18525bdb409f1a24bf542cc7c4bffc2777486319b5b350d43a4b3c571e2ab342504",  // Signature for soft confirmation
            "pub_key": "52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174",  // Public key associated with the signature
            "deposit_data": [],  // Data related to deposits (empty if none)
            "l1_fee_rate": 5000000000,  // Layer 1 fee rate
            "timestamp": 1717229186  // Timestamp of the batch
        },
        "id": 1
    }
    ```
</details>

<br>

<details>
<summary><code>ledger_getSoftBatchRange</code></summary>

This endpoint retrieves a range of soft batch data for a given `start` and `end` batch ID.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.devnet.citrea.xyz`
- **Request Body:** You can change the numbers below to the start and end batch IDs (decimal numbers) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftBatchRange",
        "params": [17, 19], 
        "id": 42
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftBatchRange","params":[17, 19], "id":42}'  https://rpc.devnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": [
            {
                "da_slot_height": 7508,  // Data Availability slot height
                "da_slot_hash": "df00a605d3723c5ce827b59547f1d343eea847683dba89ac3fb10397f7000000",  // Hash of the DA slot
                "da_slot_txs_commitment": "0000000000000000000000000000000000000000000000000000000000000000",  // Commitment of DA slot transactions
                "hash": "0d5da08e4ca04c62565521539f939640cb87e9032a5048c4a1bcfd3b28d35786",  // Hash of the soft batch
                "txs": [
                    "8539abf43ebf628c214a998a7f44ad88a7bf6dfe69d2f139f034b9401226160877690963228fa38689023703a2763cb0f84a41f22b14aacba1216c888404990c52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef17405000000010000000000000000000000001000000000000000"
                ],  // List of transactions in the soft batch
                "pre_state_root": "86f81230f33f50b4a6aa7c813f77fa43a790a232ca6cc2913e560dbcceefb062",  // Pre-state root hash
                "post_state_root": "a295329682d0f8f51e808f6e4e0a9dde30d6ddef6fe87f85b7ac42db8ec64f30",  // Post-state root hash
                "soft_confirmation_signature": "b7aa3c71eab4af87ec3dbde18b1ef7a0160d64b691e79c36628a6b049c8ab1a35ea210ed26af449d3726a669dc2ff6077bfe0a785bad6fddecca2f39883e1b04",  // Signature for soft confirmation
                "pub_key": "52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174",  // Public key associated with the signature
                "deposit_data": [],  // Data related to deposits (empty if none)
                "l1_fee_rate": 5000000000,  // Layer 1 fee rate
                "timestamp": 1717229210  // Timestamp of the batch
            },
            {
                "da_slot_height": 7508,  // Data Availability slot height
                "da_slot_hash": "df00a605d3723c5ce827b59547f1d343eea847683dba89ac3fb10397f7000000",  // Hash of the DA slot
                "da_slot_txs_commitment": "0000000000000000000000000000000000000000000000000000000000000000",  // Commitment of DA slot transactions
                "hash": "682bd9560d6fd0ee423915dbfbdb2cddfd816e82213575ed7e45a95261545fdc",  // Hash of the soft batch
                "txs": [
                    "faa306d22b5c1bd11b3e2c387e336032b3a1757348a9a6b04820e288952a0719ee651a040bf9e08522e90aad96ac4bfdd2577d98c252965b13605496313a030852f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef17405000000010000000000000000000000001100000000000000"
                ],  // List of transactions in the soft batch
                "pre_state_root": "a295329682d0f8f51e808f6e4e0a9dde30d6ddef6fe87f85b7ac42db8ec64f30",  // Pre-state root hash
                "post_state_root": "2d3ebe41f2115a2ad81a68e8179b5f0845ca3a948ac6a2f4209f3bcd35f6d0c3",  // Post-state root hash
                "soft_confirmation_signature": "0692b3566c2ac9fa50c7804d0eca37ea5db57534614f34d1b928dd73ab348632d18596ed60c7aa33e8421dafe0b0d05be30ec5c3b128e5cab66b031997c02004",  // Signature for soft confirmation
                "pub_key": "52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174",  // Public key associated with the signature
                "deposit_data": [],  // Data related to deposits (empty if none)
                "l1_fee_rate": 5000000000,  // Layer 1 fee rate
                "timestamp": 1717229212  // Timestamp of the batch
            },
            // ... remaining batches
        ],
        "id": 42
    }
    ```
</details>

<br>

<details>
<summary><code>ledger_getSoftConfirmationStatus</code></summary>

This endpoint retrieves the soft confirmation status for a given `l2_height`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.devnet.citrea.xyz`
- **Request Body:** You can change the number below to the L2 height (a decimal number) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftConfirmationStatus",
        "params": [5], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftConfirmationStatus","params":[5], "id":1}'  https://rpc.devnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": "Trusted",  // Possible values: "Trusted", "Finalized", "Proven"
        "id": 1
    }
    ```

### Response Fields Explanation

- `result`: The soft confirmation status of the batch. Possible values are:
  - `Trusted`: No confirmation yet, rely on the sequencer.
  - `Finalized`: The soft confirmation has been finalized with a sequencer commitment.
  - `Proven`: The soft batch has been ZK-proven.
</details>

<br>

<details>
<summary><code>ledger_getSequencerCommitmentsOnSlotByNumber</code></summary>

This endpoint retrieves the sequencer commitments for a given `height`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.devnet.citrea.xyz`
- **Request Body:** You can change the number below to the slot number (a decimal number) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSequencerCommitmentsOnSlotByNumber",
        "params": [10002], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByNumber","params":[5], "id":1}'  https://rpc.devnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**: `result` field will be `null` if no sequencer commitment is available in that slot.
    ```json
    {
        "jsonrpc": "2.0",
        "result": [
            {
                "found_in_l1": 7505,  // L1 block hash the commitment was on
                "merkle_root": "fb0499ec07f2126ea6acc9aa3fd3dd08f0f2b60444cc42a99b932cfc1eb40744",  // Hex encoded Merkle root of soft confirmation hashes
                "l1_start_block_hash": "bfbcddf30b2df1b7395f69295aecbbc059ebc6cd807c707f6dac3672ab020000",  // Hex encoded Start L1 block's hash
                "l1_end_block_hash": "0ae73abf5564e3d8fcfaa9fc8d892d03b901fc275e2a65684c0ee35a85010000"  // Hex encoded End L1 block's hash
            }
        ],
        "id": 1
    }
    ```

### Response Fields Explanation

- `found_in_l1`: L1 block hash the commitment was on.
- `merkle_root`: Hex encoded Merkle root of soft confirmation hashes.
- `l1_start_block_hash`: Hex encoded Start L1 block's hash.
- `l1_end_block_hash`: Hex encoded End L1 block's hash.
</details>

<br>

<details>
<summary><code>ledger_getSequencerCommitmentsOnSlotByHash</code></summary>

TODO
</details>

<br>

<details>
<summary><code>ledger_getVerifiedProofsBySlotHeight</code></summary>

This endpoint retrieves the verified proofs for a given `height` of a DA slot.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.devnet.citrea.xyz`
- **Request Body:** You can change the number below to the slot height (a decimal number) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getVerifiedProofsBySlotHeight",
        "params": [37763], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
      curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getVerifiedProofsBySlotHeight","params":[37763], "id":31}'  https://rpc.devnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": [
            {
                "proof": {
                    "type": "Full",  // Type of proof, can be "PublicInput" or "Full"
                    "data": "0200000000010000000000002d1f33d..."  // Very long encoded proof data
                },
                "state_transition": {
                    "initial_state_root": "97ac1d78a79867afae5eadcab52374dbc0790fb2bb1890483d34c3835fefcef8",  // Hex encoded initial state root
                    "final_state_root": "ba1bc2a9fb986f06b2c6a8d440e5395279222bb894fc4a364685a34c5978a1b6",  // Hex encoded final state root
                    "state_diff": {
                        "6369747265615f65766d2f45766d2f6163636f756e74732f14deaddeaddeaddeaddeaddeaddeaddeaddeaddead":"2000384756823452345...", 
                        "6369747265615f65766d2f45766d2f6163636f756e74732f3100000000000000000000000000000000000001202ac301ded24d1f6c1616eec2c8b4820b1a9384c5ea477271e5dea3a0c9fb2705":"20893cbf1129aab84a5272c01b268d3c04fbbabc9f799857aa740d3fb88b020000",
                        "6369747265615f65766d2f45766d2f6163636f756e74732f3100000000000000000000000000000000000001208231cfcdc1741e3a9ef98967e4b98d1cfb0a978b985c94cf8497878e57ec2394":"200000000000000000000000000000000000000000000000000000000000000000",
                        "6369747265615f65766d2f45766d2f6c61746573745f626c6f636b5f6861736865732f200000000000000000000000000000000000000000000000000000000000027234":null,
                        "6369747265615f65766d2f45766d2f6c61746573745f626c6f636b5f6861736865732f200000000000000000000000000000000000000000000000000000000000027235":null,
                        "6369747265615f65766d2f45766d2f6c61746573745f626c6f636b5f6861736865732f200000000000000000000000000000000000000000000000000000000000027236":null,
                        // and more...
                    },
                    "da_slot_hash": "3c620806a2cf3ba3c136dcf7ae7794555c9bea6621174144c67625de23010000",  // Hex encoded DA slot hash
                    "sequencer_public_key": "52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174",  // Hex encoded sequencer public key
                    "sequencer_da_public_key": "039cd55f9b3dcf306c4d54f66cd7c4b27cc788632cd6fb73d80c99d303c6536486",  // Hex encoded sequencer DA public key
                    "validity_condition": "3d3aa72f5435d9cee6c938dfa9c4cea918d5c2c9635b0bbef588aa67920000003c620806a2cf3ba3c136dcf7ae7794555c9bea6621174144c67625de23010000"  // Hex encoded validity condition
                }
            }
            // More verified proof responses if available
        ],
        "id": 1
    }
    ```

### Response Fields Explanation

- `proof`: The proof data.
  - `type`: Type of proof, can be `PublicInput` or `Full`.
  - `data`: Hex encoded proof data.
- `state_transition`: The state transition data.
  - `initial_state_root`: Hex encoded initial state root.
  - `final_state_root`: Hex encoded final state root.
  - `state_diff`: State diff of L2 blocks in the processed sequencer commitments.
  - `da_slot_hash`: Hex encoded DA slot hash.
  - `sequencer_public_key`: Hex encoded sequencer public key.
  - `sequencer_da_public_key`: Hex encoded sequencer DA public key.
  - `validity_condition`: Hex encoded validity condition.
</details>
