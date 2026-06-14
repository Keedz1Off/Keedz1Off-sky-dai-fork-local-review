# DSS Flow Overview

## Core Idea

Sky / DAI-style DSS accounting is centered around `Vat`.

```text
Vat = internal accounting engine
GemJoin = collateral adapter
DaiJoin = DAI adapter
Spot = oracle price updater
Jug = stability fee accumulator
Dog = liquidation trigger
Clip = liquidation auction
Pot = savings rate module
Vow = surplus / debt accounting
```

Official source files:

```text
src/vat.sol   -> core internal accounting
src/join.sol  -> token adapters
src/jug.sol   -> stability fee rates
src/spot.sol  -> oracle price updates
src/dog.sol   -> liquidation trigger
src/clip.sol  -> liquidation auction
src/pot.sol   -> DSR accounting
src/vow.sol   -> surplus and debt accounting
```

## Collateral Deposit Flow

```text
User external collateral
-> GemJoin.join(...)
-> ERC20 collateral moves into GemJoin
-> Vat.slip(...)
-> internal gem balance increases
```

Important point:

```text
GemJoin connects external collateral tokens to internal Vat accounting.
```

## Open / Modify Vault Flow

```text
User internal gem balance
-> Vat.frob(...)
-> vault collateral ink changes
-> vault debt art changes
-> internal dai balance changes
```

Important point:

```text
Vat.frob(...) is the core vault modification function.
```

## Draw External DAI Flow

```text
Vat internal dai balance
-> DaiJoin.exit(...)
-> Vat.move(...)
-> DaiJoin mints ERC20 DAI
-> user receives external DAI
```

Important point:

```text
DaiJoin.exit(...) converts internal Vat dai into external ERC20 DAI.
```

## Repay Debt Flow

```text
User external DAI
-> DaiJoin.join(...)
-> ERC20 DAI is burned
-> internal Vat dai balance increases
-> Vat.frob(...) repays vault debt
```

Important point:

```text
DaiJoin.join(...) converts external ERC20 DAI back into internal Vat dai.
```

## Withdraw Collateral Flow

```text
Debt is repaid / vault is safe
-> Vat.frob(...) frees collateral
-> internal gem balance increases
-> GemJoin.exit(...)
-> external collateral returns to user
```

## Price / Rate Update Flow

```text
Spot.poke(...)
-> reads oracle price
-> updates Vat spot price

Jug.drip(...)
-> calculates accumulated stability fee
-> calls Vat.fold(...)
-> updates rate / debt accounting
```

## Liquidation Flow

```text
Unsafe vault
-> Dog.bark(...)
-> Vat.grab(...)
-> collateral and debt move into liquidation accounting
-> Clip.kick(...)
-> auction starts
-> Clip.take(...)
-> buyer pays DAI and receives collateral
```

## DSR Flow

```text
User internal Vat dai
-> Pot.join(...)
-> savings pie increases
-> Pot.drip(...)
-> chi accumulator updates
-> Pot.exit(...)
-> user receives internal Vat dai back
```

## Surplus / Debt Flow

```text
Vow.flog(...)
-> releases queued debt

Vow.heal(...)
-> cancels surplus and debt

Vow.flop(...)
-> starts debt auction

Vow.flap(...)
-> starts surplus auction
```
