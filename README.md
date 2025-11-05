Upgradeable Contracts with Foundry (UUPS Pattern)

This repository shows how to build and upgrade smart contracts using the UUPS upgradeability pattern.
The upgrade flow uses Foundry scripts and an ERC1967 proxy to keep contract storage stable across upgrades.

Project Layout

src/
│── BoxV1.sol // Initial implementation
└── BoxV2.sol // Upgraded implementation

script/
│── DeployBox.s.sol // Deploys proxy + V1
└── UpgradeBox.s.sol // Deploys V2 and upgrades proxy

test/
└── DeployAndUpgradeTest.t.sol // End-to-end deployment and upgrade test

How It Works

BoxV1 is deployed first and wrapped inside an ERC1967Proxy.

The proxy delegates calls to the implementation contract but keeps storage on the proxy.

When upgrading, only the implementation address changes.

BoxV2 adds new functions without modifying the storage layout.

Requirements

Foundry installed

curl -L https://foundry.paradigm.xyz
 | bash
foundryup

Environment variables:

RPC_URL=<your_rpc>
PRIVATE_KEY=<your_private_key>

Deploy (V1)

Run:

forge script script/DeployBox.s.sol
--rpc-url $RPC_URL
--private-key $PRIVATE_KEY
--broadcast

Upgrade to V2

Run:

forge script script/UpgradeBox.s.sol
--rpc-url $RPC_URL
--private-key $PRIVATE_KEY
--broadcast

After the upgrade:

Storage from V1 remains

New logic becomes available (setValue())

Run Tests

forge test -vvvv

This test suite verifies:

Deployment of V1

Upgrade to V2

Storage continuity

License

MIT
