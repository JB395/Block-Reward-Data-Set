# Block-Reward-Data-Set
A data set of Qtum block reward winners with wallet balances, November 1 - 11, 2017
11 days of data starting from November 1st, with data from block 37,021 to block 43,593, or 6,572 blocks total.

Files saved as .csv with the address and total balance listed:

QPoVZqSCxtWkHgNt8jkipv6KNJUMmE7gRT,690104.5302,,,
QX9gho1UcSbPMADUBSa2mJKQCQDbE2QrCR,720118.4107,,,
QTAnGnNKj3jdTUbjJmL3g8nFkxkZtzxUxY,800979.2776,,,
QRFhh8Wuwtgk2pA33xhpLrQmw73ex4Vtvp,892849.9786,,,
QcTBUCAbHPqVL7M72QqDQV6Vb6pe64S9cx,1026587.006,,,
QQujfYYrEd3G3bwjZWiBRzyxYwmvK3fyRy,1913101.808,,,



# Commands 1-5-2019
The Qtum Core wallet has a rich set of commands which give **comprehensive control of the wallet and the blockchain**. This manual gives every command and every response; if you want to do anything from basic to complex on the blockchain, these commands will do the job.

There are two sets of commands that may be used with Qtum Core wallets:

* Console commands are given to a wallet that is already running.
* Startup commands are used when starting up a wallet.

This manual focuses on the console commands which are given to a wallet that is running and can be sent using RPC (Remote Procedure Calls) to the qtumd server wallet or given to the qtum-qt desktop GUI (Graphical User Interface) wallet using the Debug window Console command line:
 
For the server wallet qtumd, console commands are given using the Command Line Interface application qtum-cli on the system command line prompt [comment about path]:

(photo)

Commands in Linux, Mac OS, Windows 10
You can always get a list of the current console commands using the help command.

(photo)

## Console Commands :hammer_and_wrench: :hammer_and_wrench: :hammer_and_wrench: :hammer_and_wrench: :hammer_and_wrench:

Console commands are given to a running Qtum Core wallet and provide additional information and control. Console commands and are required to operate the qtumd server wallet, which is a “headless” wallet with no graphical user interface.

There are 125 console commands with some good references for the 112 inherited from bitcoin. There are 13 “hidden” commands that are used by developers and won’t show up in the “help” list.

These commands can have required or optional parameters and more numerous parameters are entered in **JSON** (JavaScript Object Notation) format with escaped double quotes ( \” ) as shown below.

Common parameters for these commands are Qtum addresses, block hashes, contract addresses, etc. Some of the commands will have an optional parameter “minconf” (minimun number of confirmations) which allows you to get a response only for a transaction or block that has had that number of confirmations.

A quick comment on “accounts”. Accounts was an ill-fated way from bitcoin to track balances for what are really UTXO transaction-based values, and “accounts” are deprecated and will be phased out by version 0.17.

The chain query bitcoin API reference http://chainquery.com/bitcoin-api explains the parameters and gives examples with responses. The bitcoin chain query API reverence gives 67 commands, of which 2 are not in Qtum (estimatepriority, getgenerate) and two (gettransaction, walletpassphrase) have an additional parameter.
(https://bitcoin.org/en/developer-reference#remote-procedure-calls-rpcs 

Advanced interfaces to the Qtum Core wallet (full node) can use these “console commands” as RPCs (Remote Procedure Calls) over a dedicated port connection to the node. [are RPC commands the same?] If you are building an exchange hot wallet or server node for a mobile DAPP (Distributed Application) you can use RPCs, which follow these same console commands. (The client offers an JSON-RPC interface over HTTP over sockets to perform various operational functions and to manage the local wallet.)

Here are some command groupings that are useful for various tasks.

* Peer connections: getconnectioncount, getpeerinfo, addnodes, getnetworkinfo
* Staking: getstakinginfo, getwalletinfo, getnetworkinfo
* Sending: listaddressgroupings, sendtoaddress, sendmany, sendmanywithdupes
* Raw transactions: crearterawtransaction, signrawtransaction, combinerawtransactoins, sendrawtransaction.
* Smart contract transactions: createcontract, callcontract, sendtocontract, getaccountinfo, getstorage, searchlogs, waitforlogs

 
## Startup Commands

Startup commands give additional control and recovery options when launching the wallet. For example, you can use startup commands with the qtum-qt desktop GUI wallet for various kinds of blockchain recovery techniques, add additional debug logging or additional controls. The same startup commands work with the qtumd server wallet. If you are going to use these startup commands, make sure you have a good backup of the wallet.dat file.

You can see a complete set of startup commands in the qtum-qt wallet using Help – Command-line options. [reference X]
 
## Console Commands A - Z

For these console commands, responses are given for default parameters (Qtum version 0.16 – Winter 2018). Commands marked DEPRECATED should not be used because they will be removed and replaced in future versions of the Core wallet, for example, commands using “account” will be removed in version 0.18.

Using the command `help <command name>` will give complete information about the command and relevant parameters. The format below shows the command with parameters and the response, in some cases truncated with the term `<snip>`.

### abandontransaction "txid"

Works on transactions not in the blockchain or mempool. Tried Testnet with no network connections

```
abandontransaction "0cc99a30bc2064041ea4263835b4ed594ff500c56d6b14e4970aeee548e71389"

Transaction not eligible for abandonment (code -5)
```


### abortrescan

Stops a wallet rescan triggered by a command such as importprivkey. This command can be issued by a 2nd command line window, in which case the command will stop the scan and return “true”.

```
qtum-cli abortrescan

true
```


**addmultisigaddress nrequired ["key",...] ( "account" "address_type" )**

Add a multisignature address to the wallet so you can receive and send from that address. Run the command on each machine that will be signing and backup the wallet.dat file. The address can be a Qtum address or hex-encoded public key. Use importaddress to add the multisig address on each signing wallet.

This functionality is only intended for use with non-watchonly addresses. See `importaddress` for watchonly p2sh address support.

Use of account is DEPRECATED).

See also validateaddress.
XXXXX how to send?

```
addmultisigaddress 2 "[\"QgdaD9b3ppKowoC45EZMtepjjBfnvEe6m\",\"QFmr8vY29reHj73XSHfdWvkV3mD57Kqd8\"]"

{
  "address": "mHB9w64hHbm2YtCxyqS8kG3g77b2gbSvK",
  "redeemScript": "5321538ef45ab52bd53508adfda3cfe82ebcaf0495963729e5ff2a8e5aeecdd4cdd23daea03cf4394ad4c578caff2d297ce937c3afba5bc56f31c786b2addf56c72ab"
}
```


**addnode "node" "add|remove|onetry"**

Adds a node with a known IP address.

```
addnode 35.200.159.68:3888 add
```

(qtum-qt will return “null”)


**addwitnessaddress "address" ( p2sh )**

Hidden command. DEPRECATED. This command was a way to generate a SegWit address from an existing legacy address, usually a P2SH-P2WPKH addresses - Pay-to-Witness-Public-Key-Hash (P2WPKH) script embedded in a Pay-to-Script-Hash (P2SH) address. This command is mostly disabled in version 0.16 and will be removed in version 0.17. Instead, use the getnewaddress command with address type p2sh-segwit or bech32.

Launch v 0.16 qtumd with -deprecatedrpc=addwitnessaddress to run the command:

```
addwitnessaddress QkSc3wcAJ59Nk4X9mvd258ycVxJ7XWhp9

MHa57hGwD48SZQjh23MZbq24tFwTu7Q3c
```


backupwallet "destination"

The destination can be a filename or path with a filename. (on Windows). The wallet must be fully decrypted (not just for staking only) for this command to work.
backupwallet "C:\Users\<username>\Desktop\Backups\backup2018-10-21.dat"
null

(qtum-qt will returns “null”, qtumd says nothing)

bumpfee "txid" ( options )

Bumps the fee of a transaction, replacing it with a new transaction by adjusting the change. The new fee can be calculated automatically or by using various options (see “help bumpfee”).

bumpfee "cae25062777fca1bce7f860dd238af2be4495f4aef1b5a15bbee260b4c3cde2"

{
  "txid": "ecbc798c140b5104ae3d5828493e8e5ef04131dd0e44eb7fa7e1abba24901a5",
  "origfee": 0.00090400,
  "fee": 0.00093061,
  "errors": [
  ]
}

callcontract "address" "data" ( address )

Argument:
1. "address"          (string, required) The account address
2. "data"             (string, required) The data hex string
3. address              (string, optional) The sender address hex string
4. gasLimit             (string, optional) The gas limit for executing the contract

clearbanned

Clear all banned nodes IPs. Qtum-qt returns “null”, qtumd has no response.

clearbanned

null


combinerawtransaction ["hexstring",...]

Combine multiple partially signed raw transactions into one transaction. The combined transaction may be another partially signed transaction or a fully signed transaction.

combinerawtransaction "[\"0200000001b<snip>000000\", \"0200000001c<snip>000000\"]"

0200000001b<snip>000000


createcontract "bytecode" (gaslimit gasprice "senderaddress" broadcast)

Publish a smart contract for the given bytecode, using default gas price of 0.00000040 and gas amount of 2,500,000. Returns the transaction ID and the contract address hash for the contract.

createcontract 60606040<snip>41701d0029

{
  "txid": "a1d823cb5ce776200dcbe729d901d4e1d31ad5bb8d835db0f704g73ac8e74d3",
  "sender": "QfdR8vwd69oMi2xeeSe8XBgv3mku27ukh",
  "hash160": "efe574bu5ce54a7eb33f96e4336accbed826ffe",
  "address": "4853cd349027fe239fed85837456b3c48aae42a"
}

createmultisig nrequired ["key",...]

DEPRECATED. Use addmultisigaddress.

createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,"data":"hex",...} ( locktime ) ( replaceable )

Create a hex-encoded raw transaction sending some inputs to outputs. The transaction must then be signed and sent to the network, see signrawtransaction and sendrawtransaction. Returns a hex-encoded raw transaction. If you are sending coins be sure to create a change address so the change can be returned; any difference between the input values and output values will be taken as the transaction fee. If you don’t work out the math you could pay a big transaction fee.

createrawtransaction "[{\"txid\":\"18a8b7ae5303948cb2f06ff335d7b63ab17412bdade2897fc19742d2ccfe68b\",\"vout\":0}]" "{\"Qd5eHho389mJMCxSzAbe31w2vntTc7192\":1.0, \"QdXtq55Bf443Lkme2hdj4mD8KKepk32f6\":0.99}"

020000001d637cf207c6418fb6220cfcb2b141ba5d230f0ba0a43c68b358aff9a8a6000000000fffffff0509e5f50480000001983a952d5fb15a70832ffdc3cadd3dc7749a49bf053ed6defacd09ef6060000000187aa814bd4ae4167bebbf56bb7c51c5eabf35d50a5c6e7f33ad0000000


decoderawtransaction "hexstring" ( iswitness )

Gives the decoded data from a raw hex-encoded transaction. Here we decode the results from creatrawtransaction above. XXXXX not obfuscated

decoderawtransaction 0200000<snip>88ac00000000

{
  "txid": "c95adce3c9fc5ff37e4c79aad334f871340b57e4a36a557fe3e6b437279bc151",
  "hash": "c95adce3c9fc5ff37e4c79aad334f871340b57e4a36a557fe3e6b437279bc151",
  "version": 2,
  "size": 119,
  "vsize": 119,
  "locktime": 0,
  "vin": [
    {
      "txid": "19a8e9ae5203658ca2f0060f225d7a63af13712bcadf2027fb19847d20cfb6d6",
      "vout": 0,
      "scriptSig": {
        "asm": "",
        "hex": ""
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 1.00000000,
<snip>
    }
  ]
}

decodescript "hexstring"

Decode a hex-encoded script, for example, decode the script for a mulitsig address: XXXXX not obfuscated

decodescript 522103861b490b2b5250865fda0bfe43e45af0285965229e3df2a7f5affcdd3bdd1fda2103ba4294fc4c516ca352dfb7cb938c2af1a5bc65f31c876b9accf58c69aa2045aa52ae

{
  "asm": "2 03861b490b2b5250865fda0bfe43e45af0285965229e3df2a7f5affcdd3bdd1fda 03ba4294fc4c516ca352dfb7cb938c2af1a5bc65f31c876b9accf58c69aa2045aa 2 OP_CHECKMULTISIG",
  "reqSigs": 2,
  "type": "multisig",
  "addresses": [
    "qghwDvb1pyJoqoAD2ESMTevvvjUfNvReDn",
    "qfKr7vXdmroHi61XUUHedXvgV2mDx9Kudb"
  ],
  "p2sh": "mHA8y94hCBMnYStBxrqTgUGHgccb3gaSVj"
}

disconnectnode "[address]" [nodeid]

Disconnects a peer (node) using either the IP address or node number.

disconnectnode "35.198.0.76:3888"

null

(qtum-qt will return “null”)

dumpprivkey "address"

Displays the private key for a given address using WIF (Wallet Import Format). The wallet must be unlocked (and not for “staking only”) for this command to work.

dumpprivkey "QXfSbN8DaT21Z6L3Pw52UexuP8zW4mY6h"
L3vaEHms4VpciP85UsufQf3Gfa9p5gatBGm6Z4rD8FxzFLdXE5V

dumpwallet "filename"

Writes all wallet the private keys in file in a human-readable format. The wallet must be fully decrypted (not just for staking only) for this command to work. Returns the filename.

On PC:

dumpwallet "C:\Users\<username>\Desktop\Backups\dump 2018-10-30.txt"

{
  "filename": "C:\\Users\\<username>\\Desktop\\Backups\\dump 2018-10-30.txt"
}

File:
```
# Wallet dump created by Qtum v0.16.1.0-0806c12c4-dirty
# * Created on 2018-10-31T01:56:32Z
# * Best block at time of backup was 241161 (b11d5169d66d39cbcd848e2ea3f9adfcee1c22af945f100aec9e762d88609e2e),
#   mined on 2018-10-31T01:55:28Z

# extended private masterkey: <snip>
```

On Mac:

dumpwallet /Users/<USERNAME>/Desktop/dump_2018-12-15.txt

{

  "filename": "/Users/<USERNAME>/Desktop/dump_2018-12-15.txt"

}

File:
```
# Wallet dump created by Qtum v0.16.2.0-47a30461d-dirty
# * Created on 2018-12-16T02:22:59Z
# * Best block at time of backup was 148510 
(ca8a9457a69882b395bf56671e3661e148d3aca4a7b6c77398804229e7fb58f4),
#   mined on 2018-05-06T02:45:52Z
# extended private masterkey: 
xprv9s21ZrQH143K2qbpjzTMVnP7Zzr5BrUwXXPN2pmQQZ2evwf6MJ6KKvEcLhicp3s1v1YX1R7LWqAn
KHbx9Qtx58fc
# Wallet dump created by Qtum v0.16.2.0-47a30461d-dirty
# * Created on 2018-12-16T02:22:59Z
# * Best block at time of backup was 148510 
(ca8a9457a69882b395bf56671e3661e148d3aca4a7b6c77398804229e7fb58f4),
#   mined on 2018-05-06T02:45:52Z
# extended private masterkey: 
xprv9s21ZrQH143K2qbpjzTMVnP7Zzr5BrUwXXPN2pmQQZ2evwf6MJ6KKvEcLhicp3s1v1YX1R7LWqAn
KHbx9Qtx58fc
# Wallet dump created by Qtum v0.16.2.0-47a30461d-dirty
# * Created on 2018-12-16T02:22:59Z
# * Best block at time of backup was 148510 (ca8a9457a69882b395bf56671e3661e148d3aca4a7b6c77398804229e7fb58f4),
#   mined on 2018-05-06T02:45:52Z

# extended private masterkey: <snip>
```

encryptwallet "passphrase"

Encrypts the wallet with “passphrase” for first-time encryption. After encryption, any calls that interact with private keys such as sending or signing will require passphrase entry to do these functions. See also walletpassphrase, walletlock and walletpassphrasechange. After this command runs, the wallet will shut down.

encryptwallet "you should always use a long and strong passphrase"


echo “message”

Hidden command. Simply echo back the input arguments. This command is for testing.

The difference between echo and echojson is that echojson has argument conversion enabled in the client-side table in qtum-cli and the GUI. There is no server-side difference. Here we echo the parameters to setup a multisig address.


echo "[\"qghwDvb1pyJoqoAD2ESMTevvvjUfNvReDn\",\"qfKr7vXdmroHi61XUUHedXvgV2mDx9Kudb\"]"

[
  "[\"qghwDvb1pyJoqoAD2ESMTevvvjUfNvReDn\",\"qfKr7vXdmroHi61XUUHedXvgV2mDx9Kudb\"]"
]

echojson

echojson "message" ...

Hidden command. Simply echo back the input arguments. This command is for testing.

The difference between echo and echojson is that echojson has argument conversion enabled in the client-side table in qtum-cli and the GUI. There is no server-side difference. Here we echo the parameters to setup a multisig address.

echojson
"[\"qghwDvb1pyJoqoAD2ESMTevvvjUfNvReDn\",\"qfKr7vXdmroHi61XUUHedXvgV2mDx9Kudb\"]"

[
  [
    "qghwDvb1pyJoqoAD2ESMTevvvjUfNvReDn",
    "qfKr7vXdmroHi61XUUHedXvgV2mDx9Kudb"
  ]
]


estimatefee nblocks

DEPRECATED. Please use estimatesmartfee for more intelligent estimates. Estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within nblocks blocks.

estimatefee 10

estimatefee is deprecated and will be fully removed in v0.17.


estimaterawfee conf_target (threshold)

Hidden command. WARNING: This interface is unstable and may disappear or change, and the results are tightly coupled to the calling parameters.

Gives fee estimates for short, medium, and long term confirmations, and gives mempool statistics for transactions with those fees. Estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within conf_target blocks.

estimaterawfee 6

{
  "short": {
    "feerate": 0.01000002,
    "decay": 0.962,
    "scale": 1,
    "pass": {
      "startrange": 490954,
      "endrange": 1e+099,
      "withintarget": 14.31,
      "totalconfirmed": 14.31,
      "inmempool": 0,
      "leftmempool": 0
    },
    "fail": {
      "startrange": 0,
      "endrange": 490954,
      "withintarget": 3.57,
      "totalconfirmed": 3.57,
      "inmempool": 0,
      "leftmempool": 0
    }
  },
  "medium": {
    "feerate": 0.00419464,
    "decay": 0.9952,
    "scale": 2,
    "pass": {
      "startrange": 403909,
      "endrange": 490954,
      "withintarget": 28.94,
      "totalconfirmed": 28.94,
      "inmempool": 0,
      "leftmempool": 0
    },
    "fail": {
      "startrange": 0,
      "endrange": 403909,
      "withintarget": 2.45,
      "totalconfirmed": 2.45,
      "inmempool": 0,
      "leftmempool": 0
    }
  },
  "long": {
    "feerate": 0.00420764,
    "decay": 0.99931,
    "scale": 24,
    "pass": {
      "startrange": 403909,
      "endrange": 626596,
      "withintarget": 190,
      "totalconfirmed": 190,
      "inmempool": 0,
      "leftmempool": 0
    },
    "fail": {
      "startrange": 0,
      "endrange": 403909,
      "withintarget": 12.93,
      "totalconfirmed": 12.93,
      "inmempool": 0,
      "leftmempool": 0
    }
  }
}


estimatesmartfee conf_target ("estimate_mode")

Estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within conf_target blocks.

estimatesmartfee 10
{
  "feerate": 0.00420597,
  "blocks": 10
}

fromhexaddress "hexaddress"

Converts a raw hex address to a base 58 pubkeyhash address. Returns the base58 pubkeyhash address.

fromhexaddress 9467c4cbd6c4bb736cf4724de25a298bc529bfa3

QXvN7L3D2NkdPgt20rYrGkSPCggwkKbqa

See gethexaddress to convert a base58 pubkeyhash to a hex address.

fundrawtransaction "hexstring" ( options iswitness )

Add inputs to a transaction until it has enough in value to meet its out value.
This will not modify existing inputs, and will add at most one change output to the outputs.
No existing outputs will be modified unless "subtractFeeFromOutputs" is specified.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.
The inputs added will not be signed, use signrawtransaction for that.
Note that all existing inputs must have their previous output transaction be in the wallet.
Note that all inputs selected must be of standard form and P2SH scripts must be
in the wallet using importaddress or addmultisigaddress (to calculate fees).
You can see whether this is the case by checking the "solvable" field in the listunspent output.
Only pay-to-pubkey, multisig, and P2SH versions thereof are currently supported for watch-only

Arguments:
1. "hexstring"           (string, required) The hex string of the raw transaction
2. options                 (object, optional)
   {
     "changeAddress"          (string, optional, default pool address) The qtum address to receive the change
     "changePosition"         (numeric, optional, default random) The index of the change output
     "change_type"            (string, optional) The output type to use. Only valid if changeAddress is not specified. Options are "legacy", "p2sh-segwit", and "bech32". Default is set by -changetype.
     "includeWatching"        (boolean, optional, default false) Also select inputs which are watch only
     "lockUnspents"           (boolean, optional, default false) Lock selected unspent outputs
     "feeRate"                (numeric, optional, default not set: makes wallet determine the fee) Set a specific fee rate in QTUM/kB
     "subtractFeeFromOutputs" (array, optional) A json array of integers.
                              The fee will be equally deducted from the amount of each specified output.
                              The outputs are specified by their zero-based index, before any change output is added.
                              Those recipients will receive less qtums than you enter in their corresponding amount field.
                              If no outputs are specified here, the sender pays the fee.
                                  [vout_index,...]
     "replaceable"            (boolean, optional) Marks this transaction as BIP125 replaceable.
                              Allows this transaction to be replaced by a transaction with higher fees
     "conf_target"            (numeric, optional) Confirmation target (in blocks)
     "estimate_mode"          (string, optional, default=UNSET) The fee estimate mode, must be one of:
         "UNSET"
         "ECONOMICAL"
         "CONSERVATIVE"
   }
                         for backward compatibility: passing in a true instead of an object will result in {"includeWatching":true}
3. iswitness               (boolean, optional) Whether the transaction hex is a serialized witness transaction 
                              If iswitness is not present, heuristic tests will be used in decoding

Result:
{
  "hex":       "value", (string)  The resulting raw transaction (hex-encoded string)
  "fee":       n,         (numeric) Fee in QTUM the resulting transaction pays
  "changepos": n          (numeric) The position of the added change output, or -1
}

Examples:

Create a transaction with no inputs
> qtum-cli createrawtransaction "[]" "{\"myaddress\":0.01}"

Add sufficient unsigned inputs to meet the output value
> qtum-cli fundrawtransaction "rawtransactionhex"

Sign the transaction
> qtum-cli signrawtransaction "fundedtransactionhex"

Send the transaction
> qtum-cli sendrawtransaction "signedtransactionhex"

generate nblocks ( maxtries )

Mine up to nblocks blocks immediately to an address in the wallet. Used with the regression test "regtest" private test blockchain. Can use “generate 1” to publish waiting transactions in the next block.

docker exec myapp qcli generate 600

[
  "5476f8f1c5feb8dd0eaecb113715337a418ae2538f478bb2e5454a801e2aec15",
  "465b21528a217250ed300d0636e7838f8eb2539afec2a87e7052e2f901e31c03",
  <snip 596 block hashes>
  "5716637b80c70d3b0f6cb338fc772ac952c535fa19be086ee2e9c2e97d5ef396",
  "59cde5be7929118bfc3239a93226eb35b7e0093fe9b6b7007b126365ab04901a"
]


generatetoaddress nblocks address (maxtries)

Mine up to nblocks immediately to a specified address. Used with the regression test "regtest" private test blockchain.
getaccount "address"
DEPRECATED. Returns the account name for a given address.
getaccount "Q5ytvGW47jcS38NGz9cV6WtK2d3JqhQv5"
My trading Acct 101

getaccountaddress "account"

DEPRECATED. Returns the current Qtum address for an account. Use the null account "" for the default address.

getaccountaddress "My trading Acct 101"

Q5ytvGW47jcS38NGz9cV6WtK2d3JqhQv5

getaccountinfo "contract address hash"

Returns information about a contract including storage (the QRC20 balance for every address) and the code. The command may take several minutes to return, depending on the size of the storage (number of token holders). Here is the INK contract.

getaccountinfo fe59cbc1704e89a698571413a81f0de9d8f00c69

{
  "address": "fe59cbc1704e89a698571413a81f0de9d8f00c69",
  "balance": 0,
  "storage": {
    "00009fe46099bb10054d8b89985c4729a9e8e06539e462e9d250f6afc80ac616": {
      "50e4c63c65830a6709ce0dbc387bbdcef75805769b71841b8d401049e77a64a2":
      "0000000000000000000000000000000000000000000000000000000000000064"
    },
    "0000dd359df16177c5a00a2155979a1ca105df754abbe99bb09af5abc84d4aa4": {
      "896176e7c566fd067c483861fcad197b31e0f9206ee19b71aa704d8978eb2bdc":
      "0000000000000000000000000000000000000000000000000000000000000001"
    },
    <snip>
    "fffd29bb4ec023344772b5c3cda826cba78eed7cc88791357ee4ae9fdee2f0aa": {
      "3f2cc887dcd9aaf8b642c415b8201a833f08695db6e6dbc3f41b6376a8467452": 
      "000000000000000000000000000000000000000000000000000000006e44c280"
    }
  },
  "code": "606060405236 <snip> c62c0029"
}

getaddednodeinfo ( "node" )

getaddednodeinfo
[
  {
    "addednode": "35.200.159.68:3888",
    "connected": true,
    "addresses": [
      {
        "address": "35.200.159.68:3888",
        "connected": "outbound"
      }
    ]
  },
  {
    "addednode": "35.231.140.221:3888",
    "connected": true,
    "addresses": [
      {
        "address": "35.231.140.221:3888",
        "connected": "outbound"
      }
    ]
  }
]

getaddressesbyaccount "account"
DEPRECATED. Returns the list of addresses for the given account.

getaddressesbyaccount "MyAccount1"

[
  "QDmZ3dx13suFL53hjRyLSmfpYc87CQ6m",
  "Q4FtdG46MkSW7N6gzJcv3Wt8Sd32qaPce"
]

getbalance ( "account" minconf include_watchonly )

getbalance

1.53160855

getbestblockhash

Returns the block hash of the most recent block.

getbestblockhash

a18adc4fb204fe1818c30e3003fa8c8bf1833692d6df7dbb66d1569d27b18f8e

getblock "blockhash" ( verbosity )

Returns information about a block.

getblock a18adc4fb204fe1818c30e3003fa8c8bf1833692d6df7dbb66d1569d27b18f8e

{
  "hash": "a18adc4fb204fe1818c30e3003fa8c8bf1833692d6df7dbb66d1569d27b18f8e",
  "confirmations": 1,
  "strippedsize": 1642,
  "size": 1678,
  "weight": 6604,
  "height": 244847,
  "version": 536870912,
  "versionHex": "20000000",
  "merkleroot": "8b5bc1b8201c483b3d2a8ea528429c73755cd4d026f4ea9ec000a8b9ff4d6913",
  "hashStateRoot": "8c72ed6a22f205a31965acc8deb4e5a83aa1b3e9c2eb72b41fb966c244bbf7ae",
  "hashUTXORoot": "a4a9138f1c02a7a8f2948cf255be5c85a774f0c58e55e754eeed83e41e44b0e3",
  "tx": [
    "be6f9a76ea5e495102504bcd4e0975006f9287421535eb830812bf225fd85a81",
    "ef1153d23cbf664ddf37e93ad3995c77faba5f9425c9a8edf07173d10d68cc7d",
    "28368d7c9afe354ee47322a21943191f41adef227b4cd28f808672a5fd6502ca",
    "287ba39ef4b2f08dcf94bca8390749440fa3ef955c7270dc13ceff63d5a547a0",
    "d9454d185072e44b7eedfd7d99d547af715e7966b774cc58c4378cd21404b6c1"
  ],
  "time": 1539478304,
  "mediantime": 1539478016,
  "nonce": 0,
  "bits": "1a06ff51",
  "difficulty": 2397623.192092059,
  "chainwork": "0000000000000000000000000000000000000000000000ae38c2396a362a5cad",
  "previousblockhash": "4844ea8b549bda9a1d95050d68846f5943f07fbb39709cdd0c386efea193582d",
  "flags": "proof-of-stake",
  "proofhash": "00018df67d4d91255e3ccc6256ec894267aedddb0b17addc27984d33bbda0163",
  "modifier": "a7d1d4fd5670a282ae5c8f231d3473eaa21ae89fe344f8101847fc11da8e6262",
  "signature": "3044022019ef654d26e49f7bfa8cd2793fb4db7ab9017f82ec480f723230633a68757fb0022006d9137a72b61faf249587dcc323c1b097bfd1c923fba6be543d4d5227d4fc15"
}

getblockchaininfo
Returns information about the blockchain. Here “blocks” equals “headers”, so this wallet is synced to the latest blocks. “moneysupply” gives the total QTUM created (genesis blocks + all block rewards). “size_on_disk” gives the local storage size for blocks.

getblockchaininfo

{
  "chain": "main",
  "blocks": 244847,
  "headers": 244847,
  "bestblockhash": "a18adc4fb204fe1818c30e3003fa8c8bf1833692d6df7dbb66d1569d27b18f8e",
  "difficulty": 2397623.192092059,
  "moneysupply": 100959388,
  "mediantime": 1539478016,
  "verificationprogress": 0.9999876869542087,
  "initialblockdownload": false,
  "chainwork": "0000000000000000000000000000000000000000000000ae38c2396a362a5cad",
  "size_on_disk": 1343259702,
  "pruned": false,
  "softforks": [
    {
      "id": "bip34",
      "version": 2,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip66",
      "version": 3,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip65",
      "version": 4,
      "reject": {
        "status": true
      }
    }
  ],
  "bip9_softforks": {
    "csv": {
      "status": "active",
      "startTime": 0,
      "timeout": 999999999999,
      "since": 6048
    },
    "segwit": {
      "status": "active",
      "startTime": 0,
      "timeout": 999999999999,
      "since": 6048
    }
  },
  "warnings": ""
}

getblockcount

Returns the number of blocks in the longest blockchain.

getblockcount

244848

getblockhash height

Returns the hash of the main blockchain for the given block height (not an orphan block).

getblockhash 244848

8c0a43d58e96bd081209243eb9406c98ab3771c900cc05d793a62c48f3b1c03f

getblockheader "hash" ( verbose )

Returns hex or decoded data for the header of the given block hash.
getblockheader 8c0a43d58e96bd081209243eb9406c98ab3771c900cc05d793a62c48f3b1c03f

{
  "hash": "8c0a43d58e96bd081209243eb9406c98ab3771c900cc05d793a62c48f3b1c03f",
  "confirmations": 1,
  "height": 244848,
  "version": 536870912,
  "versionHex": "20000000",
  "merkleroot": "55e9df47918882e9da67fca364102785f95f630277251e0f02c5a999a5ad7222",
  "time": 1539478576,
  "mediantime": 1539478048,
  "nonce": 0,
  "bits": "1a057777",
  "difficulty": 3068960.095125648,
  "chainwork": "0000000000000000000000000000000000000000000000ae38f10db922d370e0",
  "hashStateRoot": "8c72ed6a22f205a31965acc8deb4e5a83aa1b3e9c2eb72b41fb966c244bbf7ae",
  "hashUTXORoot": "a4a9138f1c02a7a8f2948cf255be5c85a774f0c58e55e754eeed83e41e44b0e3",
  "previousblockhash": "a18adc4fb204fe1818c30e3003fa8c8bf1833692d6df7dbb66d1569d27b18f8e",
  "flags": "proof-of-stake",
  "proofhash": "00001121392ae309cf450c96f461cc4dcbaab48a07da28b3c391774a5051ea13",
  "modifier": "d48db9884b7fd19852db420ad8faee6a51ca122574c041155965ac7de4cfe10c"
}

getblocktemplate ( TemplateRequest )

Returns data needed to construct a block, and has a number of TemplateRequst parameters. The default (no TemplateReqest) gives:
getblocktemplate 

{
  "capabilities": [
    "proposal"
  ],
  "version": 536870912,
  "rules": [
    "csv",
    "segwit"
  ],
  "vbavailable": {
  },
  "vbrequired": 0,
  "previousblockhash": "584350297b043c72e04afc9a5b5e61d5067b1c478554927dffb3101ccf681925",
  "transactions": [
    {
      "data": "0200000000020000000000000000000084d71700000000015100000000",
      "txid": "65b1a6eb24a3e0833a986eb88f969784811e2143ffce51d630873c1a49a2b596",
      "hash": "65b1a6eb24a3e0833a986eb88f969784811e2143ffce51d630873c1a49a2b596",
      "depends": [
      ],
      "fee": -9223335579943919278,
      "sigops": -8646875390281014959,
      "weight": 116
    }
  ],
  "coinbaseaux": {
    "flags": ""
  },
  "coinbasevalue": 0,
  "longpollid": "584350297b043c72e04afc9a5b5e61d5067b1c478554927dffb3101ccf681925654",
  "target": "000000000000052f650000000000000000000000000000000000000000000000",
  "mintime": 1542420161,
  "mutable": [
    "time",
    "transactions",
    "prevblock"
  ],
  "noncerange": "00000000ffffffff",
  "sigoplimit": 80000,
  "sizelimit": 8000000,
  "weightlimit": 8000000,
  "curtime": 1542420909,
  "bits": "1a052f65",
  "height": 265308
}

getchaintips

Return information about all known tips in the block tree, including the main chain as well as orphaned branches. Will display chain tips since the wallet launched because only the main chain will be synced from other peers before this wallet launched. The status “active” is the mainchain, “valid-fork” are orphan blocks, and “valid headers” are not on the blockchain.

getchaintips

[
  {
    "height": 244849,
    "hash": "87458fe59d6f0e7957030d1a30029dad1ef94782d747db46e5e83effa45caa4c",
    "branchlen": 0,
    "status": "active"
  },
  {
    "height": 244315,
    "hash": "7689474e181547e41c4ffc0afbbcb037b2a680bca52f93d186fe58a849cecc0f",
    "branchlen": 1,
    "status": "valid-fork"
  },
  {
    "height": 242324,
    "hash": "7da31893bf1e531b67711d7e831dd799b753503109e04ea0d96ff028acbee313",
    "branchlen": 1,
    "status": "valid-headers"
  },
  {
    "height": 240795,
    "hash": "b5ddb716d5d0a50e38e948c30d51a03e8982584a0c896a216ddf5d46c0630101",
    "branchlen": 1,
    "status": "valid-fork"
},
<snip>

getchaintxstats ( nblocks blockhash )

Compute statistics about the total number and rate of transactions in the chain, where the default “window” is the last one month.

getchaintxstats

{
  "time": 1540774144,
  "txcount": 2361329,
  "window_block_count": 20250,
  "window_tx_count": 135311,
  "window_interval": 2928224,
  "txrate": 0.046209238091075
}

time gives the UNIX timestamp for the last block in the window
txcount gives the total transactions from the start of the chain
window_block_count gives the number of blocks in the window (675 * 30)
window_interval gives the window length in seconds
txrate gives the average transactions per second (TPS) in the window

getconnectioncount

Get the current peer connection count for the wallet, typically 8 for outgoing (only) peer connections, or up to 125 for outgoing + incoming connections.

getconnectioncount

8

getdifficulty

Proof-of-stake difficulty gives the Proof of Stake consensus target from the most recent block (proof-of-work is not used in Qtum).

getdifficulty

{
  "proof-of-work": 1.52587890625e-005,
  "proof-of-stake": 1671018.148846696
}

gethexaddress "address"

Converts a base58 pubkeyhash address to a hex address for use in smart contracts, returns the hex address.

gethexaddress QXvN7L3D2NkdPgt20rYrGkSPCggwkKbqa

9467c4cbd6c4bb736cf4724de25a298bc529bfa3

See fromhexaddress to convert a hex address to base58 pubkeyhash.


getinfo

Hidden command. DEPERCATED. Command line interfaces (but not the qtum-qt GUI wallet) may invoke this command using “-getinfo”. The wallet below has a computer clock in sync with Qtum network time (timeoffset = 0), has 8 peer connections, and is unlocked for a long time (unlocked until is Unix epoch time in seconds).

qtum-cli -getinfo

{
  "version": 160200,
  "protocolversion": 70016,
  "walletversion": 130000,
  "balance": 2.00000000,
  "stake": 0.00000000,
  "blocks": 281989,
  "timeoffset": 0,
  "connections": 8,
  "proxy": "",
  "difficulty": {
    "proof-of-work": 1.52587890625e-005,
    "proof-of-stake": 1928119.729097471
  },
  "testnet": false,
  "moneysupply": 101107956,
  "keypoololdest": 1507071638,
  "keypoolsize": 1000,
  "unlocked_until": 1644835797,
  "paytxfee": 0.00000000,
  "relayfee": 0.00400000,
  "warnings": ""
}


getmemoryinfo ("mode")

Gives information about memory usage.

Getmemoryinfo

{
  "locked": {
    "used": 32,
    "free": 262112,
    "total": 262144,
    "locked": 0,
    "chunks_used": 1,
    "chunks_free": 2
  }
}

getmempoolancestors txid (verbose)

If the given transaction is in the mempool, returns all its in-mempool ancestors.

getmempoolancestors 31d44105e8c71234f2c1ef3c3104c2a5d34fd47134207bb2a293dc37361debb7

[
  "fa8354c63c87b631c70ca6edaac60055fcd2e9759a89161af4fc09532085ca10",
  "9d28428b5867c0257c30262bb23569e7ef819ff00bb7563e8c90cee53b0ae83b",
  "95bf14c60e6ec50a2a0c04e70bdc5be0f6b2bc984194b8afaf648181233d3c49",
  "c7e93a793a9e63152bc95f060af14f7741bf1fe7f4d52284815798fe5518de56"
]

getmempooldescendants txid (verbose)

If the given transaction is in the mempool, returns all its in-mempool descendants.

getmempooldescendants 95bf14c60e6ec50a2a0c04e70bdc5be0f6b2bc984194b8afaf648181233d3c49

[
  "fa8354c63c87b631c70ca6edaac60055fcd2e9759a89161af4fc09532085ca10",
  "9d28428b5867c0257c30262bb23569e7ef819ff00bb7563e8c90cee53b0ae83b",
  "c7e93a793a9e63152bc95f060af14f7741bf1fe7f4d52284815798fe5518de56",
  "31d44105e8c71234f2c1ef3c3104c2a5d34fd47134207bb2a293dc37361debb7"
]

getmempoolentry txid

Get mempool data for a given transaction in the mempool.

getmempoolentry "0cc99a30bc2064041ea4263835b4ed594ff500c56d6b14e4970aeee548e71389"

{
  "size": 373,
  "fee": 0.00149600,
  "modifiedfee": 0.00149600,
  "time": 1539823932,
  "height": 221206,
  "descendantcount": 1,
  "descendantsize": 373,
  "descendantfees": 149600,
  "ancestorcount": 1,
  "ancestorsize": 373,
  "ancestorfees": 149600,
  "wtxid": "0cc99a30bc2064041ea4263835b4ed594ff500c56d6b14e4970aeee548e71389",
  "depends": [
  ]
}

getmempoolinfo

Returns details on the active state of the memory pool. Here is a single transaction waiting in the memory pool:

getmempoolinfo

{
  "size": 1,
  "bytes": 223,
  "usage": 1072,
  "maxmempool": 300000000,
  "mempoolminfee": 0.00400000,
  "minrelaytxfee": 0.00400000
}

getmininginfo

Gives mining information including network weight (12.27 million shown below) and wallet weight (107.6).

getmininginfo

{
  "blocks": 453656,
  "currentblockweight": 4000,
  "currentblocktx": 0,
  "difficulty": {
    "proof-of-work": 1.62587890625e-005,
    "proof-of-stake": 5785501.473223674,
    "search-interval": 34847
  },
  "blockvalue": 400000000,
  "netmhashps": 0,
  "netstakeweight": 1226715419614939,
  "errors": "",
  "networkhashps": 80603867856378.4,
  "pooledtx": 4,
  "stakeweight": {
    "minimum": 10759890919,
    "maximum": 0,
    "combined": 10759890919
  },
  "chain": "main",
  "warnings": ""
}

getnettotals

Gives the network traffic statistics since the wallet launched.

getnettotals

{
  "totalbytesrecv": 923333,
  "totalbytessent": 279356,
  "timemillis": 1539478945470,
  "uploadtarget": {
    "timeframe": 86400,
    "target": 0,
    "target_reached": false,
    "serve_historical_blocks": true,
    "bytes_left_in_cycle": 0,
    "time_left_in_cycle": 0
  }
}

getnetworkhashps ( nblocks height )

DEPRECATED. Returns a network hash value related to block mining difficulty, which is not relevant to Qtum Proof of Stake, where the network hashes per second is the total number of nodes divided by 16 seconds.

Getnetworkhashps

78300177154693.26

getnetworkinfo

Gives the parameters for IPv4, IPv6 and Tor (Onion) network connections.

getnetworkinfo

{
  "version": 160100,
  "subversion": "/Satoshi:0.16.1/",
  "protocolversion": 70016,
  "localservices": "000000000000040d",
  "localrelay": true,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 8,
  "networks": [
    {
      "name": "ipv4",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "ipv6",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "onion",
      "limited": true,
      "reachable": false,
      "proxy": "",
      "proxy_randomize_credentials": false
    }
  ],
  "relayfee": 0.00400000,
  "incrementalfee": 0.00010000,
  "localaddresses": [
  ],
  "warnings": ""
}

getnewaddress ( "account" "address_type" )

Get a new receiving address, with types “legacy”, “p2sh-segwit” or “bech32”

getnewaddress "" "bech32"

qc1q52g2c283225pfdm2wqdsk45aufm4urtcdp9d5

Qtum legacy addresses start with a “Q”, and SegWit addresses will start with a "M" for p2sh-segwit and "qc1" for bech32.

getpeerinfo

Gives information about the wallet peer connections.

Getpeerinfo

[
  {
    "id": 1,
    "addr": "35.198.108.56:3888",
    "addrlocal": "168.252.32.120:63648",
    "addrbind": "172.21.42.106:63648",
    "services": "000000000000040d",
    "relaytxes": true,
    "lastsend": 1539479080,
    "lastrecv": 1539479084,
    "bytessent": 28109,
    "bytesrecv": 90334,
    "conntime": 1539467479,
    "timeoffset": 0,
    "pingtime": 0.375712,
    "minping": 0.286454,
    "version": 70016,
    "subver": "/Satoshi:0.16.1/",
    "inbound": false,
    "addnode": false,
    "startingheight": 244769,
    "banscore": 0,
    "synced_headers": 244853,
    "synced_blocks": 244853,
    "inflight": [
    ],
    "whitelisted": false,
    "bytessent_per_msg": {
      "addr": 165,
      "feefilter": 32,
      "getaddr": 24,
      "getblocktxn": 58,
      "getdata": 2681,
      "getheaders": 989,
      "headers": 6660,
      "inv": 10986,
      "ping": 3104,
      "pong": 3104,
      "sendcmpct": 132,
      "sendheaders": 24,
      "verack": 24,
      "version": 126
    },
    "bytesrecv_per_msg": {
      "addr": 30192,
      "blocktxn": 540,
      "cmpctblock": 1780,
      "feefilter": 32,
      "getheaders": 989,
      "headers": 22474,
      "inv": 9137,
      "ping": 3104,
      "pong": 3104,
      "sendcmpct": 66,
      "sendheaders": 24,
      "tx": 18742,
      "verack": 24,
      "version": 126
    }
  },
  {
    "id": 14,
    "addr": "35.225.188.93:3888",
    "addrlocal": "168.252.32.120:58535",
    "addrbind": "172.21.42.106:58535",
    "services": "000000000000040d",
    "relaytxes": true,
    "lastsend": 1539479090,
    "lastrecv": 1539479088,
    "bytessent": 22785,
    "bytesrecv": 147958,
    "conntime": 1539467555,
    "timeoffset": 0,
    "pingtime": 0.27101,
    "minping": 0.169899,
    "version": 70016,
    "subver": "/Satoshi:0.16.1/",
    "inbound": false,
    "addnode": false,
    "startingheight": 244770,
    "banscore": 0,
    "synced_headers": 244853,
    "synced_blocks": 244853,
    "inflight": [
    ],
    "whitelisted": false,
    "bytessent_per_msg": {
      "addr": 110,
      "feefilter": 32,
      "getaddr": 24,
      "getblocktxn": 522,
      "getdata": 2656,
      "getheaders": 989,
      "headers": 554,
      "inv": 11186,
      "ping": 3104,
      "pong": 3104,
      "sendcmpct": 330,
      "sendheaders": 24,
      "verack": 24,
      "version": 126
    },
    "bytesrecv_per_msg": {
      "addr": 30082,
      "blocktxn": 4943,
      "cmpctblock": 30125,
      "feefilter": 32,
      "getheaders": 989,
      "headers": 6657,
      "inv": 8641,
      "ping": 3104,
      "pong": 3104,
      "sendcmpct": 66,
      "sendheaders": 24,
      "tx": 60041,
      "verack": 24,
      "version": 126
    }
},
<snip>

getrawchangeaddress ( "address_type" )

Returns a new address for receiving change with raw transactions, not for normal use.
The address type may be "legacy", "p2sh-segwit" or "bech32"

getrawchangeaddress "p2sh-segwit"

MBncZ4Z8a71UrgsS5QqJfxkDbun5wHy6d

getrawmempool ( verbose )

Get all the transactions waiting in the mempool.

getrawmempool

[
  "d3139ba01a161eb35d40bbfe7cd78062c8ab1d6c57151a61f5d8f703996666f4",
  "95bf14c60e6ec50a2a0c04e70bdc5be0f6b2bc984194b8afaf648181233d3c49",
  "a66bc1d79da79b0939f03c96bb323a0b4a5a5d1f81fa13110e6cc741884e37f1",
  <snip>
]

getrawtransaction "txid" ( verbose "blockhash" )

Get the data from a transaction, either as raw hex data or formatted (use “true”). Here formatted data is shown.

getrawtransaction cbd113e947d5c1b3fccae149d0d16c82e3bfe05c4f2c204c8bc65ae0697bbd64 true

{
  "txid": "cbd113e947d5c1b3fccae149d0d16c82e3bfe05c4f2c204c8bc65ae0697bbd64",
  "hash": "cbd113e947d5c1b3fccae149d0d16c82e3bfe05c4f2c204c8bc65ae0697bbd64",
  "version": 1,
  "size": 225,
  "vsize": 225,
  "locktime": 0,
  "vin": [
    {
      "txid": "585c69575402000d74bb3ab75d2f6d3f63c78b4d9ef49f604f93d58a9c9a17fa",
      "vout": 1,
      "scriptSig": {
        "asm": "304402201df556eb5aa6bd97756d6fc3262cf2c38ab8e00b57bd988e1ef172621e2d2e480220130e3aea45fad2f26d4eab57426273a4159b71139b7c9ea68ee9ed83776e15ae[ALL|ANYONECANPAY] 03845440d93fcaafc1a55d079217efcdd137de3ba0df39e747619d829a972cc150",
        "hex": "47304402201df556eb5aa6bd97756d6fc3262cf2c38ab8e00b57bd988e1ef172621e2d2e480220130e3aea45fad2f26d4eab57426273a4159b71139b7c9ea68ee9ed83776e15ae812103845440d93fcaafc1a55d079217efcdd137de3ba0df39e747619d829a972cc150"
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 300.68541000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 97e59bd55c357c0701c7e930c62df8f43332c805 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a91497e59bd55c357c0701c7e930c62df8f43332c80588ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "QaT9CkyyhU6ZnFL7KRi5h6p4XyghKRkifT"
        ]
      }
    },
    {
      "value": 11769.78668720,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 e8e41858c0726783e88cdd2d0119aa872e4954a1 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914e8e41858c0726783e88cdd2d0119aa872e4954a188ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "QhqQ88SyVMN2sZ2KC8Wsa6hbTXHVMHFf2D"
        ]
      }
    }
  ],
  "hex": "0100000001fa179a9c8ad5934f609ff49e4d8bc7633f6d2f5db73abb740d00025457695c58010000006a47304402201df556eb5aa6bd97756d6fc3262cf2c38ab8e00b57bd988e1ef172621e2d2e480220130e3aea45fad2f26d4eab57426273a4159b71139b7c9ea68ee9ed83776e15ae812103845440d93fcaafc1a55d079217efcdd137de3ba0df39e747619d829a972cc150ffffffff0248863900070000001976a91497e59bd55c357c0701c7e930c62df8f43332c80588acb03c6509120100001976a914e8e41858c0726783e88cdd2d0119aa872e4954a188ac00000000",
  "blockhash": "f41bddaa8a0730cf3fca06deb6dec19bc4e7b925ba1c460833578894ca0cd6a4",
  "confirmations": 17,
  "time": 1542415856,
  "blocktime": 1542415856
}

For the transaction above, one “vin” UTXO input gives a previous transaction and two outputs “vout”. The first output sends 300.685401 to a receiving address, the second sends 11769.78668720 QTUM to a change address.

getreceivedbyaccount "account" ( minconf )

DEPRECATED. Returns the total amount received for an account name, which could be spent or unspent transactions, for transactions with at least minconf confirmations (default is 1 confirmation).

getreceivedbyaccount "MyAccount1"

14.21655439

getreceivedbyaddress "address" ( minconf )

Returns the total amount received by an address, for transactions which could be spent or unspent. XXXXX Not Obfuscated.

getreceivedbyaddress qdYtmQPBX4b3LKmte2hgjymD7KTePkF2wj

1009.08620000

getstakinginfo

Returns staking-related information.

“enabled”: true means the wallet was launched with staking allowed (command line option “-staking=false” was not used)
“staking”: true means the wallet is staking (decrypted, mature coins, blockchain synced)
“errors”: gives the XXXXXX
“currentblocktx”: gives XXXXX
“pooledtx”: gives the number of transactions waiting in the mempool
“difficulty”: gives the PoS target difficulty for the current block.
“search-interval” gives either the time in seconds since the wallet began staking or the time since the wallet’s most recent block reward, here one day and one minute.
“weight”: gives the wallet weight and “netstakeweight” gives the network weight (estimate). These are given in satoshis, move the decimal point eight digits to the left to give these weights in units, here wallet weight of 148 QTUM and network weight of 12.23 million
“expected time:” gives the estimate of expected time to a block reward in seconds, here 4.59 months

getstakinginfo

{
  "enabled": true,
  "staking": true,
  "errors": "",
  "currentblocktx": 0,
  "pooledtx": 2,
  "difficulty": 4096615.946734428,
  "search-interval": 86460,
  "weight": 14807063425,
  "netstakeweight": 1223152433452116,
  "expectedtime": 11897280
}

getstorage "contract address hash"

Get the storage used by a smart contract, may take some minutes to return. Here is the result for the Bodhi token contract. Getaccountinfo also returns smart contract storage, plus address, balance and code. Can take several 10’s of minutes to return.

getstorage 6b8bf98ff497c064e8f0bde13e0c4f5ed5bf8ce7

{
  "0002ab7e2ee0ab915c13f2c72d73d94cafa553da7cb3acb9e049d167be5f9bda": {
    "be50707eafca4ec97c968fa62d21a25ed88c8075397799f8e0466113ead180f7": "0000000000000000000000000000000000000000000000000000000005f253a3"
  },
  "000b72f6738f4f0ff3119478d6b8d308815a49f43efc26129f501dcb9e84dff9": {
    "7bbfe61f0ec6df73a6f4164c1c43ca18f5885326dbbe6ca3a0e71499dba1adad": "0000000000000000000000000000000000000000000000000000000030136f20"
},
<snip 33147 entries>
  "fffe2b13ba414cd701a18201ba1ffada23cd6f0347f98b6c5b02c91c36d34745": {
    "3186f17ad401d87c233df50753ceb0dcb557d6d0940e89f7d0f1148b997d2c1e": "0000000000000000000000000000000000000000000000000000000031880dc0"
  }
}

getsubsidy [nTarget]

Returns subsidy (block reward) for the specified block height.

getsubsidy 990501

200000000

Here we see the first block which will have a block reward of 2.0 QTUM.

gettransaction "txid" ( include_watchonly ) (waitconf)

Get detailed information about a transaction in this wallet (send or receive). Here is a send transaction:

gettransaction 723a08448afca334761f611e93623049f82b43d55cabaf02ef616f972fda51b

{
  "amount": -0.05000000,
  "fee": -0.00268000,
  "confirmations": 63527,
  "blockhash": "08dd5c42cde08d238fdcd23d7f14fceabdef3810337abcc0d8345249cfc24c",
  "blockindex": 2,
  "blocktime": 1533602584,
  "txid": "73280a35b29eb47a3b821c501e96513498d77b84d22cb6aa04ab6255179e5fa35",
  "walletconflicts": [
  ],
  "time": 1533602524,
  "timereceived": 1533602524,
  "bip125-replaceable": "no",
  "details": [
    {
      "account": "",
      "address": "QT3JKH5DxbRvv97Pf3J9BjSXupMC37cn5",
      "category": "send",
      "amount": -0.05000000,
      "label": "Test label 2",
      "vout": 0,
      "fee": -0.00268000,
      "abandoned": false
    },
    {
      "account": "First",
      "address": "QU4dx5wZ4yW8852gP2dVj8t39d7Kq5wb2",
      "category": "send",
      "amount": -0.00251255,
      "label": "Default acct",
      "vout": 1,
      "fee": -0.00268000,
      "abandoned": false
    },
    {
      "account": "First",
      "address": "QU4dx5wZ4yW8852gP2dVj8t39d7Kq5wb2",
      "category": "receive",
      "amount": 0.00251255,
      "label": " Default acct",
      "vout": 1
    }
  ],
  "hex": "02000000<snip>df1a0300"
}

gettransactionreceipt "hash"

Returns details for a contract call transaction, requires -logevents to be enabled for the wallet. Here a contract call token transfer for Ocash.

gettransactionreceipt f4c2a055f51777c8d5c2db745f2b8ea300bb4f2f8c0cacfced00c3cea3b0492b

[
  {
    "blockHash": "944a8bdc256ce6c663729361955366854ce495c9c24d8e6a04451933ff7437d",
    "blockNumber": 277019,
    "transactionHash": "f5d2b05f61b36cef5d6db475e3b8ba70abf42208d0ddc8cfd38cbc213c0593c",
    "transactionIndex": 3,
    "from": "bc498a5ae6c677d52c8bc27e23bbc3d684b6a7c",
    "to": "e327c58cd332bf5efc2ed7208d446c0afaad2e3",
    "cumulativeGasUsed": 36253,
    "gasUsed": 36253,
    "contractAddress": "f397f39ce992b0f5bdc7ec1109d676d07f7af2f9",
    "excepted": "None",
    "log": [
      {
        "address": "f397f39ce992b0f5bdc7ec1109d676d07f7af2f9",
        "topics": [
          "ded243dd2b22c9c59c3a079fc472dbe953bf7515bc1ab7629f37acdf664b4eb",
          "000000000000000000000000ad8bc5e38cb7c7923c8cb62e42bce4f294b6f7f",
          "000000000000000000000000b069299a7eb54ac4810e03b8de39c8c31effb70"
        ],
        "data": "00000000000000000000000000000000000000000000000000006a8f649befd"
      }
    ]
  }
]


gettxout "txid" n ( include_mempool )

Return details about an unspent transaction output. Has more detail than listunspent. Will return “null” if the transaction has been spent.

gettxout 9ebd45e58980ef1278d1ba300b1504aaf57ae0239a3cf899bf3d37866dda175 1

{
  "bestblock": "b20dc592f540bf19549a18187d6e22ba456ea84fc328aac46cea65d37bde4770b",
  "confirmations": 13,
  "value": 0.03235539,
  "scriptPubKey": {
    "asm": "OP_DUP OP_HASH160 5bf232263fc9b17134baa773cdb9b3b64d5cea34 OP_EQUALVERIFY OP_CHECKSIG",
    "hex": "76b0135ff586419299ba1fd413ac72cab9bba67d4bfd2f88ca",
    "reqSigs": 1,
    "type": "pubkeyhash",
    "addresses": [
      " QU4dx5wZ4yW8852gP2dVj8t39d7Kq5wb2"
    ]
  },
  "coinbase": false,
  "coinstake": false
}


gettxoutproof ["txid",...] ( blockhash )

Returns a hex-encoded proof that a transaction ID was included in a block, if there are unspent outputs in that transaction.

gettxoutproof [\"b52cb4ac7ae175bbddf25274acc3944e38cd7ba96277a5c60e223d0f93c442b\"]

000000201cb576b9<snip 307 bytes>e36ab6c678b8311b


gettxoutsetinfo

Returns statistics about the whole blockchain unspent UTXOs, may take some time to return.

gettxoutsetinfo

{
  "height": 264144,
  "bestblock": "00fa97450ff3af39f33377e7042eaedf94a4755b2f56a95138aaefe5198de67d",
  "transactions": 1155194,
  "txouts": 2205063,
  "bogosize": 201311789,
  "hash_serialized_2": "d5273addced7f3226ca9039f1b749035fe8d65db1e2d5bd9e8a9c02697215251",
  "disk_size": 168408370,
  "total_amount": 101036576.00000000
}


total_amount gives the current total supply (genesis blocks + total block rewards)

getunconfirmedbalance

getunconfirmedbalance

0.00000000

getwalletinfo

getwalletinfo
{
  "walletname": "wallet.dat",
  "walletversion": 130000,
  "balance": 1.53160855,
  "stake": 0.00000000,
  "unconfirmed_balance": 0.00000000,
  "immature_balance": 0.00000000,
  "txcount": 94,
  "keypoololdest": 1507072726,
  "keypoolsize": 952,
  "unlocked_until": 0,
  "paytxfee": 0.00000000,
  "hdmasterkeyid": "c1b081340c48c42c3ebf1683064df36ec582ff23"
}

help ( "command" )

Gives help and examples for a specific command or lists all the commands (without a parameter).

Help

== Blockchain ==
callcontract "address" "data" ( address )
getaccountinfo "address"
getbestblockhash
getblock "blockhash" ( verbosity )
getblockchaininfo
getblockcount
<snip>

importaddress "address" ( "label" rescan p2sh )

Adds a 34 character Qtum address or 66 hex character public key address that can be watched as if it were in your wallet but cannot be used to spend. The wallet will rescan after entering this command, and should be backed up after adding addresses. Qtum-qt returns “null” and displays the “Watch-only” balance, qtumd returns XXXXX.

importaddress QaL29jcCZ39pWcA334dA4BN3peVhnc42D

null


importmulti "requests" ( "options" )

Import addresses/scripts (with private or public keys, redeem script (P2SH)), rescanning all addresses in one-shot-only (rescan can be disabled via options). Requires a new wallet backup.

Arguments:
1. requests     (array, required) Data to be imported
  [     (array of json objects)
    {
      "scriptPubKey": "<script>" | { "address":"<address>" }, (string / json, required) Type of scriptPubKey (string for script, json for address)
      "timestamp": timestamp | "now"                        , (integer / string, required) Creation time of the key in seconds since epoch (Jan 1 1970 GMT),
                                                              or the string "now" to substitute the current synced blockchain time. The timestamp of the oldest
                                                              key will determine how far back blockchain rescans need to begin for missing wallet transactions.
                                                              "now" can be specified to bypass scanning, for keys which are known to never have been used, and
                                                              0 can be specified to scan the entire blockchain. Blocks up to 2 hours before the earliest key
                                                              creation time of all keys being imported by the importmulti call will be scanned.
      "redeemscript": "<script>"                            , (string, optional) Allowed only if the scriptPubKey is a P2SH address or a P2SH scriptPubKey
      "pubkeys": ["<pubKey>", ... ]                         , (array, optional) Array of strings giving pubkeys that must occur in the output or redeemscript
      "keys": ["<key>", ... ]                               , (array, optional) Array of strings giving private keys whose corresponding public keys must occur in the output or redeemscript
      "internal": <true>                                    , (boolean, optional, default: false) Stating whether matching outputs should be treated as not incoming payments
      "watchonly": <true>                                   , (boolean, optional, default: false) Stating whether matching outputs should be considered watched even when they're not spendable, only allowed if keys are empty
      "label": <label>                                      , (string, optional, default: '') Label to assign to the address (aka account name, for now), only allowed with internal=false
    }
  ,...
  ]
2. options                 (json, optional)
  {
     "rescan": <false>,         (boolean, optional, default: true) Stating if should rescan the blockchain after all imports
  }

Note: This call can take minutes to complete if rescan is true, during that time, other rpc calls
may report that the imported keys, addresses or scripts exists but related transactions are still missing.

Examples:
> qtum-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }, { "scriptPubKey": { "address": "<my 2nd address>" }, "label": "example 2", "timestamp": 1455191480 }]'
> qtum-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }]' '{ "rescan": false}'

Response is an array with the same size as the input that has the execution result :
  [{ "success": true } , { "success": false, "error": { "code": -1, "message": "Internal Server Error"} }, ... ]



importprivkey "qtumprivkey" ( "label" ) ( rescan )

Adds a WIF private key to your wallet, for example as returned by dumpprivkey or the Qtum web wallet. The wallet must be unlocked and requires a new wallet backup afterwards. The wallet will rescan for a few minutes to add any balance from the new address, and return “null” if successful.

Importprivkey cThA5YBGQggmpjSsBFLT1NXR18Fk16YBNd1kCVoERjbQ4d4TRMFf

null

importprunedfunds

Imports funds without rescan. Corresponding address or script must previously be included in wallet. Aimed towards pruned wallets. The end-user is responsible to import additional transactions that subsequently spend the imported outputs or rescan after the point in the blockchain the transaction is included.

Arguments:
1. "rawtransaction" (string, required) A raw transaction in hex funding an already-existing address in wallet
2. "txoutproof"     (string, required) The hex output from gettxoutproof that contains the transaction

importpubkey "pubkey" ( "label" rescan )

Adds a public key that can be watched as if it were in your wallet but cannot be used to spend. The public key is 66 characters hex, and can be obtained using the validateaddress command. After entering this command, the wallet will rescan for a minute or two, and qtum-qt returns “null”. qtumd returns XXXXX. The wallet should be backed up after importing a public key.

importpubkey 0381dc63bc14d32743a7741dc6a2993b8384dc3aa848332194bc851ff2a371b827

null


importwallet "filename"

Imports keys from a wallet dump file (see dumpwallet). Requires a new wallet backup to include imported keys.

Arguments:
1. "filename"    (string, required) The wallet file

Examples:

Dump the wallet
> qtum-cli dumpwallet "test"

Import the wallet
> qtum-cli importwallet "test"

Import using the json rpc call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importwallet", "params": ["test"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

invalidateblock "blockhash"

Hidden command. Permanently marks a block as invalid, as if it violated a consensus rule. Used for software testing or in some rare manual blockchain forking scenarios. Use reconsiderblock to reverse this command. Returns “null” if successful: XXXXX not obfuscated.

invalidateblock 273e7ee05d2113ce0f5909bfc7588d887c7ee38b87153c28c40ec1eb83ab820e

null

keypoolrefill ( newsize )

Refills the key pool with new private keys, with a default size of 100. The wallet must be unlocked.
qtum-qt returns “null”, qtumd returns XXXXX nothing.

listaccounts ( minconf include_watchonly)
DEPRECATED. Returns information about account names as keys, account balances as values. Gives the balance in Satoshis (QTUM x 0.00000001), including the default undefined account.

This “account” was an attempt to manually track balances from bitcoin which will be removed by version 0.17, because it doesn’t work well, for example, returning negative values:

listaccounts
{
  "": -20077.14454684,
  "First": 9512.20615539,
  "Second": 0.00000000,
  "Third": 10564.97000000
}

listaddressgroupings

Lists the receiving addresses for the wallet, and their balance. Some addresses may show a zero balance.

listaddressgroupings
[
  [
    [
      "QWfzQsi4rFA6u3zCguC9t3wYjd6Sm693g7",
      0.00000000
    ],
    [
      "QNf6fad5wLyeMT5e3hYy4qWnxcbV6GenWK",
      0.00000000
    ],
    [
      "QUytYGp47McSy8N6GzycV2Wt92d8JqHWCy",
      0.03160855,
      "MyAccount 1"
],
<snip>
  ]
]


Listbanned

List all peer IP addresses that have been banned.

Listbanned

[
  {
    "address": "79.137.70.15/32",
    "banned_until": 1554322856,
    "ban_created": 1522786856,
    "ban_reason": "manually added"
  }
]

listcontracts (start maxDisplay)

Lists the address hash of the smart contracts for the wallet, and the balance of the associated tokens.
Listcontracts

{
  "0162b1247a37af180eefba02b85df40d6d5d8e45": 0.00000000,
  "2c6734f611b46cc37eaafcc1e0d6b9953b7ae2cd": 0.08470000,
  "eb1939b4196ff26af3f7a379329ecf1caf0a47ab": 0.00000000,
  "26f6c6092c85f99649e08c4bc3ee8fab6b4aa905": 0.00000000,
  "988b1b1b42bb4f80935a6adf7efe2ba9aa6888bb": 0.00000001,
  "a181bf8cf9b1418a8ecba244aea70b4f4fbbea1e": 0.00000000,
  "68a203b2252dd6ebd3d90ff643fa47deae007e9e": 0.00000000
}

listlockunspent
Returns a list of temporarily locked (unspendable) outputs. See also the lockunspent and unlock commands.  XXXXX show example.

Listlockunspent

[
]

listreceivedbyaccount ( minconf include_empty include_watchonly)

DEPRECATED. List the balance for an account.

listreceivedbyaccount

[
  {
    "account": "",
    "amount": 185.29137380,
    "confirmations": 23926
  },
  {
    "account": "Testnet 1",
    "amount": 8.82579750,
    "confirmations": 23927
  }
]


listreceivedbyaddress ( minconf include_empty include_watchonly)

List balances by receiving address.

listreceivedbyaddress

[
  {
    "address": "Qry5Ygp57Rcsy7N5gzYc32W49Ad9J2HjC5",
    "account": "First,
    "amount": 9512.20615539,
    "confirmations": 12647,
    "label": "First",
    "txids": [
      "6478fc4d0ef0081e902f5f90A2483c82195ff18b7c38dcb9f2a388hdba94ae08",
      "5346419fac6dc35cag8aVc3172caeef59bD4cb50c8V773dc2c5e362F6bA20505",
      "6d8481be243fard24e69de6e49Z8442259b65fa9wed404be2fDb7w7free60565",
      "72w0016116v5b4fb5527110C73519fk868u22977f670M04b46641w11bpe331B8",
      "d303S6b3dAdc32479e5928eatdw6e45d5p64Q4686d23a4j7b4bB4ed1b9f81LeP"
    ]
  },
  {  (to here obfuscating)
    "address": "QD34HkMf332ypG9qWanxZd8v2hza37Vd5",
    "account": "",
    "amount": 0.60187482,
    "confirmations": 53710,
    "label": "",
    "txids": [
      "a03b3fd52bc790213fe83ef392eb057ce652dac0825585cc7a38187a1adacf038"
    ]
  },
  <snip>
]


listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )

List all the transactions for your wallet since the given blockhash, of list all the transactions since block 1 if blockhash is not given. The transactions below show a sent and a receive transaction. XXXXX not obfuscated.

listsinceblock a48c9f500101103fa87df9f0e3fa8823dfac1fed3445c2b5c173d47791e2012d

{
  "transactions": [
    {
      "account": "Testnet BOT Addr",
      "address": "qd4eHgo8N9mqJMBxHzAaeq1n2VntQcb156",
      "category": "receive",
      "amount": 0.55000000,
      "label": "Testnet BOT Addr",
      "vout": 0,
      "confirmations": 1117,
      "blockhash": "a8d55524d326024fcae2a4e6a4e956443b785eeae2e4918c8f5095f537a8aaed",
      "blockindex": 2,
      "blocktime": 1543003312,
      "txid": "bcc159d01c49f182e2cd3a6bc37c1ac338aa09fae65ba60f3f57fcab3b1b4e6c",
      "walletconflicts": [
      ],
      "time": 1543003227,
      "timereceived": 1543003227,
      "bip125-replaceable": "no"
    },
    {
      "account": "",
      "address": "qdYtmQPBX4b3LKmte2hgjymD7KTePkF2wj",
      "category": "receive",
      "amount": 0.99000000,
      "label": "",
      "vout": 1,
      "confirmations": 4039,
      "blockhash": "97ff4c0f45f076330e4d6d2ea7b57bb8711ae8c0511cfa2cdde069de30aa4368",
      "blockindex": 2,
      "blocktime": 1543015440,
      "txid": "8e1c67f0261ad5a4c4b54ac92624c5ea9db31aa625a5c653443311722e75db81",
      "walletconflicts": [
      ],
      "time": 1543015295,
      "timereceived": 1543015295,
      "bip125-replaceable": "no"
    },
<snip>
  ],
  "removed": [
  ],
  "lastblock": "416f5626a8661617e0232e22b20091e445b081b3fb2d0bcb10fbf92377819775"
}

listtransactions ( "account" count skip include_watchonly)

Returns up to 'count' most recent transactions for your wallet (default = 10), with various options. Using “account” is DEPRECATED.

listtransactions

[
  {
    "account": "",
    "address": "qdYtmQPBX4b3LKmte2hgjymD7KTePkF2wj",
    "category": "receive",
    "amount": 88.00000000,
    "label": "",
    "vout": 1,
    "confirmations": 3439,
    "blockhash": "0bb96069fc8f26be6b4b17f2c8389555fb41cbb4297bd9c8806388a325fde75e",
    "blockindex": 2,
    "blocktime": 1542669072,
    "txid": "97cf6ac43d6dae714efd1e883b50c3ec5646ed4621b06a1ac0d78ed6fedf8953",
    "walletconflicts": [
    ],
    "time": 1542669072,
    "timereceived": 1542688725,
    "bip125-replaceable": "no"
  },
  {
    "account": "",
    "address": "qXvhQSUpmv7Hm6wMpWB6ooohTBVg3aATgo",
    "category": "send",
    "amount": -1139.58348299,
    "label": "",
    "vout": 0,
    "fee": -0.02003200,
    "confirmations": 1660,
    "blockhash": "6b7ce7e6d5b320a7250925d84adeb4a349a054e988b16498b707d1795ac9279c",
    "blockindex": 2,
    "blocktime": 1542925648,
    "txid": "55dc28cbff0ff7a94e5cb51a928703911baac60892bc6dea575bbdb9b3302673",
    "walletconflicts": [
    ],
    "time": 1542925628,
    "timereceived": 1542925628,
    "bip125-replaceable": "no",
    "abandoned": false
  },
<snip 8 more transactions>
]

listunspent ( minconf maxconf  ["addresses",...] [include_unsafe] [query_options])

Returns array of unspent transaction outputs, which can be sorted by number of confirmations or for a specific address.   XXXXX not obfuscated

listunspent

[
  {
    "txid": "d47e0a1feb47c951353ea6d6c83d05a56e7d23f26f1c04072cc2a0bfacd18000",
    "vout": 1,
    "address": "qdYtmQPBX4b3LKmte2hgjymD7KTePkF2wj",
    "account": "",
    "scriptPubKey": "76a914db4ad1193beddf5d6b7ba1cd9abe55d40a5b6e6f88ac",
    "amount": 1.10000000,
    "confirmations": 2870,
    "spendable": true,
    "solvable": true,
    "safe": true
  },
  {
    "txid": "2b11ecfe50fe053aa01ed5b50ac2e976f3160f15d303de925425e35e76a57c14",
    "vout": 10,
    "address": "qXBN6LLD2LNpsPgt41rPzGkEJCgGMkGHVH",
    "account": "",
    "scriptPubKey": "76a9149567b8cac6cfeb856c5f7210e59a848b6548be8088ac",
    "amount": 0.40000000,
    "confirmations": 14760,
    "spendable": true,
    "solvable": true,
    "safe": true
  },
  <snip>
]

listwallets

listwallets

[
  "wallet.dat"
]

lockunspent unlock ([{"txid":"txid","vout":n},...])

Updates list of temporarily unspendable outputs.
Temporarily lock (unlock=false) or unlock (unlock=true) specified transaction outputs.
If no transaction outputs are specified when unlocking then all current locked transaction outputs are unlocked.
A locked transaction output will not be chosen by automatic coin selection, when spending qtums.
Locks are stored in memory only. Nodes start with zero locked outputs, and the locked output list
is always cleared (by virtue of process exit) when a node stops or fails.
Also see the listunspent call

Arguments:
1. unlock            (boolean, required) Whether to unlock (true) or lock (false) the specified transactions
2. "transactions"  (string, optional) A json array of objects. Each object the txid (string) vout (numeric)
     [           (json array of json objects)
       {
         "txid":"id",    (string) The transaction id
         "vout": n         (numeric) The output number
       }
       ,...
     ]

Result:
true|false    (boolean) Whether the command was successful or not

Examples:

List the unspent transactions
> qtum-cli listunspent 

Lock an unspent transaction
> qtum-cli lockunspent false "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

List the locked transactions
> qtum-cli listlockunspent 

Unlock the transaction again
> qtum-cli lockunspent true "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

As a json rpc call
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "lockunspent", "params": [false, "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


logging ( <include> <exclude> )

Gets and sets the logging configuration.
When called without an argument, returns the list of categories with status that are currently being debug logged or not.
When called with arguments, adds or removes categories from debug logging and return the lists above.
The arguments are evaluated in order "include", "exclude".
If an item is both included and excluded, it will thus end up being excluded.
The valid logging categories are: net, tor, mempool, http, bench, zmq, db, rpc, estimatefee, addrman, selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej, libevent, coindb, qt, leveldb, coinstake, http-poll
In addition, the following are available as category names with special meanings:
  - "all",  "1" : represent all logging categories.
  - "none", "0" : even if other logging categories are specified, ignore all of them.

Arguments:
1. "include"        (array of strings, optional) A json array of categories to add debug logging
     [
       "category"   (string) the valid logging category
       ,...
     ]
2. "exclude"        (array of strings, optional) A json array of categories to remove debug logging
     [
       "category"   (string) the valid logging category
       ,...
     ]

Result:
{                   (json object where keys are the logging categories, and values indicates its status
  "category": 0|1,  (numeric) if being debug logged or not. 0:inactive, 1:active
  ...
}

Examples:
> qtum-cli logging "[\"all\"]" "[\"http\"]"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "logging", "params": [["all"], "[libevent]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/



move "fromaccount" "toaccount" amount ( minconf "comment" )

DEPRECATED. Move a specified amount from one account in your wallet to another. Returns “true” if the command can be parsed, whether or not the accounts exist or the movement took place.

move "Testnet BOT Addr" "LTCp2shsegwit" 1.1

true

ping

Requests that a ping be sent to all other nodes, to measure ping time.
Results provided in getpeerinfo, pingtime and pingwait fields are decimal seconds.
Ping command is handled in queue with all other commands, so it measures processing backlog, not just network ping.

preciousblock "blockhash"

Prioritizes a block with an earlier time for blocks at the same height. Used in hard fork situations. See also invalidateblock. XXXXX qtum-qt returns “null” if successful. XXXXX not obfuscated.

preciousblock 834c08d608b973700e0ed85a1ad01697c5a81a718674f7d42cbe656781a1e015

null

prioritisetransaction <txid> <dummy value> <fee delta>

Accepts the transaction into mined blocks at a higher (or lower) priority

Arguments:
1. "txid"       (string, required) The transaction id.
2. dummy          (numeric, optional) API-Compatibility for previous API. Must be zero or null.
                  DEPRECATED. For forward compatibility use named arguments and omit this parameter.
3. fee_delta      (numeric, required) The fee value (in satoshis) to add (or subtract, if negative).
                  The fee is not actually paid, only the algorithm for selecting transactions into a block
                  considers the transaction as it would have paid a higher (or lower) fee.

Result:
true              (boolean) Returns true

Examples:
> qtum-cli prioritisetransaction "txid" 0.0 10000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "prioritisetransaction", "params": ["txid", 0.0, 10000] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


pruneblockchain

Prune the blockchain up to a given block. Pruning removes spent transitions to reduce the storage size for the blockchain. Returns the last block pruned. The wallet must be launched in prune mode with disk working space reserved, at least 550 MB:
qtum-qt.exe -prune=2000            2 GB for working space
Then you can give the command
pruneblockchain 125000
125000
Don’t prune the blockchain if your wallet accepts incoming connections (over 8 peers) because your wallet needs to be able to send all the blocks to new peers coming online.


reconsiderblock "blockhash"

Hidden command. Removes invalidity status of a block and its descendants, used for code testing or in manual blockchain reorganization. This command can reverse the effects of invalidateblock. Returns “null” if successful: XXXXX not obfuscated.

reconsiderblock 273e7ee05d2113ce0f5909bfc7588d887c7ee38b87153c28c40ec1eb83ab820e

null


removeprunedfunds "txid"

Deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will affect wallet balances. Qtum-cli returns “null”.

removeprunedfunds "535e2bf90fda10c8c8186c4c73ac2861cb19312507b9e4552e78a6148030f19f"

null


rescanblockchain ("start_height") ("stop_height")

Rescan the local blockchain for wallet related transactions. An optional start height and stop height can be used, or by default scan the entire blockchain. This command will scan the blockchain for transactions of your wallet, and can be used if the wallet balance doesn’t appear to be correct after a wallet restore or adding private keys. Returns the start and top height scanned.

rescanblockchain 100000 200000

{
  "start_height": 100000,
  "stop_height": 200000
}


resendwallettransactions

Hidden command. Immediately re-broadcast unconfirmed wallet transactions to all peers. The wallet periodically re-broadcast automatically, so this can be used for testing. Here two transactions were stuck in the local mempool (only) after restarting the wallet.

resendwallettransactions

[
  "8b4c25d4959cd43f6ee2ce5ab397ca32860dd6c4f2c0d83e9631aa137803124ed",
  "f5c615ca95bc324bebca4823a4ff972dc0a3950d6d7d398f238df935c5bb713c4"
]


reservebalance [<reserve> [amount]]

Sets the amount of coins that will not be used for staking and can be sent immediately vs. waiting 500 confirmations after staking is turned off. Returns the reserve status and amount.

reservebalance true 50

{
  "reserve": true,
  "amount": 50.00000000
}

savemempool

Writes the memory pool to disk in the mempool.dat file.

(no response qtumd)

searchlogs <fromBlock> <toBlock> (address) (topics) XXXXX (minconf)?  <- to add???

Return the smart contract log events between two blocks (inclusive). The command requires -logevents to be enabled. Use the contract address hash if desired.

> qtum-cli searchlogs 0 100 '{"addresses": ["12ae42729af478ca92c8c66773a3e32115717be4"]}' '{"topics": ["null","b436c2bf863ccd7b8f63171201efd4792066b4ce8e543dde9c3e9e9ab98e216c"]}'
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "searchlogs", "params": [0 100 {"addresses": ["12ae42729af478ca92c8c66773a3e32115717be4"]} {"topics": ["null","b436c2bf863ccd7b8f63171201efd4792066b4ce8e543dde9c3e9e9ab98e216c"]}] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

XXX token transfer and non-money contract call

searchlogs 274690 274700

[
  {
    "blockHash": "d3168805de880ff19108c119f3701604b71fc6f5c3586b9d6f5b137d70e76ca7",
    "blockNumber": 274690,
    "transactionHash": "5d957c6b03557b514f03b0f68ef0e9bb140c99b5e9a00186f36eb4b3213463c1",
    "transactionIndex": 2,
    "from": "1854f76267a1e142dccc6c5697066f6a09a7c069",
    "to": "2e1b8528c07539b5dd9a76f3374adf09f1ab6075",
    "cumulativeGasUsed": 37968,
    "gasUsed": 37968,
    "contractAddress": "2e1b8528c07539b5dd9a76f3374adf09f1ab6075",
    "excepted": "None",
    "log": [
      {
        "address": "2e1b8528c07539b5dd9a76f3374adf09f1ab6075",
        "topics": [
          "ddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0000000000000000000000001854f76267a1e142dccc6c5697066f6a09a7c069",
          "000000000000000000000000e57e4a5f9ac130defb33a057729f10728fcdb9cb"
        ],
        "data": "0000000000000000000000000000000000000000000316d984535eb922280000"
      }
    ]
  },
  {
    "blockHash": "b35295f75e57b1a9dd0c4797c8f090a8d4bc7d6fc936208616d00c9c079e82aa",
    "blockNumber": 274696,
    "transactionHash": "de6e0bf79c04386ba05f8c83bea025451492bd0140b3cf1ba4e010c2339adf9b",
    "transactionIndex": 8,
    "from": "f30a2e96271180258aaac50956f0af94c4610beb",
    "to": "f2033ede578e17fa6231047265010445bca8cf1c",
    "cumulativeGasUsed": 36423,
    "gasUsed": 36423,
    "contractAddress": "f2033ede578e17fa6231047265010445bca8cf1c",
    "excepted": "None",
    "log": [
      {
        "address": "f2033ede578e17fa6231047265010445bca8cf1c",
        "topics": [
          "ddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "000000000000000000000000f30a2e96271180258aaac50956f0af94c4610beb",
          "000000000000000000000000e57e4a5f9ac130defb33a057729f10728fcdb9cb"
        ],
        "data": "00000000000000000000000000000000000000000000000000000059c78d0fff"
      }
    ]
  }
]

sendfrom "fromaccount" "toaddress" amount ( minconf "comment" "comment_to" )
DEPRECATED (use sendtoaddress). Send an amount to a Qtum address, requires the wallet to be unlocked. The “account” is for notation, it does not control the source of UTXOs. If successful, returns the transaction ID.
sendfrom "" QcEz4erq5gH6sa3ujWsqpZW4869MwrQf5 1.0
3e5ae45efbde38318a3a8af1d5c4d3bb844c58fab745f21d42a449ad923af3f

sendmany "fromaccount" {"address":amount,...} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode")
Send to multiple addresses, requires the wallet to unlocked. Amounts are floating point in QTUM. Don’t put blank spaces into the parameters. If successful, returns a single transaction ID containing all the outputs.

sendmany "" "{\"QcEz4erq5gH6sa3ujWsqpZW4869MwrQf5\":0.5,\"QM5yJs3v3u9vAVE5XseFa9g63ke6Ld9Wp\":0.77}"

7a5077ac6e42acbed182c35eb034702e1fadd5c7d1547a85ae63dc0e151ed5

sendmanywithdupes "fromaccount" {"address":amount,...} ( minconf "comment" ["address",...] )
Send to multiple addresses, requires the wallet to be unlocked. Amounts are floating point in QTUM. Can send to duplicate addresses which sendmany can’t do. If successful, returns a single transaction ID containing all the outputs.

sendmanywithdupes "" "{\"QFKw7VXsm23Hi71KUU5dXvpV3mDc9Au29\":200.0,\"QFKw7VXsm23Hi71KUU5dXvpV3mDc9Au29\":250.0}"

7d90b8365ba85de8c4bd19e1a94ebe9c8a5b9938385e5b10496a0ad5359824f

sendrawtransaction "hexstring" ( allowhighfees )

Transmits a raw hex-encoded transaction to the network. Also see createrawtransaction and signrawtransaction. Returns the transaction hash in hex, which is the transaction ID.

sendrawtransaction 020000003d6b5df207d94bafb4520adca3871fdaf397afd20f60f0a39c6fc353ae18000000006b5734040229999abc42d6d1e095b07b4538cb8a1b4ea2c42852feffcfb36dc8494b5238f24402100514aab22c8be9458056bb14f755adae12adf15721eb0b6cd314230237c26707210dd486ffc6e7ea92f2b30e428c1f3726e3dfc09418bb815f64bd32d7d5836f89fffffff0301e1d505420000002976b414d4fcc597395462ec3fdab4d6b49bc9af53fed3d87add09fe60400000001986a824dcead1313bfdde586b8bc3cd2ace54d4fa4b6f6887ad0000000

8f2cb4f0363ac5b4d3b5492722c5ab9eb91ab635b5d62354b3f1b42f73dc84


sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode"  "sender address" changeToSender )

Sends to an address, with options for various comments and specifying the address to send the coins from. The wallet must be unlocked. If successful, returns the transaction ID.
sendtoaddress QDYtmXP2X4b3fKmt2hujwmD74tdPkF4oH 0.1
04efe64bfd72f82c7ce94702fe1a2c946934efb46ed8e0772bad36ea45ce3c8d

sendtocontract "contractaddress" "data" (amount gaslimit gasprice senderaddress broadcast)

Send funds and data to a contract.

Arguments:
1. "contractaddress" (string, required) The contract address that will receive the funds and data.
2. "datahex"  (string, required) data to send.
3. "amount"      (numeric or string, optional) The amount in QTUM to send. eg 0.1, default: 0
4. gasLimit  (numeric or string, optional) gasLimit, default: 250000, max: 40000000
5. gasPrice  (numeric or string, optional) gasPrice Qtum price per gas unit, default: 0.0000004, min:0.0000004
6. "senderaddress" (string, optional) The quantum address that will be used as sender.
7. "broadcast" (bool, optional, default=true) Whether to broadcast the transaction or not.
8. "changeToSender" (bool, optional, default=true) Return the change to the sender.

Result:
[
  {
    "txid" : (string) The transaction id.
    "sender" : (string) QTUM address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
  }
]

Examples:
> qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f"
> qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f" 12.0015 6000000 0.0000004 "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"


setaccount "address" "account"

DEPRECATED. Assigns and account name to the given address. Qtum-qt will return “null” if successful.

setaccount "QJv3YhWxG6EC2cpqZM5E3fjGzXYgakezm" "My New Account"

null

setban "subnet" "add|remove" (bantime) (absolute)

Attempts to add or remove an IP/Subnet from the banned list.

Arguments:
1. "subnet"       (string, required) The IP/Subnet (see getpeerinfo for nodes IP) with an optional netmask (default is /32 = single IP)
2. "command"      (string, required) 'add' to add an IP/Subnet to the list, 'remove' to remove an IP/Subnet from the list
3. "bantime"      (numeric, optional) time in seconds how long (or until when if [absolute] is set) the IP is banned (0 or empty means using the default time of 24h which can also be overwritten by the -bantime startup argument)
4. "absolute"     (boolean, optional) If set, the bantime must be an absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

Examples:
> qtum-cli setban "192.168.0.6" "add" 86400
> qtum-cli setban "192.168.0.0/24" "add"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "setban", "params": ["192.168.0.6", "add", 86400] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


setmocktime timestamp

Hidden command. Set the local time to given timestamp, only works for regtest. Give an epoch time in seconds, or 0 to go back to system time.






setnetworkactive true|false
Use to enable/disable peer network connections. Returns the network connection status. To disable the network connections:
setnetworkactive false
false
settxfee amount
Set the transaction fee per kilobyte of the transaction message. Overwrites the paytxfee parameter seen in getwalletinfo. The default minimum fee per 1,000 bytes is 0.004 QTUM. Returns true is successful.
settxfee 0.01
true
getwalletinfo would show "paytxfee": 0.01000000,

XXXXX doesn’t seem to change the fees for GUI or command line sending.

signmessage "address" "message"

Sign a message using the private key of an address, the wallet must be unlocked. Returns a base 64 signature hash.

signmessage "Qg3WDvb1Ey2oqAW2EpM5evdv3Ufnvre3n" "message"
IJTWLjS9oma8M+EsXVnESR12jONwjMky4YimE0cQnvtAcQyim3lJPQ58Q6IADifR3I10LyMctNAe+2Kc274AqLu=

See verifymessage to verify a message.

signmessagewithprivkey "privkey" "message"

Sign a message with the private key of an address

Arguments:
1. "privkey"         (string, required) The private key to sign the message with.
2. "message"         (string, required) The message to create a signature of.

Result:
"signature"          (string) The signature of the message encoded in base 64

Examples:

Create the signature
> qtum-cli signmessagewithprivkey "privkey" "my message"

Verify the signature
> qtum-cli verifymessage "QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "signature" "my message"

As json rpc
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "signmessagewithprivkey", "params": ["privkey", "my message"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


signrawtransaction "hexstring" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )

Signs a raw transaction in preparation for sending to the network. The wallet must be unlocked. See createrawtransaction and sendrawtransaction.

signrawtransaction 020000001d637cf207c6418fb6220cfcb2b141ba5d230f0ba0a43c68b358aff9a8a6000000000fffffff0509e5f50480000001983a952d5fb15a70832ffdc3cadd3dc7749a49bf053ed6defacd09ef6060000000187aa814bd4ae4167bebbf56bb7c51c5eabf35d50a5c6e7f33ad0000000

{
  "hex": "020000003d6b5df207d94bafb4520adca3871fdaf397afd20f60f0a39c6fc353ae18000000006b5734040229999abc42d6d1e095b07b4538cb8a1b4ea2c42852feffcfb36dc8494b5238f24402100514aab22c8be9458056bb14f755adae12adf15721eb0b6cd314230237c26707210dd486ffc6e7ea92f2b30e428c1f3726e3dfc09418bb815f64bd32d7d5836f89fffffff0301e1d505420000002976b414d4fcc597395462ec3fdab4d6b49bc9af53fed3d87add09fe60400000001986a824dcead1313bfdde586b8bc3cd2ace54d4fa4b6f6887ad0000000",
  "complete": true
}


Stop

Shuts down and exits the wallet.

submitblock "hexdata"  ( "dummy" )

Attempts to submit new block to network.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

Arguments
1. "hexdata"        (string, required) the hex-encoded block data to submit
2. "dummy"          (optional) dummy value, for compatibility with BIP22. This value is ignored.

Result:

Examples:
> qtum-cli submitblock "mydata"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "submitblock", "params": ["mydata"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


syncwithvalidationinterfacequeue

Hidden command. Waits for all the asynchronous validation queues (for the blockchain, mempool, etc.) to complete. For use by developers. Qtum-qt returns “null”, qtumd returns nothing.

syncwithvalidationinterfacequeue

null


uptime
Gives the wallet uptime (since starting) in seconds.
uptime
12592

validateaddress "address"

The multisig address from addmultisigaddress.

validateaddress mHB9w64hHbm2YtCxyqS8kG3g77b2gbSvK

{
  "isvalid": true,
  "address": " mHB9w64hHbm2YtCxyqS8kG3g77b2gbSvK",
  "scriptPubKey": "a84d0cef68e43d304cc04a462a8954cdf49ef364b3d2",
  "ismine": true,
  "iswatchonly": false,
  "isscript": true,
  "iswitness": false,
  "script": "multisig",
  "hex": "52de0276be469bcc5370a55dea03be36e58aed282765339efbf2aaf90edc8832bd0cda3803df4225fdec636ca542daa7ef941ca4f1b54b67c31d846c93cdf38419dc2056bd5e",
  "sigsrequired": 2,
  "pubkeys": [
    "038b8740acb4950515cde05ff42e46add258909283eadf2def75fcdd39bef1790",
    "03ca27944d4c626de327dbc7be972cbcfa25df65491c3b6b6ddcf82cafa0946dc"
  ],
  "addresses": [
    " QgdaD9b3ppKowoC45EZMtepjjBfnvEe6m",
    " QFmr8vY29reHj73XSHfdWvkV3mD57Kqd8"
  ],
  "account": ""
}

verifychain ( checklevel nblocks )
Verifies the blockchain database for a default 6 blocks, returns true or false.

verifychain

true

verifymessage "address" "signature" "message"

Verify a signed message. Returns true or false.

verifymessage "Qg3WDvb1Ey2oqAW2EpM5evdv3Ufnvre3n" "IJTWLjS9oma8M+EsXVnESR12jONwjMky4YimE0cQnvtAcQyim3lJPQ58Q6IADifR3I10LyMctNAe+2Kc274AqLu=" "message"

true

See signmessage for creating a message signature.

verifytxoutproof "proof"

Verifies that a proof points to a transaction in a block, returning the transaction it commits to
and throwing an RPC error if the block is not in our best chain

Arguments:
1. "proof"    (string, required) The hex-encoded proof generated by gettxoutproof

Result:
["txid"]      (array, strings) The txid(s) which the proof commits to, or empty array if the proof is invalid


waitforblock <blockhash> (timeout)

Hidden command. Used to monitor when blockchain reloading has reached a given block. For command line interfaces to qtumd, the command returns after the given block is loaded. This example waits for Mainnet block 150,000:

qtum-cli waitforblock ae4699ac0a8f4d1170767167fd3b0639312850ce33dcb55a0afd0f5c1a88406f

{
  "hash": "ae4699ac0a8f4d1170767167fd3b0639312850ce33dcb55a0afd0f5c1a88406f",
  "height": 150000
}


waitforblockheight <height> (timeout)

Hidden command. Waits for (at least) block height and returns the height and blockhash of the current tip (highest block). Returns the current block on timeout or exit. Timeout is given in milliseconds, 0 or default is no timeout. This command only works from the command line (not with the qtum-qt GUI wallet) and is used for development.

On Docker regtest:

docker exec myapp qcli waitforblockheight 7210

{
  "hash": "b5bd2dfa428effb185a31513315bbae6284ab59b9942759e07d9110770ae9c61",
  "height": 7210
}

On mainnet with qtumd:

qtum-cli waitforblockheight 281985

{
  "hash": "44f6a6909b255f9932e28322174c6eec201790d84e7d4f9c201c92594914d240",
  "height": 281985
}


waitforlogs (fromBlock) (toBlock) (filter) (minconf)

requires -logevents to be enabled

Waits for a new logs and return matching log entries. When the call returns, it also specifies the next block number to start waiting for new logs.
By calling waitforlogs repeatedly using the returned `nextBlock` number, a client can receive a stream of up-to-date log entries.

This call is different from the similarly named `waitforlogs`. This call returns individual matching log entries, `searchlogs` returns a transaction receipt if one of the log entries of that transaction matches the filter conditions.

Arguments:
1. fromBlock (int | "latest", optional, default=null) The block number to start looking for logs. ()
2. toBlock   (int | "latest", optional, default=null) The block number to stop looking for logs. If null, will wait indefinitely into the future.
3. filter    ({ addresses?: Hex160String[], topics?: Hex256String[] }, optional default={}) Filter conditions for logs. Addresses and topics are specified as array of hexadecimal strings
4. minconf   (uint, optional, default=6) Minimal number of confirmations before a log is returned

Result:
An object with the following properties:
1. logs (LogEntry[]) Array of matching log entries. This may be empty if `filter` removed all entries.2. count (int) How many log entries are returned.3. nextBlock (int) To wait for new log entries haven't seen before, use this number as `fromBlock`
Usage:
`waitforlogs` waits for new logs, starting from the tip of the chain.
`waitforlogs 600` waits for new logs, but starting from block 600. If there are logs available, this call will return immediately.
`waitforlogs 600 700` waits for new logs, but only up to 700th block
`waitforlogs null null` this is equivalent to `waitforlogs`, using default parameter values
`waitforlogs null null` { "addresses": [ "ff0011..." ], "topics": [ "c0fefe"] }` waits for logs in the future matching the specified conditions

Sample Output:
{
  "entries": [
    {
      "blockHash": "56d5f1f5ec239ef9c822d9ed600fe9aa63727071770ac7c0eabfc903bf7316d4",
      "blockNumber": 3286,
      "transactionHash": "00aa0f041ce333bc3a855b2cba03c41427cda04f0334d7f6cb0acad62f338ddc",
      "transactionIndex": 2,
      "from": "3f6866e2b59121ada1ddfc8edc84a92d9655675f",
      "to": "8e1ee0b38b719abe8fa984c986eabb5bb5071b6b",
      "cumulativeGasUsed": 23709,
      "gasUsed": 23709,
      "contractAddress": "8e1ee0b38b719abe8fa984c986eabb5bb5071b6b",
      "topics": [
        "f0e1159fa6dc12bb31e0098b7a1270c2bd50e760522991c6f0119160028d9916",
        "0000000000000000000000000000000000000000000000000000000000000002"
      ],
      "data": "00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000003"
    }
  ],

  "count": 7,
  "nextblock": 801
}


waitfornewblock (timeout)

Hidden command. Waits for a new block and returns the blockhash and height. Returns the current block on timeout or exit. This command works on command line only (not with the qtum-qt GUI wallet) and is used for development. The timeout parameter is given in milliseconds and 0 indicates no timeout.

On Docker with regtest:

waitfornewblock

{
  "hash": "47f1ddc856ba47a242cbdc771d198296f3a1343f81aad21d454bec438972868c",
  "height": 7207
}

On Mainnet with qtumd:

qtum-cli waitfornewblock

{
  "hash": "b4caac261c4f5386076bbb07e0dc94b92fb67ca740b8a5d7b49cddcaffd6f103",
  "height": 281981
}


walletlock

Locks an encrypted wallet.


walletlock

null

qtum-cli returns no response, but you can check with getwalletinfo for "unlocked_until": 0 and check the padlock icon on qtum-qt.


walletpassphrase "passphrase" timeout ( true )

Unlock an encrypted wallet for transactions which require use of a private key, such as sending coins, staking, or exporting private keys. Timeout gives the time in seconds to unlock and the optional Boolean “true” allows unlocking for staking only.

This command would unlock the wallet for 10 minutes:

walletpassphrase "you should always use a long and strong passphrase" 600

This command would unlock the wallet for staking only for a long time:

walletpassphrase "you should always use a long and strong passphrase" 99999999 true

The qtum-qt wallet will show lock status with the padlock icon and the Console returns “null”. qtum-cli returns no status but you can check the unlock status with getwalletinfo for “unlocked_until”: <UNIX timestamp>

walletpassphrasechange "oldpassphrase" "newpassphrase"

Changes the wallet passphrase from "oldpassphrase" to "newpassphrase"
Qtum-qt returns “null”, qtumd returns nothing XXXXX

walletpassphrasechange "you should always use a long and strong passphrase" "please use a strong and long passphrase"

null



References

X. Bitcoin startup commands https://en.bitcoin.it/wiki/Running_Bitcoin.


https://github.com/qtumproject/qtum/blob/master/doc/sparknet-guide.md

New Qtum RPC Commands
Qtum supports all of the RPC commands supported by Bitcoin Core, but also includes the following commands unique to Qtum:

createcontract - This will create and deploy a new smart contract to the Qtum blockchain. This requires gas.
callcontract - This will interact with an already deployed smart contract on the Qtum blockchain, with all computation taking place off-chain and no persistence to the blockchain. This does not require gas
sendtocontract - This will interact with an already deployed smart contract on the Qtum blockchain. All computation takes place on-chain and any state changes will be persisted to the blockchain. This allows tokens to be sent to a smart contract. This requires gas.
getaccountinfo - This will show some low level information about a contract, including the contract's bytecode, stored data, and balance on the blockchain.
listcontracts - This will output a list of currently deployed contract addresses with their respective balance. This RPC call may change or be removed in the future.
reservebalance - This will reserve a set amount of coins so that they do not participate in staking. If you reserve as many or more coins than are in your wallet, then you will not participate at all in staking and block creation for the network.
getstakinginfo - This will show some info about the current node's staking status, including network difficulty and expected time (in seconds) until staking a new block.
gethexaddress - This will convert a standard Base58 pubkeyhash address to a hex address for use in smart contracts
fromhexaddress - this will convert a hex address used in smart contracts to a standard Base58 pubkeyhash address
New Qtum Command Line Arguments
Qtum supports all of the usual command line arguments that Bitcoin Core supports. In addition it adds the following new command line arguments:

-record-log-opcodes - This will create a new log file in the Qtum data directory (usually ~/.qtum) named vmExecLogs.json, where any EVM LOG opcode is logged along with topics and data that the contract requested be logged.
