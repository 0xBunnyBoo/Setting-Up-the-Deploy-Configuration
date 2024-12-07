import { vars, type HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const PK = vars.get("PRIVATE_KEY");

const config: HardhatUserConfig = {
  solidity: "0.8.27",
  defaultNetwork: "minato",
  networks: {
    minato: {
      url: "https://rpc.minato.soneium.org",
      accounts: [PK],
    },
  },
  etherscan: {
    apiKey: {
      minato: "NO API KEY",
    },
    customChains: [
      {
        network: "minato",
        chainId: 1946,
        urls: {
          apiURL: "https://soneium-minato.blockscout.com/api",
          browserURL: "https://soneium-minato.blockscout.com",
        },
      },
    ],
  },
};

export default config;
npx hardhat ignition deploy ./ignition/modules/Lock.ts
> ✔ Confirm deploy to network minato (1946)? … yes
Compiled 1 Solidity file successfully (evm target: paris).
Hardhat Ignition 🚀

Deploying [ LockModule ]

Batch #1
  Executed LockModule#Lock

[ LockModule ] successfully deployed 🚀

Deployed Addresses

LockModule#Lock - 0x25b046FB5Af9d85264f89BcdD8229bfda01bAF89
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

const JAN_1ST_2030 = 1893456000;
const ONE_GWEI: bigint = 1_000_000_000n;

const LockModule = buildModule("LockModule", (m) => {
  const unlockTime = m.getParameter("unlockTime", JAN_1ST_2030);
  const lockedAmount = m.getParameter("lockedAmount", ONE_GWEI);

  const lock = m.contract("Lock", [unlockTime], {
    value: lockedAmount,
  });

  return { lock };
});

export default LockModule;
