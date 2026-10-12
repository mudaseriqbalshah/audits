# The Contract Layer Trust Checklist

Seven questions you can answer yes or no by reading the source.

Cryptographic verification tells you that an authority *was* exercised. It says nothing about whether that authority should have existed in the first place. A system can prove every action perfectly and still leave the weaker party with no way to refuse. Verifiable is not the same as bounded, and the gap between them is where the expensive failures live.

This is the contract layer version of that question. Every check below is answerable from code, maps to a specific thing a reviewer looks for, and has cost somebody real money when the answer went the wrong way. Examples marked with a link come from reviews published in this repository.

---

## 1. Can the operator change the rules after users have committed?

**Look for:** setters on fees, oracles, implementations, strategies, routers or reward rates that take effect immediately.

A user who deposits under one set of rules has consented to those rules, not to whatever replaces them a block later. Immediate-effect setters convert a user's past decision into a blank cheque.

**Example:** in Chainlink's [EACAggregatorProxy review](reports/2026-10-07-chainlink-eth-usd.pdf), a single owner can redirect the price feed that every downstream protocol depends on, with no timelock, no event and no validation of the new aggregator. Nothing is broken. The authority is simply unbounded.

**Bounded version:** a timelock long enough for users to exit, an event on every change, and where possible a hard limit on how far a parameter can move in one step.

## 2. Is there an exit the operator cannot block?

**Look for:** pause functions that cover withdrawals, allowlists on redemption, admin-gated exit paths, or withdrawal queues an operator can stall.

A pause that stops deposits is prudence. A pause that stops withdrawals is custody. The difference is rarely stated in the documentation and is always visible in the code.

**Bounded version:** emergency controls that exclude the exit path by construction, or that expire automatically after a fixed window so inaction cannot become a permanent freeze.

## 3. Is privileged authority bounded in scope, or merely logged?

**Look for:** arbitrary external calls (`target.call(data)` with both arguments controlled), unrestricted upgrades, unlimited mint, sweep functions that can touch user assets rather than only protocol assets.

An event log tells you what happened after it happened. It is a record, not a constraint. The question is what the function *can* do, not what it has done so far.

**Example:** in the [FermiSwap review](reports/2026-10-11-fermiswap.pdf), the manager can call any contract with any calldata. Users must approve the router before they can swap at all, so that one function quietly turns every outstanding allowance into something a single key can move. The swap logic itself is clean.

**Bounded version:** an allowlist of targets, an explicit denylist of the `transferFrom` and `approve` selectors, a timelock, and narrow purpose-built functions instead of a general escape hatch.

## 4. Can that authority be revoked or rotated?

**Look for:** immutable owners, a single externally owned account, one-step ownership transfer, no pause of any kind.

This is the question most reviews skip, because an immutable owner looks like a commitment rather than a risk. It is both. There is no takeover risk and no recovery either, so a leaked key is permanent.

**Example:** FermiSwap's manager is `immutable`. Combined with check 3, the most powerful key in that system is also the only one that can never be replaced. The only remaining defence is every user revoking their approvals individually, which in practice reaches a small fraction of holders.

**Bounded version:** two-step transfer (propose, then accept) to a multisig, so a key can be rotated before it is compromised and a mistyped address cannot brick the system.

## 5. Does identity come from the call, or from calldata?

**Look for:** an address decoded from user-supplied bytes and then used for limits, cooldowns, allowances, rewards or permissions.

If the caller supplies the identity that a control is keyed on, the control is advisory. This is subtle because the code reads correctly, the mapping lookup is right, and the revert condition is right. Only the input is wrong.

**Example:** in the [SATO hook review](reports/2026-10-12-sato.pdf), the trader's identity is decoded from `hookData`. The anti-sandwich cooldown keys on it, so passing a fresh address on the sell skips the check entirely. One extra word of calldata defeats the only trading control in the contract.

**Bounded version:** derive identity from `msg.sender` or from the trusted caller parameter the framework already provides. Treat calldata as a hint, never as an authorisation.

## 6. Does the accounting have a single source of truth?

**Look for:** two counters that are supposed to describe the same state, silent clamps (`if (a > b) a = 0`) where a revert belongs, balances tracked separately from the contract's actual holdings.

Divergence between two numbers that must agree is not a rounding nuisance. It is a standing arbitrage against the protocol that nobody is monitoring, because the contract never says it happened.

**Example:** SATO maintains cumulative ETH and fair supply as a linked pair, but the sell path clamps one to zero while reducing the other normally. After that, buys and sells price from different points on the same curve.

**Bounded version:** derive one value from the other so they cannot drift, or revert when the invariant breaks. A failed transaction is cheaper than a silent inconsistency.

## 7. What is unreviewed because it shipped after the audit?

**Look for:** functions whose style, naming or compiler pragma differs from the rest of the file, features announced after the audit date, proxy implementations deployed later than the reviewed version.

An audit is a photograph of a codebase on one day. Everything added afterwards is unaudited code sitting behind an audited reputation.

**Example:** Euler Finance was audited multiple times and still lost $197M. The vulnerable function, `donateToReserves`, was added after those audits and skipped the solvency check every sibling function performed.

**Bounded version:** treat the audit as scoped to a commit hash, publish that hash, and make a re-review a gate in the deployment process rather than a decision taken later.

---

## How to read the answers

A system passes a check when the answer is a bounded yes. It does not pass on "there is an event for that", on "the team would never do that", or on "the key is in a hardware wallet". Those are statements about intent and operational practice. This checklist is about what the code permits.

Most systems in production today pass checks 3 and 7 on paper and fail them in substance, because the authority is documented rather than constrained. That is the distinction worth holding on to: a well designed system does not ask to be trusted, it removes the need.

---

*Written by [Mudaser Iqbal (Crypto Hawking)](https://www.cryptohawking.com), ETHDenver 2025 winner. The examples come from independent reviews published in this repository. Free contract scan: [cryptohawking.com/audit](https://www.cryptohawking.com/audit).*
