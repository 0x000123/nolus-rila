## Create Wallet
```
nolusd keys add $WALLET_NAME
```
or recover old wallet:
```
nolusd keys add $WALLET_NAME --recover
```

Get your public address from the keystore
```
nolusd keys show $WALLET_NAME -a
```

## Create validator
```python
nolusd tx staking create-validator \
  --amount 1000000unls \
  --from $wallet \
  --commission-max-change-rate "0.1" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey  $(nolusd tendermint show-validator) \
  --moniker 0x000123 \
  --chain-id nolus-rila \
  --identity="" \
  --details="" \
  --website="" -y
```

## Confirm That Your Validator Is Active

If running the following command returns something, your validator is active:
```bash
nolusd query tendermint-validator-set | grep "$(nolusd tendermint show-address)”
```

# 🔓Unjail a Validator

If your validator fails to sign at least 5% of the last 10000 blocks, it will get jailed. To unjail it so that it can continue to sign blocks and respectively earn staking rewards, you can submit the following transaction:
```python
nolusd tx slashing unjail --from <yourKey> --chain-id nolus-rila --fees 500unls
```
