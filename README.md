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

### addmultisigaddress nrequired ["key",...] ( "account" "address_type" )

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





