# Notional contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Resources

- [Twitter](https://twitter.com/NotionalFinance)
- [Website](https://www.notional.finance/)

# On-chain context

```
DEPLOYMENT: mainnet
ADMIN: trusted
```
# Audit scope

PR #35 - Implementing new strategy token valuation model + balancer oracle removal

https://github.com/notional-finance/leveraged-vaults/pull/35/files

Background:
As a result of findings from the first Sherlock Leveraged Vault audit, we decided
to refactor the oracle valuation method to depend on Chainlink oracles instead of
the native Balancer pool TWAP oracle. These changes were too large for the fix review
and therefore are included in this audit.

Specific Focus Areas for PR #35:
  - Adding AccessControlUpgradeable in contracts/vaults/BaseStrategyVault.sol
  - Changes to oracle valuation method:
    - contracts/vaults/balancer/internal/math/Stable2TokenOracleMath.sol
    - contracts/vaults/balancer/internal/pool/TwoTokenPoolUtils.sol

PR #39 - Fixing boosted pool

https://github.com/notional-finance/leveraged-vaults/pull/39/files

Background:
The first Sherlock audit covered the Boosted3TokenAura strategy, however,
Balancer has since migrated the Boosted3Token pool from the legacy BoostedPool
structure to a new ComposableBoostedPool contract. The changes in this PR are
related to adapting to the new ComposableBoostedPool as well as changing the valuation
methodology to rely on Chainlink. The relevant Balancer ComposableBoostedPool contract
can be found here: https://etherscan.io/address/0xa13a9247ea42d743238089903570127dda72fe44#code

Specific Focus Areas for PR #39:
  - Changes to the Boosted3TokenAura Strategy:
    - contracts/vaults/balancer/external/Boosted3TokenAuraHelper.sol
    - contracts/vaults/balancer/internal/pool/Boosted3TokenPoolUtils.sol
    - contracts/vaults/balancer/internal/math/LinearMath.sol
    - contracts/vaults/balancer/internal/math/StableMath.sol
    - contracts/vaults/balancer/mixins/Boosted3TokenPoolMixin.sol

# Running Unit Tests

### Install brownie
```
python3 -m pip install --user pipx
python3 -m pipx ensurepath
pipx install eth-brownie
```
https://eth-brownie.readthedocs.io/en/stable/install.html

### Install hardhat
```
yarn install
```
### Add mainnet-fork
* Add the following YAML block to ~/.brownie/network-config.yaml under development
```
- name: Hardhat (Mainnet Fork)
  id: mainnet-fork
  cmd: "npx.cmd hardhat node"
  host: http://127.0.0.1
  timeout: 120
  cmd_settings:
    port: 8545
    fork: mainnet
```
https://eth-brownie.readthedocs.io/en/stable/network-management.html#
### Execute tests
```
brownie run tests/balancer --network mainnet-fork
```

# About Notional