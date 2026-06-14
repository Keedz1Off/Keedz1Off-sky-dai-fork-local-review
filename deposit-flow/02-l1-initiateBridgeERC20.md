# Function Review: L1TokenBridge._initiateBridgeERC20(...)

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
    require(isOpen == 1, "L1TokenBridge/closed"); // do not allow initiating new xchain messages if bridge is closed
    require(_remoteToken != address(0) && l1ToL2Token[_localToken] == _remoteToken, "L1TokenBridge/invalid-token");

    TokenLike(_localToken).transferFrom(msg.sender, escrow, _amount);

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
Original source: makerdao/op-token-bridge/src/L1TokenBridge.sol
```

## What This Function Does

This is the main L1 deposit function.

It does three important things:

```text
checks bridge/token state
locks tokens in L1 escrow
sends a message to L2
```

## Important Logic Notes

### Bridge Open Check

```solidity
require(isOpen == 1, "L1TokenBridge/closed");
```

If the bridge is closed, new deposits cannot start.

### Token Mapping Check

```solidity
require(_remoteToken != address(0) && l1ToL2Token[_localToken] == _remoteToken, "L1TokenBridge/invalid-token");
```

This checks that the L1 token maps to the expected L2 token.

Simple meaning:

```text
This token pair must be registered before bridging.
```

### Lock Tokens In Escrow

```solidity
TokenLike(_localToken).transferFrom(msg.sender, escrow, _amount);
```

This transfers L1 tokens from the user into the escrow contract.

Simple meaning:

```text
user -> escrow
```

### Create L2 Finalize Message

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

This creates calldata for the L2 bridge.

The L2 bridge will later execute `finalizeBridgeERC20(...)` and mint tokens.

### Send Message

```solidity
messenger.sendMessage({
    _target: address(otherBridge),
    _message: ...,
    _minGasLimit: _minGasLimit
});
```

This sends the deposit message to the L2 bridge through the OP messenger.

