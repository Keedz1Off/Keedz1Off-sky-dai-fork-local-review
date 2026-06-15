# Function Review: L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)

## Function Code

```solidity
function bridgeERC20(
    address _localToken,
    address _remoteToken,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes calldata _extraData
) external {
    require(msg.sender.code.length == 0, "L1TokenBridge/sender-not-eoa");
    _initiateBridgeERC20(_localToken, _remoteToken, msg.sender, _amount, _minGasLimit, _extraData);
}

function bridgeERC20To(
    address _localToken,
    address _remoteToken,
    address _to,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes calldata _extraData
) external {
    _initiateBridgeERC20(_localToken, _remoteToken, _to, _amount, _minGasLimit, _extraData);
}
```
bridgeERC20()


 bridgeERC20To()

Note:

```text
Original source: makerdao/op-token-bridge/src/L1TokenBridge.sol
```

## What This Function Does

These are the user entry points for L1 -> L2 deposits.

`bridgeERC20(...)` sends tokens to the same address on L2.

`bridgeERC20To(...)` sends tokens to a chosen recipient on L2.

Simple meaning:

```text
User starts a deposit from L1 to L2.
```

## Important Logic Notes

### EOA Check

```solidity
require(msg.sender.code.length == 0, "L1TokenBridge/sender-not-eoa");
```

This makes `bridgeERC20(...)` callable only by EOAs.

`bridgeERC20To(...)` does not use this EOA check because it supports deposits to another recipient.

### Internal Flow Call

```solidity
_initiateBridgeERC20(_localToken, _remoteToken, msg.sender, _amount, _minGasLimit, _extraData);
```

This moves execution into the core deposit logic.

The real lock and message creation happen inside `_initiateBridgeERC20(...)`.

