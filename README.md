# CryptoHawking, Public Security Reviews 🛡️

Independent smart contract security reviews by **Mudaser Iqbal (Crypto Hawking)**, ETHDenver 2025 winner, auditor at [cryptohawking.com](https://www.cryptohawking.com).

> **Scope & honesty:** every review here is an **independent analysis of publicly verified contract source**, not commissioned by the projects unless explicitly stated. Reports are produced with my audit platform (AI analysis, Slither static analysis, manual review) against SWC Registry, OWASP Smart Contract Top 10, and EEA EthTrust checklists. A review is a risk snapshot, not a guarantee. "Risk" reflects worst-case residual risk (often centralization/key-compromise scenarios), not a claim of an active exploit.

## Reports

| # | Protocol | Category | Chain | Date | Risk | Findings | Report |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Uniswap v4 PoolManager | DEX | Ethereum | 2026-10-07 | Low | 5 | [PDF](reports/2026-10-07-uniswap-v4-poolmanager.pdf) |
| 2 | Uniswap V2 Router02 | DEX | Ethereum | 2026-10-07 | Low | 4 | [PDF](reports/2026-10-07-uniswap-v2-router02.pdf) |
| 3 | PancakeSwap v3 Factory | DEX | BNB Chain | 2026-10-07 | Medium | 6 | [PDF](reports/2026-10-07-pancakeswap-v3-factory.pdf) |
| 4 | Morpho Blue | Lending | Ethereum | 2026-10-07 | Low | 6 | [PDF](reports/2026-10-07-morpho-blue.pdf) |
| 5 | Lido wstETH | Liquid staking | Ethereum | 2026-10-07 | Medium | 2 | [PDF](reports/2026-10-07-lido-wsteth.pdf) |
| 6 | Rocket Pool rETH | Staking | Ethereum | 2026-10-07 | Medium | 4 | [PDF](reports/2026-10-07-rocketpool-reth.pdf) |
| 7 | Spark sDAI | Yield | Ethereum | 2026-10-07 | Medium | 4 | [PDF](reports/2026-10-07-spark-sdai.pdf) |
| 8 | Chainlink ETH/USD Aggregator Proxy | Oracle | Ethereum | 2026-10-07 | High | 5 | [PDF](reports/2026-10-07-chainlink-eth-usd.pdf) |
| 9 | OpenSea Seaport 1.6 | NFT marketplace | Ethereum | 2026-10-07 | Low | 6 | [PDF](reports/2026-10-07-seaport-1-6.pdf) |

Note on #8: the High rating reflects the well-known owner-controlled aggregator rotation in Chainlink's proxy pattern (a centralization/key-compromise risk documented in the report), not a newly discovered exploit.

## Responsible disclosure

If a review uncovers an unpatched vulnerability that endangers live user funds, the report is **withheld** and the finding is disclosed privately to the project team first. Reports appear here only when publication creates no exploit risk.

## Want your contracts reviewed?

- **Free AI audit** (90-second PDF): [cryptohawking.com/audit](https://www.cryptohawking.com/audit)
- **Manual audit** ($5,000, 3 business days) · **Full dApp audit** ($12,000): [cryptohawking.com/services](https://www.cryptohawking.com/services)
- WhatsApp: [+92 322 4274236](https://wa.me/923224274236)
