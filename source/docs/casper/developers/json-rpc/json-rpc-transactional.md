# Transactional JSON-RPC Methods

---

## account_put_deploy

:::caution

**DEPRECATED**: Use [account_put_transaction](#account-put-transaction) instead.

:::

This endpoint allows sending a deploy to be executed by the network.

|Parameter|Type|Description|
|---------|----|-----------|
|[deploy](./types_chain.md#deploy)|Object|A Deploy consists of an item containing a smart contract along with the requester's signature(s).|

<details>

<summary>Example account_put_deploy request</summary>

```json

{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "account_put_deploy",
  "params": [
    {
      "name": "deploy",
      "value": {
        "hash": "5c9b3b099c1378aa8e4a5f07f59ff1fcdc69a83179427c7e67ae0377d94d93fa",
        "header": {
          "account": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c",
          "timestamp": "2020-11-17T00:39:24.072Z",
          "ttl": "1h",
          "gas_price": 1,
          "body_hash": "d53cf72d17278fd47d399013ca389c50d589352f1a12593c0b8e01872a641b50",
          "dependencies": [
            "0101010101010101010101010101010101010101010101010101010101010101"
          ],
          "chain_name": "casper-example"
        },
        "payment": {
          "StoredContractByName": {
            "name": "casper-example",
            "entry_point": "example-entry-point",
            "args": [
              [
                "amount",
                {
                  "cl_type": "I32",
                  "bytes": "e8030000",
                  "parsed": 1000
                }
              ]
            ]
          }
        },
        "session": {
          "Transfer": {
            "args": [
              [
                "amount",
                {
                  "cl_type": "I32",
                  "bytes": "e8030000",
                  "parsed": 1000
                }
              ]
            ]
          }
        },
        "approvals": [
          {
            "signer": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c",
            "signature": "014c1a89f92e29dd74fc648f741137d9caf4edba97c5f9799ce0c9aa6b0c9b58db368c64098603dbecef645774c05dff057cb1f91f2cf390bbacce78aa6f084007"
          }
        ]
      }
    }
  ]
}

```

</details>

### `account_put_deploy_result`

The result contains the RPC API version and a [deploy_hash](./types_chain.md#deployhash), which is the primary identifier of a Deploy within a Casper network.

|Parameter|Type|Description|
|---------|----|-----------|
|api_version|String|The RPC API version.|
|[deploy_hash](./types_chain.md#deployhash)|String| A hex-encoded hash of the Deploy as sent.|

<details>

<summary>Example account_put_deploy result</summary>

```json

{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "name": "account_put_deploy_result",
    "value": {
      "api_version": "2.0.0",
      "deploy_hash": "5c9b3b099c1378aa8e4a5f07f59ff1fcdc69a83179427c7e67ae0377d94d93fa"
    }
  }
}

```

</details>

## account_put_transaction

This is the recommended means by which users can send their compiled Wasm (as part of a Transaction) to a node on a Casper network. The request takes in the [transaction](./types_chain.md#transaction) as a parameter, prior to sending it to a node on a network for execution.

|Parameter|Type|Description|
|---------|----|-----------|
|[transaction](./types_chain.md#transaction)|Object|A transaction consists of an item containing a Deploy and a [`Transaction`](./types_chain.md#transaction) along with the requester's signature(s).|

> **Note**: You can find a list of [trusted peers](../../operators/setup/joining.md/#known-addresses) in the network's configuration file, `config.toml`. Here is an [example config.toml](https://github.com/casper-network/casper-node/blob/dev/resources/production/config-example.toml#L131). You may send transactions to one of the trusted nodes or use them to query other online nodes.

<details>

<summary>Example account_put_transaction request</summary>

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "account_put_transaction",
  "params": [
    {
      "name": "transaction",
      "value": {
        "Version1": {
          "hash": "f5582cb81a5abda63ebaa4edb3b05210ecbd63ffb8dd17bfbeb3b867f4014468",
          "header": {
            "chain_name": "casper-example",
            "timestamp": "2020-11-17T00:39:24.072Z",
            "ttl": "1h",
            "body_hash": "aa24833ffbf31d62c8c8c4265349e7c09cd71952fcbce6f7b12daf5e340bf2cc",
            "pricing_mode": {
              "Fixed": {
                "gas_price_tolerance": 5
              }
            },
            "initiator_addr": {
              "PublicKey": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c"
            }
          },
          "body": {
            "args": [
              [
                "source",
                {
                  "cl_type": {
                    "Option": "URef"
                  },
                  "bytes": "010a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a07",
                  "parsed": "uref-0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a-007"
                }
              ],
              [
                "target",
                {
                  "cl_type": "URef",
                  "bytes": "1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b00",
                  "parsed": "uref-1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b-000"
                }
              ],
              [
                "amount",
                {
                  "cl_type": "U512",
                  "bytes": "0500ac23fc06",
                  "parsed": "30000000000"
                }
              ],
              [
                "id",
                {
                  "cl_type": {
                    "Option": "U64"
                  },
                  "bytes": "01e703000000000000",
                  "parsed": 999
                }
              ]
            ],
            "target": "Native",
            "entry_point": "Transfer",
            "transaction_category": 0,
            "scheduling": "Standard"
          },
          "approvals": [
            {
              "signer": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c",
              "signature": "0137d3f468d8f8a6e63f4110d79be29b8c8428e9cd858a92049660e7851ae16a299640d1fc1c930ab6cb424f1a6eec0b194df74bede14f4af1b5133106f1280d0b"
            }
          ]
        }
      }
    }
  ],
}

```

</details>

### `account_put_transaction_result`

The result contains the RPC API version and the [transaction_hash](./types_chain.md#transactionhash), which is the primary identifier of a transaction within a Casper network.

|Parameter|Type|Description|
|---------|----|-----------|
|api_version|String|The RPC API version.|
|[transaction_hash](./types_chain.md#transactionhash)|String| A hex-encoded hash of the transaction as sent.|

<details>

<summary>Example account_put_transaction result</summary>

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "name": "account_put_transaction_result",
    "value": {
      "api_version": "2.0.0",
      "transaction_hash": {
        "Version1": "f5582cb81a5abda63ebaa4edb3b05210ecbd63ffb8dd17bfbeb3b867f4014468"
      }
    }
  }
}

```

</details>

## speculative_exec

:::caution

**DEPRECATED**: Use [speculative_exec_txn](#speculative-exec-txn) instead.

:::

The `speculative_exec` endpoint allows executing a `Deploy` without committing its execution effects to global state. By default, `speculative_exec` is turned off on a node. Sending a request to a node with the endpoint unavailable will result in an error message. Find out if this endpoint is available and which port to access. The port is separate from the node's binary port and the default is 7778.

|Parameter|Type|Description|
|---------|----|-----------|
|[deploy](./types_chain.md#deploy)|Object|A Deploy consists of an item containing a smart contract along with the requester's signature(s).|

<details>

<summary>Example speculative_exec request</summary>

```json

{
  "jsonrpc": "2.0",
  "method": "speculative_exec",
  "params": {
    "deploy": {
      "hash": "b6aa46333fb858deee7f259a5bca581251c6200a5d902aeb1244c3a7169b5971",
      "header": {
        "account": "01a2905e4680aa49e0b44100d9dfc861b9605bb35f9956b1e99eb43863363d80aa",
        "timestamp": "2023-05-23T13:32:45.554Z",
        "ttl": "30m",
        "gas_price": 1,
        "body_hash": "74db109805bb20de43ef89a5b084544a858908b236601519d5827cd9b7fbb925",
        "dependencies": [],
        "chain_name": "integration-test"
      },
      "payment": {
        "ModuleBytes": {
          "module_bytes": "",
          "args": [
            [
              "amount",
              {
                "cl_type": "U512",
                "bytes": "0400e1f505",
                "parsed": "100000000"
              }
            ]
          ]
        }
      },
      "session": {
        "Transfer": {
          "args": [
            [
              "amount",
              {
                "cl_type": "U512",
                "bytes": "0400f90295",
                "parsed": "2500000000"
              }
            ],
            [
              "target",
              {
                "cl_type": "PublicKey",
                "bytes": "01265ea737411b349ad3d0fc724c2c588acd2765c057e5c690cd5e3dade401782b",
                "parsed": "01265ea737411b349ad3d0fc724c2c588acd2765c057e5c690cd5e3dade401782b"
              }
            ],
            [
              "id",
              {
                "cl_type": {
                  "Option": "U64"
                },
                "bytes": "010000000000000000",
                "parsed": 0
              }
            ]
          ]
        }
      },
      "approvals": [
        {
          "signer": "01a2905e4680aa49e0b44100d9dfc861b9605bb35f9956b1e99eb43863363d80aa",
          "signature": "01c94d517d5bbc8d5c74e0e68b8cb308561ff979a1c91907b56d427cc90156c437726c0b736d17f7303f2db66e405c7e5c8175b8b863703938eff1659766dff808"
        }
      ]
    }
  },
  "id": 6889533540839698701
}

```

</details>

### `speculative_exec_result`

The result contains the RPC API version and the speculative execution result.

|Parameter|Type|Description|
|---------|----|-----------|
|api_version|String|The RPC API version.|
|[execution_results](./types_chain.md#executionresult)|Object|The map of Block hash to execution result.|

<details>

<summary>Example speculative_exec result</summary>

<!--TODO the latest example from the Sidecar is empty. Test it or work with a dev to get the details. -->

```json

{
  "jsonrpc": "2.0",
  "id": -8801853076373554652,
  "result": {
    "name": "speculative_exec_result",
    "value": {
      "api_version": "2.0.0",
      "execution_result": {
        "block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
        "transfers": [],
        "limit": "0",
        "consumed": "0",
        "effects": [],
        "messages": [],
        "error": null
      }
    }
  }
}

```


</details>


## speculative_exec_txn

The `speculative_exec_txn` endpoint allows executing a `Transaction` without committing its execution effects to global state. By default, `speculative_exec_txn` is turned off on a node. Sending a request to a node with the endpoint unavailable will result in an error message. Find out if this endpoint is available and which port to access. The port is separate from the node's binary port and the default is 7778.

|Parameter|Type|Description|
|---------|----|-----------|
|[transaction](./types_chain.md#transaction)|Object|A Transaction is a versioned wrapper for a transaction or deploy.|

<details>

<summary>Example speculative_exec_txn request</summary>

```json

{
  "jsonrpc": "2.0",
  "method": "speculative_exec_txn",
  "params": {
    "transaction": {
      "Version1": {
        "hash": "f5582cb81a5abda63ebaa4edb3b05210ecbd63ffb8dd17bfbeb3b867f4014468",
        "header": {
          "chain_name": "casper-example",
          "timestamp": "2020-11-17T00:39:24.072Z",
          "ttl": "1h",
          "body_hash": "aa24833ffbf31d62c8c8c4265349e7c09cd71952fcbce6f7b12daf5e340bf2cc",
          "pricing_mode": {
            "Fixed": {
              "gas_price_tolerance": 5
            }
          },
          "initiator_addr": {
            "PublicKey": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c"
          }
        },
        "body": {
          "args": [
            [
              "source",
              {
                "cl_type": {
                  "Option": "URef"
                },
                "bytes": "010a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a07",
                "parsed": "uref-0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a-007"
              }
            ],
            [
              "target",
              {
                "cl_type": "URef",
                "bytes": "1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b00",
                "parsed": "uref-1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b-000"
              }
            ],
            [
              "amount",
              {
                "cl_type": "U512",
                "bytes": "0500ac23fc06",
                "parsed": "30000000000"
              }
            ],
            [
              "id",
              {
                "cl_type": {
                  "Option": "U64"
                },
                "bytes": "01e703000000000000",
                "parsed": 999
              }
            ]
          ],
          "target": "Native",
          "entry_point": "Transfer",
          "transaction_category": 0,
          "scheduling": "Standard"
        },
        "approvals": [
          {
            "signer": "01d9bf2148748a85c89da5aad8ee0b0fc2d105fd39d41a4c796536354f0ae2900c",
            "signature": "0137d3f468d8f8a6e63f4110d79be29b8c8428e9cd858a92049660e7851ae16a299640d1fc1c930ab6cb424f1a6eec0b194df74bede14f4af1b5133106f1280d0b"
          }
        ]
      }
    }
  },
  "id": 6889533540839698701
}

```

</details>

### `speculative_exec_txn_result`

The result contains the RPC API version and the speculative execution result.

|Parameter|Type|Description|
|---------|----|-----------|
|api_version|String|The RPC API version.|
|[execution_result](./types_chain.md#executionresult)|Object|Result of the speculative execution.|

<details>

<!--TODO the latest example from the Sidecar is empty. Test it or work with a dev to get the details. -->

<summary>Example speculative_exec_txn result</summary>

```json

{
  "jsonrpc": "2.0",
  "id": -8801853076373554652,
  "result": {
    "name": "speculative_exec_txn_result",
    "value": {
      "api_version": "2.0.0",
      "execution_result": {
        "block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
        "transfers": [],
        "limit": "0",
        "consumed": "0",
        "effects": [],
        "messages": [],
        "error": null
      }
    }
  }
}

```


</details>