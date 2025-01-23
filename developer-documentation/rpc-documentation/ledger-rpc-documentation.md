# RPC Documentation of Citrea Ledger

<!-- TODO: FIX CAMEL CASE LOL -->

This documentation explores main ledger RPC endpoints of Citrea that are relevant to batches & proofs:

<!-------------------------------------------------------------------->
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
        "id": 78
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[], "id":78}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    If synced fully:
    ```json
    {
        "jsonrpc": "2.0",
        "id": 78,
        "result": {
            "l1Status": {
                "Synced": 19224
            },
            "l2Status": {
                "Synced": 2326433
            }
        }
    }
    ```
    If still syncing:
    ```json
    {
        "jsonrpc": "2.0",
        "result": {
            "Syncing": {
                "headBlockNumber": 264052,
                "syncedBlockNumber": 27050
            }
        },
        "id": 78
    }
    ```

### Response Fields Explanation

- `Syncing`: The synchronization status object.
  - `headBlockNumber`: The latest block number known to the node.
  - `syncedBlockNumber`: The block number up to which the node has synced.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSoftConfirmationByNumber</code></summary>

This endpoint retrieves a specific soft confirmation by its number from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftConfirmationByNumber",
        "params": [78], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftConfirmationByNumber","params": [78], "id":1}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": {
        "l2Height": 78,
        "daSlotHeight": 45496,
        "daSlotHash": "58e6a5aa69520460dca3d6e1c8c2f4f362a032a066439f3062405bc300000000",
        "daSlotTxsCommitment": "f825fc909215f67698399adad8c48c7574ed9845a7c9e12bd1231489ab3551e1",
        "hash": "3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2",
        "prevHash": "d160b04f32aa629658b28e648b78c4ebfbb04fefb3100fb16fd7dda11f03afeb",
        "txs": [],
        "stateRoot": "c2d1060a6b3224f66815fb5c53e2fe25c45eb3a5aa546d4e2ca6a5e6987efa0a",
        "softConfirmationSignature": "c05f6f1bee89ed9473e4b8caac36a7de6069dccf5ff2b499ff0609de974475bc05837d62139d574b9be4be26b82fef4ceb648b301d75dc136adcb73b6f05840f",
        "pubKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
        "depositData": [],
        "l1FeeRate": 2500000000,
        "timestamp": 1726584752
      }
    }
    ```

### Response Fields Explanation

- `l2Height`: The L2 height (block number) of the soft confirmation.
- `daSlotHeight`: The Data Availability (DA) slot height associated with this soft confirmation.
- `daSlotHash`: The hash of the Data Availability (DA) slot.
- `daSlotTxsCommitment`: The transaction commitment of the Data Availability (DA) slot.
- `hash`: The hash of the soft confirmation itself.
- `prevHash`: The hash of the preceding soft confirmation.
- `txs`: An array of transactions included in this soft confirmation (currently empty in this example).
- `stateRoot`: The state root after processing this soft confirmation.
- `softConfirmationSignature`: The signature of the soft confirmation by the sequencer.
- `pubKey`: The sequencer's public key used to sign the soft confirmation.
- `depositData`: Data related to deposits from the L1 chain (currently empty in this example).
- `l1FeeRate`: The L1 fee rate at the time of this soft confirmation.
- `timestamp`: The timestamp of the soft confirmation.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSoftConfirmationByHash</code></summary>

This endpoint retrieves a specific soft confirmation by its hash from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftConfirmationByHash",
        "params": ["3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2"],
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftConfirmationByHash","params": ["3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2"], "id":1}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": {
        "l2Height": 78,
        "daSlotHeight": 45496,
        "daSlotHash": "58e6a5aa69520460dca3d6e1c8c2f4f362a032a066439f3062405bc300000000",
        "daSlotTxsCommitment": "f825fc909215f67698399adad8c48c7574ed9845a7c9e12bd1231489ab3551e1",
        "hash": "3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2",
        "prevHash": "d160b04f32aa629658b28e648b78c4ebfbb04fefb3100fb16fd7dda11f03afeb",
        "txs": [],
        "stateRoot": "c2d1060a6b3224f66815fb5c53e2fe25c45eb3a5aa546d4e2ca6a5e6987efa0a",
        "softConfirmationSignature": "c05f6f1bee89ed9473e4b8caac36a7de6069dccf5ff2b499ff0609de974475bc05837d62139d574b9be4be26b82fef4ceb648b301d75dc136adcb73b6f05840f",
        "pubKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
        "depositData": [],
        "l1FeeRate": 2500000000,
        "timestamp": 1726584752
      }
    }
    ```

### Response Fields Explanation

- `l2Height`: The L2 height (block number) of the soft confirmation.
- `daSlotHeight`: The Data Availability (DA) slot height associated with this soft confirmation.
- `daSlotHash`: The hash of the Data Availability (DA) slot.
- `daSlotTxsCommitment`: The transaction commitment of the Data Availability (DA) slot.
- `hash`: The hash of the soft confirmation itself.
- `prevHash`: The hash of the preceding soft confirmation.
- `txs`: An array of transactions included in this soft confirmation (currently empty in this example).
- `stateRoot`: The state root after processing this soft confirmation.
- `softConfirmationSignature`: The signature of the soft confirmation by the sequencer.
- `pubKey`: The sequencer's public key used to sign the soft confirmation.
- `depositData`: Data related to deposits from the L1 chain (currently empty in this example).
- `l1FeeRate`: The L1 fee rate at the time of this soft confirmation.
- `timestamp`: The timestamp of the soft confirmation.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSoftConfirmationRange</code></summary>

This endpoint retrieves a range of soft confirmations from the ledger, specified by their block numbers.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSoftConfirmationRange",
        "params": [78, 79], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftConfirmationRange","params": [78, 79], "id":1}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": [
        {
          "l2Height": 78,
          "daSlotHeight": 45496,
          "daSlotHash": "58e6a5aa69520460dca3d6e1c8c2f4f362a032a066439f3062405bc300000000",
          "daSlotTxsCommitment": "f825fc909215f67698399adad8c48c7574ed9845a7c9e12bd1231489ab3551e1",
          "hash": "3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2",
          "prevHash": "d160b04f32aa629658b28e648b78c4ebfbb04fefb3100fb16fd7dda11f03afeb",
          "txs": [],
          "stateRoot": "c2d1060a6b3224f66815fb5c53e2fe25c45eb3a5aa546d4e2ca6a5e6987efa0a",
          "softConfirmationSignature": "c05f6f1bee89ed9473e4b8caac36a7de6069dccf5ff2b499ff0609de974475bc05837d62139d574b9be4be26b82fef4ceb648b301d75dc136adcb73b6f05840f",
          "pubKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
          "depositData": [],
          "l1FeeRate": 2500000000,
          "timestamp": 1726584752
        },
        {
          "l2Height": 79,
          "daSlotHeight": 45496,
          "daSlotHash": "58e6a5aa69520460dca3d6e1c8c2f4f362a032a066439f3062405bc300000000",
          "daSlotTxsCommitment": "f825fc909215f67698399adad8c48c7574ed9845a7c9e12bd1231489ab3551e1",
          "hash": "89ff969ed52b8f04aa3a4b963cf8151e615fbb1e3e0b206fff117863d4309121",
          "prevHash": "3a0ff107c1605867e0d9b87e9d9e20141903957c9bd6ebeec96a63711936fdc2",
          "txs": [],
          "stateRoot": "b1d021697bc30166e7ef67d723bec665e0509f39c3340ec859b6ca05c2701ce6",
          "softConfirmationSignature": "d07a81206b6e5bf9260964e681c3e2b2882632c4427ebcb44aeda514de8e7b37c2ededee72c68c3c56207984118949bcf7cdb5056560043750becd673fa5560b",
          "pubKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
          "depositData": [],
          "l1FeeRate": 2500000000,
          "timestamp": 1726584755
        }
      ]
    }
    ```

### Response Fields Explanation

- The response is an array of soft confirmations, each corresponding to a block number within the requested range.
- Each soft confirmation object in the array contains the same fields as described in the `ledger_getSoftConfirmationByNumber` and `ledger_getSoftConfirmationByHash` endpoints, providing detailed information for each soft confirmation in the range.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSoftConfirmationStatus</code></summary>

This endpoint retrieves the soft confirmation status for a given `l2_height`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.testnet.citrea.xyz`
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
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSoftConfirmationStatus","params":[5], "id":1}'  https://rpc.testnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "result": "Finalized",
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

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getL2GenesisStateRoot</code></summary>

This endpoint retrieves the genesis state root of the L2 ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getL2GenesisStateRoot",
        "params": [],
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getL2GenesisStateRoot","params": [], "id":1}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": [
        5,
        24,
        63,
        175,
        36,
        133,
        127,
        15,
        166,
        212,
        167,
        115,
        143,
        229,
        239,
        20,
        183,
        235,
        232,
        139,
        224,
        246,
        110,
        111,
        135,
        244,
        97,
        72,
        85,
        84,
        213,
        49
      ]
    }
    ```

### Response Fields Explanation

- The response is a 32-byte array representing the genesis state root hash.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSequencerCommitmentsOnSlotByNumber</code></summary>

This endpoint retrieves the sequencer commitments for a given `height`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.testnet.citrea.xyz`
- **Request Body:** You can change the number below to the slot number (a decimal number) you want to query.
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSequencerCommitmentsOnSlotByNumber",
        "params": [55000], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByNumber","params":[55000], "id":1}'  https://rpc.testnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:** `result` field will be `null` if no sequencer commitment is available in that slot.
    ```json
    {
        "jsonrpc": "2.0",
        "result": [
            {
                "foundInL1": 55000,
                "merkleRoot": "c09408c5e7d81634e1d2e4080c0cc08d3ff801b0c362f2b438bd540fa6682b5d",
                "l2StartBlockNumber": 2809369,
                "l2EndBlockNumber": 2810368 
            }
        ],
        "id": 1
    }
    ```

### Response Fields Explanation

- `foundInL1`: L1 block number where the sequencer commitment is found.
- `merkleRoot`: Hex encoded Merkle root of soft confirmation hashes.
- `l2StartBlockNumber`: L2 block number where the sequencer commitment starts.
- `l2EndBlockNumber`: L2 block number where the sequencer commitment ends.
</details>

<br>


<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getSequencerCommitmentsOnSlotByHash</code></summary>

This endpoint retrieves the sequencer commitments for a given DA `hash`.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `https://rpc.testnet.citrea.xyz`
- **Request Body:** 
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getSequencerCommitmentsOnSlotByHash",
        "params": ["a65a1d15b08c518799c19e3123be7583ed8b5a287fe18a7848c39e7200000000"], 
        "id": 1
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByHash","params":["a65a1d15b08c518799c19e3123be7583ed8b5a287fe18a7848c39e7200000000"], "id":1}'  https://rpc.testnet.citrea.xyz
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:** `result` field will be `null` if no sequencer commitment is available in that slot.
    ```json
    {
        "jsonrpc": "2.0",
        "result": [
            {
                "foundInL1": 45866,
                "merkleRoot": "db9a3e1c0dedbdc1aa7856a9b7417c8c3bd7a5aaf66277d018afdd71a83d8c17",
                "l2StartBlockNumber": 48537,
                "l2EndBlockNumber": 49536 
            }
        ],
        "id": 1
    }
    ```

### Response Fields Explanation

- `foundInL1`: L1 block number where the sequencer commitment is found.
- `merkleRoot`: Hex encoded Merkle root of soft confirmation hashes.
- `l2StartBlockNumber`: L2 block number where the sequencer commitment starts.
- `l2EndBlockNumber`: L2 block number where the sequencer commitment ends.
</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getHeadSoftConfirmation</code></summary>

This endpoint retrieves the most recent (head) soft confirmation from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getHeadSoftConfirmation",
        "params": [],
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getHeadSoftConfirmation","params":[], "id":31}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 31,
      "result": {
        "l2Height": 5470829,
        "daSlotHeight": 66287,
        "daSlotHash": "ddcbf17902f373fba9809a9ec6c2f4f362a032a066439f3062405bc300000000",
        "daSlotTxsCommitment": "071b87a106d71efb7b665d32b9c72a7b2262140707f46c90fa4494aaa4e25aed",
        "hash": "a8dd8ec47b4a63829e122f814b5bfcbacebd511f1fe304d98e02d3eb7415be06",
        "prevHash": "05a6e2793a36325ccd1fc29fe2f523c11db4eaf4209a43d855cd42003370187c",
        "txs": [],
        "stateRoot": "198f22b82ac213545acbc981d02ebd1c48edd4400e7add2f9487ecd02308bdf2",
        "softConfirmationSignature": "a5336213c57b155243b475ef9ae00c3e133ecb39134ce1b76de01a751c054722f553a5dead19a0dae8e2ec43426e7a0e011fe7f105d761586905f7d355ce4d0e",
        "pubKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
        "depositData": [],
        "l1FeeRate": 2500000000,
        "timestamp": 1737602189
      }
    }
    ```

### Response Fields Explanation

- `l2Height`: The L2 height (block number) of the soft confirmation.
- `daSlotHeight`: The Data Availability (DA) slot height associated with this soft confirmation.
- `daSlotHash`: The hash of the Data Availability (DA) slot.
- `daSlotTxsCommitment`: The transaction commitment of the Data Availability (DA) slot.
- `hash`: The hash of the soft confirmation itself.
- `prevHash`: The hash of the preceding soft confirmation.
- `txs`: An array of transactions included in this soft confirmation (currently empty in this example).
- `stateRoot`: The state root after processing this soft confirmation.
- `softConfirmationSignature`: The signature of the soft confirmation by the sequencer.
- `pubKey`: The sequencer's public key used to sign the soft confirmation.
- `depositData`: Data related to deposits from the L1 chain (currently empty in this example).
- `l1FeeRate`: The L1 fee rate at the time of this soft confirmation.
- `timestamp`: The timestamp of the soft confirmation.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getHeadSoftConfirmationHeight</code></summary>

This endpoint retrieves the L2 height of the most recent (head) soft confirmation from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getHeadSoftConfirmationHeight",
        "params": [],
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getHeadSoftConfirmationHeight","params":[], "id":31}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 31,
      "result": 5470849
    }
    ```

### Response Fields Explanation

- The response is a number representing the L2 height (block number) of the head soft confirmation.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->


<details>
<summary><code>ledger_getVerifiedBatchProofsBySlotHeight</code></summary>

This endpoint retrieves the last verified batch proof from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getVerifiedBatchProofsBySlotHeight",
        "params": [55555],
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getVerifiedBatchProofsBySlotHeight","params":[55555], "id":31}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 31,
      "result": {
        "proof": {
          "proof": [
            2,
            0,
            1,
            215,
            ... // very long response
          ],
          "proofOutput": {
            "initialStateRoot": "3a0cb3797428e996e3dccf890e9a8fbb07c95745e4b72d83f8b7ac299804c43f",
            "finalStateRoot": "b8a4a101d199164db83fc6a3a49c6bf9face9539e485a24948dadd785fb5d1e4",
            "prevSoftConfirmationHash": "0000000000000000000000000000000000000000000000000000000000000000",
            "finalSoftConfirmationHash": "0000000000000000000000000000000000000000000000000000000000000000",
            "stateDiff": {
              "4163636f756e74732f6163636f756e74732f4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1": "eb9593515dd32b28a13c40fa96f6320fdd762aa6cfc2c6a8b6543b6a3ba40e346ab1050000000000","45766d2f612f1403c036ffb7dfb6b6d19f763948b8fb8faccaa529": "200000000000000000000000000000000000000000000000000011c37937e08000000000000000000000",
              ... // very long response
            },
            "daSlotHash": "20f4eafc5d9657c2e22be2323a42f8f801d395fb8b4e5e1138953f0000000000",
            "sequencerCommitmentsRange": [
              0,
              0
            ],
            "sequencerPublicKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
            "sequencerDaPublicKey": "03015a7c4d2cc1c771198686e2ebef6fe7004f4136d61f6225b061d1bb9b821b9b",
            "preprovenCommitments": [],
            "lastL2Height": 0
          },
          "height": 50684
        }
    }
    ```

### Response Fields Explanation

- `proof`: Contains the zero-knowledge proof data.
  - `proof`: Raw proof data, represented as an array.
- `proofOutput`: Contains the output data of the proof verification process.
  - `initialStateRoot`: The state root before the state transition.
  - `finalStateRoot`: The state root after the state transition.
  - `prevSoftConfirmationHash`: The hash of the last soft confirmation before this one.
  - `finalSoftConfirmationHash`: The hash of the last soft confirmation in the state transition.
  - `stateDiff`: Differences in state resulting from the batch processing (collapsed for brevity).
  - `daSlotHash`: The Data Availability (DA) slot hash.
  - `sequencerCommitmentsRange`: The range of sequencer commitments in the DA slot.
  - `sequencerPublicKey`: The sequencer's public key.
  - `sequencerDaPublicKey`: The sequencer's Data Availability (DA) public key.
  - `preprovenCommitments`: List of pre-proven commitments (empty in this example).
  - `lastL2Height`: The L2 height of the last block included in the proof.
- `height`: The L1 height at which this proof was generated and verified.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getLastVerifiedBatchProof</code></summary>

This endpoint retrieves the last verified batch proof from the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getLastVerifiedBatchProof",
        "params": [],
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getLastVerifiedBatchProof","params":[], "id":31}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 31,
      "result": {
        "proof": {
          "proof": [
            2,
            0,
            1,
            215,
            ... // very long response
          ],
          "proofOutput": {
            "initialStateRoot": "3a0cb3797428e996e3dccf890e9a8fbb07c95745e4b72d83f8b7ac299804c43f",
            "finalStateRoot": "b8a4a101d199164db83fc6a3a49c6bf9face9539e485a24948dadd785fb5d1e4",
            "prevSoftConfirmationHash": "0000000000000000000000000000000000000000000000000000000000000000",
            "finalSoftConfirmationHash": "0000000000000000000000000000000000000000000000000000000000000000",
            "stateDiff": {
              "4163636f756e74732f6163636f756e74732f4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1": "eb9593515dd32b28a13c40fa96f6320fdd762aa6cfc2c6a8b6543b6a3ba40e346ab1050000000000","45766d2f612f1403c036ffb7dfb6b6d19f763948b8fb8faccaa529": "200000000000000000000000000000000000000000000000000011c37937e08000000000000000000000",
              ... // very long response
            },
            "daSlotHash": "20f4eafc5d9657c2e22be2323a42f8f801d395fb8b4e5e1138953f0000000000",
            "sequencerCommitmentsRange": [
              0,
              0
            ],
            "sequencerPublicKey": "4682a70af1d3fae53a5a26b682e2e75f7a1de21ad5fc8d61794ca889880d39d1",
            "sequencerDaPublicKey": "03015a7c4d2cc1c771198686e2ebef6fe7004f4136d61f6225b061d1bb9b821b9b",
            "preprovenCommitments": [],
            "lastL2Height": 0
          },
          "height": 50684
        }
    }
    ```

### Response Fields Explanation

- `proof`: Contains the zero-knowledge proof data.
  - `proof`: Raw proof data, represented as an array.
- `proofOutput`: Contains the output data of the proof verification process.
  - `initialStateRoot`: The state root before the state transition.
  - `finalStateRoot`: The state root after the state transition.
  - `prevSoftConfirmationHash`: The hash of the last soft confirmation before this one.
  - `finalSoftConfirmationHash`: The hash of the last soft confirmation in the state transition.
  - `stateDiff`: Differences in state resulting from the batch processing (collapsed for brevity).
  - `daSlotHash`: The Data Availability (DA) slot hash.
  - `sequencerCommitmentsRange`: The range of sequencer commitments in the DA slot.
  - `sequencerPublicKey`: The sequencer's public key.
  - `sequencerDaPublicKey`: The sequencer's Data Availability (DA) public key.
  - `preprovenCommitments`: List of pre-proven commitments (empty in this example).
  - `lastL2Height`: The L2 height of the last block included in the proof.
- `height`: The L1 height at which this proof was generated and verified.

</details>

<br>

<!-------------------------------------------------------------------->
<!-------------------------------------------------------------------->

<details>
<summary><code>ledger_getLastScannedL1Height</code></summary>

This endpoint retrieves the L1 height of the last scanned block by the ledger.

### Request

- **Method:** `POST`
- **Content-Type:** `application/json`
- **Endpoint URL:** `http://0.0.0.0:8080` (This is for docker-compose setup, replace with your rpc binding)
- **Request Body:**
    ```json
    {
        "jsonrpc": "2.0",
        "method": "ledger_getLastScannedL1Height",
        "params": [],
        "id": 31
    }
    ```
- **Example Request:** Here's an example curl you can use directly from your terminal
    ```sh
    curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"ledger_getLastScannedL1Height","params":[], "id":31}'  http://0.0.0.0:8080
    ```

### Response

- **Content-Type:** `application/json`
- **Response Body:**
    ```json
    {
      "jsonrpc": "2.0",
      "id": 31,
      "result": 66295
    }
    ```

### Response Fields Explanation

- The response is a number representing the L1 height of the last scanned block.

</details>