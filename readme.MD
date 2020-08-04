# smWallet - an implementation of spacemesh wallet

smWallet implements the Spacemesh encrypted wallet used by the Spacemesh App.

## NewWallet - Creating a new wallet

`NewWallet` creates an unlocked encrypted wallet with mnemonic phrase and a single keypair

``` go
myWallet, err := NewWallet("My main wallet","notAGoodPassword")
```

## LoadWallet - Open an existing wallet

`LoadWallet` loads a wallet but does **not** unlock it.

``` go
myWallet, err := LoadWallet("keystores/dz.json")
```

## Unlock - access encrypted parts of the wallet

To access the accounts or mnemonic phrase, you need to unlock the wallet.

example : load and unlock the wallet

``` go
myWallet, err := LoadWallet("keystores/dz.json")
if err != nil {
    log.Fatal(err)
}
err := myWallet.Unlock("notAGoodPassword")
....
```

## SaveWalletAs - save this wallet to a specific location

This is used to save a new wallet (which does not have a load path) or to save the current state of any open wallet.

``` go
err = myWallet.SaveAs("keystores/backup_copy.json")
```

## SaveWallet - save open wallet (does not work for new wallets that have never been saved)

Only works with wallets that have been loaded or previously saved with `SaveWalletAs`

``` go
err = myWallet.SaveWallet()
```

## SignedTransaction - convert a transaction to signed transaction ready for submission

Signs a transaction using current keypair, returns `[]byte` ready to submit

``` go
    var tx types.Transaction
    tx.Recipient = types.BytesToAddress(util.Hex2Byte("865330189761187daa2243a1533b0412b8e14613"))
    tx.GasLimit = 100
    tx.Fee = 1
    tx.Amount = 1000000000000
    tx.AccountNonce = 3      // get from account info
    txs, err := smData.SignedTransaction(&tx)
    ...
```

## GenerateNewPair - add a new keypair from mnemonic phrase

Requires an unlocked wallet.

``` go
keyNum,err := myWallet.GenerateNewPair("name of new keypair")
```

## GetNumberOfAccounts() - find out how many keypairs are stored in wallet

``` go
num,err := myWallet.GetNumberOfAccounts()
```

## GetAddress - get the address associated with a specific keypair

``` go
addr,err := myWallet.GetAddress(3)
...
fmt.Println(addr.Hex())
```

## GetPublicKey - get the public key associated with a specific keypair

``` go
pub,err := myWallet.GetPublicKey(3)
```

## GetDisplayName - get the display name associated with a specific keypair

``` go
name,err := myWallet.GetAddress(3)
...
fmt.Println(name)
```

## SetCurrent - select the wallet to be used for signing transactions

select an existing keypair to be used for signing

``` go
err := myWallet.SetCurrent(3)
```
