# Function Review: L2TokenBridge._initiateBridgeERC20(...)

## Function Code

```solidity
function _initiateBridgeERC20(
    address _localToken,
    address _remoteToken,
    address _to,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes memory _extraData
) internal {
    require(isOpen == 1, "L2TokenBridge/closed"); // do not allow initiating new xchain messages if bridge is closed
    require(_localToken != address(0) && l1ToL2Token[_remoteToken] == _localToken, "L2TokenBridge/invalid-token");
    require(_amount <= maxWithdraws[_localToken], "L2TokenBridge/amount-too-large");

    TokenLike(_localToken).burn(msg.sender, _amount);

    messenger.sendMessage({
        _target: address(otherBridge),
        _message: abi.encodeCall(this.finalizeBridgeERC20, (
            // Because this call will be executed on the remote chain, we reverse the order of
            // the remote and local token addresses relative to their order in the
            // finalizeBridgeERC20 function.
            _remoteToken,
            _localToken,
            msg.sender,
            _to,
            _amount,
            _extraData
        )),
        _minGasLimit: _minGasLimit
    });

    emit ERC20BridgeInitiated(_localToken, _remoteToken, msg.sender, _to, _amount, _extraData);
}
```

Note:

```text
Original source: makerdao/op-token-bridge/src/L2TokenBridge.sol
```

## What This Function Does

This is the main L2 withdrawal function.

It does four important things:

```text
checks bridge/token state
checks withdrawal limit
burns L2 tokens
sends a release message to L1
```

## Important Logic Notes

### Bridge Open Check

```solidity
require(isOpen == 1, "L2TokenBridge/closed");
```

If the bridge is closed, new withdrawals cannot start.

### Token Mapping Check

```solidity
require(_localToken != address(0) && l1ToL2Token[_remoteToken] == _localToken, "L2TokenBridge/invalid-token");
```

This checks that the L1 token maps to the expected L2 token.

### Withdraw Limit

```solidity
require(_amount <= maxWithdraws[_localToken], "L2TokenBridge/amount-too-large");
```

This limits how much of a token can be withdrawn in one transaction.

### Burn Tokens On L2

```solidity
TokenLike(_localToken).burn(msg.sender, _amount);
```

This burns the user's L2 tokens.

Simple meaning:

```text
tokens are removed on L2 before L1 release
```

### Create L1 Finalize Message

```solidity
_message: abi.encodeCall(this.finalizeBridgeERC20, (
    _remoteToken,
    _localToken,
    msg.sender,
    _to,
    _amount,
    _extraData
))
```

This creates calldata for the L1 bridge.

The L1 bridge will later release tokens from escrow.

## Main Invariants

```text
1. L2 burned amount must equal L1 released amount.
2. The L2 token must map to the correct L1 token.
3. The withdrawal message must be created only after the burn step.
```

## Additional Invariants / Checks

```text
The bridge must be open before sending a new message.
The withdrawal amount must not exceed maxWithdraw.
The recipient must be the intended recipient.
The encoded calldata must match the real L2 burn.
```
