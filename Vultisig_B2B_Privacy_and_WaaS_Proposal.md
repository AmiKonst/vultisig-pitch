# Strategic Architecture & Technical Specification: Institutional Privacy & Wallet-as-a-Service (WaaS) for Vultisig

**Authors:** TK
**Target Audience:** Core Protocol Engineers, Cryptographers, Leadership, and Ecosystem Builders  
**Status:** Comprehensive Technical Proposal & Phased Implementation Blueprint  
**Date:** November 2026

---

## 1. Executive Summary

Public blockchains suffer from an existential dilemma: **radical transparency empowers retail trust, but mathematically disqualifies enterprise adoption**. A corporation cannot legally or commercially expose employee payrolls, vendor payment terms, or strategic treasury allocations on a public EOA or multisig. Yet, legacy privacy protocols (such as Tornado Cash) contaminate the anonymity pool with illicit funds, resulting in OFAC sanctions, CEX asset freezes, and developer prosecution.

This document outlines a **production-ready, legally immune, and cryptographically verified architecture** designed specifically for Vultisig’s Threshold Signature Scheme (TSS / DKLS) stack:

1. **Stage-Gated Commercial Wedge — B2B SaaS + Turnkey Neobank WaaS + Clean Swaps (Phase 1, Zero ZK Wait):** Launches immediately on Vultisig’s **existing production DKLS / FastVault stack** across BTC, SOL, and EVM—requiring **no new key types, no 4-platform ZK provers, and no upfront ZK circuit audits**. Monetizes from Day 1 via B2B/WaaS SaaS licenses, MAU pricing, ERP audit exports, and clean swap/bridge routing (`0.25%–0.40%`), building active users, swap cashflow, and resting L2 liquidity before committing heavy ZK R&D capital.
2. **Customer-Gated B2B Privacy Layer (Phase 2):** Unlocked strictly after Phase 1 validates commercial demand (`2–3` paying pilots / signed enterprise LOIs). Deploys audited ZK-Shielded Pools on Arbitrum and Base featuring a deterministic **"ReadOnly Share" (`sk_view`)**, **2-of-3 EdDSA Quorum Spend Circuit** (`pk_nullifier` bound inside `quorum_hash`), and **Zero-Knowledge Dual-Recipient Encryption** with hidden gateway keys (`G_root`), layered onto existing WaaS retail flow and resting USDC TVL.
3. **Absolute Regulatory & Fee-Model Immunity (Zero `%` Withdrawal Toll):** Under FinCEN Guidance FIN-2019-G001 and EU MiCA Recital 22, Vultisig remains 100% open-source, non-custodial software while KYB, Travel Rule reporting, and Clean Association Set (`ASP_root`) curation sit with accredited gateways and independent ASPs. Crucially—to eliminate the *Samourai Wallet* coordinator-fee vector—**Vultisig charges `0%` on private deposits, transfers, or withdrawals**, monetizing strictly through B2B SaaS software licenses and standard swap routing.
4. **Two-Stage $VULT Economics (Buybacks First → Permissionless Infrastructure Nodes at Scale):** In Stage 1, programmatic `$VULT` buybacks (`25%` of net SaaS + Swap USDC revenue) and optional swap/SaaS fee-discount locks drive token value with **zero** upfront node overhead. Once transaction volume reaches critical mass (Phase 3), anyone can bond **100,000 $VULT** to operate a **Decentralized FastVault Co-signer / ERC-4337 Paymaster & Stateless Shuttle Batcher**, earning pure USDC from **flat per-slot/gas spreads and Co-signer SLA fees** (never a `%` toll on shielded withdrawals).
5. **Lean Stage-Gated Capital Plan (`$125k–$165k` Upfront | `$565k–$810k` Full 14-Month Stack):** Phase 1 ships the commercial WaaS/B2B SaaS & Swap stack in Months 1–4 for **`$125k–$165k`**. Only upon passing Stage-Gate #1 do WaaS cashflow and L2 grants co-fund Phase 2 ZK build + audits (Months 5–10, `$325k–$480k`) and Phase 3 permissionless `$VULT` infrastructure nodes (Months 11–14, `$115k–$165k`).
6. **Future R&D Horizon (Post-Mainnet, Out of Phase 1–3 Budget Scope):** Extends Vultisig’s MPC foundation beyond payments into **Anti-Deepfake Voice/Video Identity Verification** and **Threshold-Encrypted Enterprise Data Vaults**.

---

## 2. Market Demand & Total Addressable Market (TAM)

### 2.1 The Underserved B2B Crypto Economy
While retail trading volume is cyclical and volatile, institutional and enterprise B2B payments represent sticky, high-volume recurring flow:
* **Stablecoin Commercial Volume:** Over **$30 Billion** in active circulating supply on EVM Layer 2s (Base, Arbitrum), processing more than **$1.5 Billion** in daily commercial settlements.
* **B2B Web3 Benchmarks:** Platforms like Request Finance have processed over **$500 Million** in crypto invoices and payrolls despite zero on-chain privacy. Over **68% of surveyed Web3 CFOs** cite public on-chain transparency as their #1 operational barrier to migrating traditional vendor payments on-chain.
* **Institutional Multi-Sig Limitations:** Safe (Gnosis) secures over **$100 Billion** in assets, yet every transaction is publicly labeled as a smart-contract contract call. Competitors and MEV bots monitor these addresses in real time.

### 2.2 The Commercial Leaks in Existing Multisigs
| Vulnerability | Business Consequence | Real-World Impact |
| :--- | :--- | :--- |
| **Public Payrolls** | Salaries are traceable per employee address. | Talent poaching, internal salary friction, employee doxxing. |
| **Supplier Invoicing** | Discounted rates and terms are transparent. | Loss of pricing leverage with other global suppliers. |
| **Treasury Movements** | Rebalancing or OTC sales are publicly telegraphed. | Front-running by MEV bots, market panic on routine transfers. |

### 2.3 The Untapped Emerging Market Neobank Demand
In Latin America (Argentina, Brazil), Sub-Saharan Africa (Nigeria, Kenya), and Southeast Asia, local fiat currencies suffer from chronic inflation, and cross-border bank wires incur 5–9% fees with 3–5 day delays. Millions of businesses and consumers are adopting digital dollars ($USDC). However, legacy self-custody wallets (with 12-word seed phrases) suffer an **80%+ drop-off rate** during onboarding. Vultisig’s FastVault provides the missing link: a consumer-grade banking UX backed by non-custodial MPC security.

### 2.4 Launch Target Verticals & Anonymity-Set Sizing Benchmarks
To eliminate cold-start guesswork before production engineering, commercial validation targets **three concrete B2B verticals** experiencing immediate on-chain privacy leaks today:
1. **Web3 Payroll & Invoicing Platforms (e.g., Request Finance, Rise Works, Franklin class & distributed Web3 studios):** Teams paying 20–200 remote contributors in USDC want to conceal individual `$1,000–$5,000` salaries and contractor rates from Arkham/Etherscan while exporting ReadOnly logs for accounting.
2. **Regional Crypto PSPs, Acquirers & OTC Desks (e.g., CoinsPaid, Triple-A, B2BinPay, Bitso Business, Yellow Card class):** Cross-border commercial settlement desks leak supplier lists, procurement cadence, and pricing margins when transacting from public EOA/Safe addresses, yet require strict compliance for banking off-ramps.
3. **Stablecoin Neobanks, Crypto Card Programs & Fund Treasuries (e.g., THORWallet/Fiat24, Kast, Gnosis Pay class & ecosystem funds):** Card/neobank programs need seedless 2-of-3 MPC security and feed continuous, chaotic retail card top-ups and transfers into the shared pool, while fund treasuries eliminate MEV/copy-trading front-running.

#### Minimum Anonymity-Set Sizing, Resting TVL & Payday Decorrelation
* **B2B-Only Pool (No Retail):** Reaching `k ≥ 20–30` independent transactions per window requires **40–60 active B2B companies** (1–2h windows) or **10–15 active companies** using 12–24h clearing windows. With only 1–3 early clients on an empty L2 pool, a B2B-only pool leaks both timing and pool-TVL spikes.
* **Hybrid Clean Flow + Resting L2 Liquidity (Phase 1 Seeded):** Launching WaaS/B2B SaaS first means that by the time the ZK-Pool opens in Phase 2, **500–1,000+ daily verified retail/card transactions** and **multi-million resting WaaS/treasury USDC balances** already sit on Base/Arbitrum—producing `k ≥ 40–80` per 2-hour batch window while smoothing large corporate deposit spikes. Subsidizing 1,000 batched retail txs on Base costs **~$5–$10/day in L2 gas**, effortlessly covered by B2B SaaS subscriptions (`$500–$1,500/mo`) and flat batch relay fees (`~$1.00/slot`) with zero `%` tax on shielded withdrawals.
* **Payday Temporal Decorrelation:** To defeat fixed-date (1st/15th) payroll correlation, WaaS-integrated payroll allows contributors to **hold and spend balances natively inside the shielded/card wallet** (e.g., Visa/Mastercard top-ups, P2P transfers, or swaps) rather than dumping 100% into public CEXs on payday, while any public EOA exits enforce randomized cross-window jitter and quantized denominations (`$100–$5,000`).

---

## 3. Regulatory & Legal Framework: Why Vultisig Requires Zero Licenses

A central fear for Web3 engineering teams is the fate of Tornado Cash and Samourai Wallet. This architecture is structurally immune to those enforcement vectors by design.

```
┌─────────────────────────────────────────────────────────────┐
│          REGIONAL ACCREDITED GATEWAY (US / EU / UAE)        │
│  - Licensed VASP / CASP / FinCEN MSB                        │
│  - Executes KYB on Enterprise & Sanctions Screening         │
│  - Issues On-Chain ZK-Attestation (EAS / Soulbound Token)   │
└──────────────────────────────┬──────────────────────────────┘
                               │
                [ Verifiable On-Chain Pass ]
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│            VULTISIG PERMISSIONED SHIELDED POOL (L2)         │
│  - Autonomous, Non-Custodial Smart Contract                 │
│  - Accepts deposits ONLY from attested addresses            │
│  - Immutable, no admin keys, no pause button                │
└──────────────────────────────┬──────────────────────────────┘
                               ▲
                [ Off-Chain P2P Signing ]
                               │
┌──────────────────────────────┴──────────────────────────────┐
│                    VULTISIG CLIENT APP                      │
│  - 100% Client-Side Open-Source Code (MIT/Apache 2.0)       │
│  - P2P Threshold Signing (DKLS WASM)                        │
│  - Zero Custody, Zero Centralized Server Possession         │
└─────────────────────────────────────────────────────────────┘
```

### 3.1 Unhosted Wallet Exemption (FinCEN & MiCA)
* **FinCEN Guidance (FIN-2019-G001, Section 5.5.1):** Software developers who create and distribute unhosted wallets are explicitly **not** Money Transmitters under US Federal Law, provided they do not possess unilateral control or independent custody of user funds.
* **EU MiCA (Markets in Crypto-Assets, Recital 22):** Software developers providing decentralized, non-custodial crypto-asset interfaces and hardware/software tools are exempt from CASP licensing requirements.
* **Vultisig's Defense:** In Vultisig's 2-of-3 setup, the user and their designated co-signers control all key shares. Vultisig Labs holds zero shares, zero customer funds, and has no power to execute, halt, or reverse a transaction.

### 3.2 The Accredited Gateway (Music Distributor Model)
Instead of Vultisig attempting to secure SOC 2, MSB, or banking licenses in 150 jurisdictions, the protocol employs the **Distributor Model** (analogous to how Spotify relies on DistroKid or TuneCore to clear legal rights):
1. Independent, licensed compliance brokers (e.g., licensed VASPs in the EU, regulated trust entities in the UAE, FinCEN MSBs in the US) establish front-end business gateways.
2. The gateway performs standard corporate Know-Your-Business (KYB) and checks corporate UBOs (Ultimate Beneficial Owners).
3. The gateway issues a non-transferable, cryptographic on-chain credential (via Ethereum Attestation Service or Zero-Knowledge Pass).
4. **Result:** The smart contract pool strictly permits deposits from attested addresses. Illicit actors (Lazarus, darknet markets) are mathematically barred at the contract entry point. Vultisig never touches user KYC data.

### 3.3 Compliance with FATF Travel Rule & Trade Secret Protection
* **US Defend Trade Secrets Act (DTSA) & EU Directive 2016/943:** Corporations have a legal mandate to protect proprietary financial data, payrolls, and trade secrets. Public block explorers force businesses to violate this mandate.
* **FATF Recommendation 16 (Travel Rule):** By embedding Dual-Recipient Encryption into the shielded transaction, the originator and beneficiary information is cryptographically encrypted directly for the accredited compliance partner. Travel Rule requirements are fulfilled on-chain without exposing data to the public internet.

### 3.4 Zero-Friction Verification for Verified Retail "Cover Traffic" (The Fiat24 Pattern)
To prevent an early B2B pool from becoming transparent without admitting unscreened illicit actors—and without turning Vultisig into a KYC bureau—the pool adopts the exact partner-delegated credential pattern proven in production by **THORWallet** (via **Fiat24** Account NFTs on Arbitrum and **Unblock** for Visa/Mastercard programs):
1. **Reusable On-Chain KYC Attestations (Zero Clicks, Zero PII):** Millions of wallets on Base and Arbitrum already hold verifiable, non-transferable on-chain credentials issued by regulated entities (e.g., **Coinbase Verifications EAS**, **Fiat24 Account NFTs**, **Binance BAB**, or **Sumsub Reusable IDs**). Any wallet holding a whitelisted attestation can route transfers through the pool without uploading documents to Vultisig.
2. **Independent ASP Exclusion Sets & Stateless Relayer Screening:** Independent, regulated **Association Set Providers (ASPs)**—separate from Vultisig Labs and separate from permissionless gas relayers—maintain the on-chain *Proof of Innocence* Merkle roots using sanctions/AML feeds (OFAC, Chainalysis Sanctions Oracle). Permissionless `$VULT` relayer nodes only perform stateless pre-submission checks against those public ASP roots and refuse tainted deposits, keeping nodes free of discretionary compliance/MSB liability.
3. **Gasless Privacy Incentive & Unified Ciphertext Format:** Clean retail users receive **sponsored gasless private transfers** (`$0` ETH required for gas) subsidized from B2B SaaS infrastructure budgets and flat batch-slot economics. Crucially, retail cover transactions emit the **exact same circuit shape and hidden compliance ciphertext structure** (encrypted to a public/ASP epoch key in the shared Gateway Registry) so chain analytics cannot filter out retail cover traffic.

---

## 4. Deep Technical Architecture: MPC + ZK Protocol Specification

This section outlines the exact cryptographic mechanics, data structures, and state transitions for engineering implementation.

```
                    [ 2-of-3 Device Quorum ]
         (Each Device Holds Independent BabyJubJub EdDSA Share)
                             │
        ┌────────────────────┴────────────────────┐
        ▼                                         ▼
[ Viewing & Nullifier Derivation ]     [ Quorum Spend Signing ]
  Separate view_seed (.vult) or          Device 1 + Device 2 each sign
  Threshold DH (OPRF) -> sk_view         H(nullifier, out_wd_hash, relayer_fee)
  + pk_nullifier = Poseidon1(sk_null)    (Private Keys NEVER leave devices)
        │                                         │
        ▼                                         │
 Export sk_view to ReadOnly Client                │
 (0/3 Spend Power — Auditors / PwC)               ▼
                                      Construct Shielded Note:
                                      quorum_hash = Poseidon4(pk_ed_1..3, pk_null)
                                      Note = { asset, amount, quorum_hash,
                                               pk_view_rec, rho, r }
                                                  │
        ┌─────────────────────────────────────────┴──────────────────────┐
        ▼                                                                ▼
 Encrypt for Recipient (ECDH+Poseidon)                Encrypt for Gateway (ECDH+Poseidon)
 C_rec = Enc(pk_view_rec, Note)                       C_comp = Enc(pk_gateway_epoch, Note)
        │                                                                │
        └─────────────────────────────────────────┬──────────────────────┘
                                                  ▼
                    Generate Groth16 / UltraHonk ZK-Proof (Native rapidsnark / Noir):
                    1. 2-of-3 BabyJubJub EdDSA Quorum + pk_nullifier Binding in quorum_hash
                    2. Proof of Clean Association Set (ASP_root)
                    3. Proof of Gateway Registry Membership (G_root — pk_gateway hidden!)
                    4. Dual-Encryption Equivalence (C_rec, C_comp) & Nullifier Derivation
                                                  │
                                                  ▼
                   Submit to L2 Smart Contract Pool (Base / Arbitrum)
                   (Ring buffer: 4,096 Note roots, 256 ASP roots)
```

### 4.1 Quorum-Safe Key Architecture (Preserving 2-of-3 Security + DKLS Compatibility)
In DKLS23 threshold ECDSA, signing ceremonies intentionally use a fresh random ephemeral nonce ($k$) on every run—meaning raw `TSS-Sign` outputs cannot derive static keys. At the same time, deriving a **monolithic `sk_spend`** from a shared `.vult` seed on a single witness-generating device would break Vultisig's 2-of-3 security model (allowing 1 compromised device or leaked `.vult` backup to drain the shielded pool).

To guarantee **both** a static ReadOnly viewing key and unbreakable **2-of-3 threshold spend security**, Vultisig strictly separates **Viewing**, **Nullifier**, and **Quorum Spend** authority:

1. **Static Viewing Key (`sk_view`) — 0-of-3 Spend Power:**
   * At KeyGen, a 32-byte `view_seed` is persisted in the encrypted vault file (`.vult`)—or computed deterministically via Threshold Diffie-Hellman (OPRF) over $M = \text{"VULTISIG\_VIEW\_V1"}$ ($P_{\text{OPRF}} = sk_{\text{master}} \cdot H_{\text{curve}}(M)$, zero ephemeral nonce):
     $$PRK_{view} = \text{HKDF-Extract}(\text{Salt}=\text{"Vultisig-ZK-View-V1"}, \text{view\_seed\_or\_OPRF})$$
     $$sk_{view} = \text{HKDF-Expand}(PRK_{view}, \text{"VIEWING\_KEY"}, 32), \quad pk_{view} = sk_{view} \cdot G_{\text{BabyJubJub}}$$
   * Exporting `sk_view` to an auditor requires zero signing ceremonies and leaks **zero** ability to spend funds or forge quorum approvals.
2. **Per-Device Independent BabyJubJub EdDSA Keys (Zero Monolithic `sk_spend`):**
   * Each of the 3 executive devices generates and retains its own **hardware-isolated BabyJubJub EdDSA keypair** $(sk_{ed,i}, pk_{ed,i})$ inside Secure Enclave / KeyStore. **Device private keys never leave their device and are never combined on any witness generator.**
   * To cryptographically bind the nullifier secret and prevent a malicious signer from grinding arbitrary `sk_nullifier` values to double-spend a note, the vault's private on-chain spend policy commits to **both** the 2-of-3 device public keys and the nullifier public commitment $pk_{nullifier} = \text{Poseidon1}(sk_{nullifier})$:
     $$\text{quorum\_hash} = \text{Poseidon4}(pk_{ed,1}, pk_{ed,2}, pk_{ed,3}, pk_{nullifier})$$
   * To spend a note, any 2 of 3 devices independently sign the spend digest $\mu = \text{Poseidon3}(\text{nullifier}, \text{out\_and\_withdrawal\_hash}, \text{fee\_and\_relayer})$ over P2P WebRTC and return only their signatures $(\sigma_a, \sigma_b)$. The initiating client feeds $(\sigma_a, \sigma_b, pk_{ed,1}, pk_{ed,2}, pk_{ed,3}, pk_{nullifier}, sk_{nullifier})$ as **Private Inputs** into the circuit (`EdDSAPoseidonVerifier` in `circomlib`), proving 2-of-3 authorization **without ever revealing the quorum public keys or `pk_nullifier` on-chain** and without any single device holding 2 keys.
3. **Nullifier Key Separation (`sk_nullifier` Committed via `pk_nullifier`):**
   * `sk_nullifier` is derived separately from `sk_view` (e.g., via a separate Threshold DH / OPRF domain `"VULTISIG_NULLIFIER_V1"` or dedicated vault secret), and only its one-way hash $pk_{nullifier} = \text{Poseidon1}(sk_{nullifier})$ is bound inside `quorum_hash`.
   * Because an external auditor holding `sk_view` only observes `quorum_hash` (not `pk_nullifier` or `sk_nullifier`), they cannot precompute nullifiers to correlate future unspends. Meanwhile, because `pk_nullifier` is hashed inside `note.quorum_hash`, even a compromised quorum cannot double-spend a note by altering `sk_nullifier`.
4. **Device Loss & Shielded Key Resharing:**
   * If Device 1 is lost, Devices 2 and 3 co-sign a shielded self-transfer (`Note_old -> Note_new`) sweeping notes to $\text{quorum\_hash}_{\text{new}} = \text{Poseidon4}(pk_{ed,1}^{\text{new}}, pk_{ed,2}, pk_{ed,3}, pk_{nullifier})$ privately inside the pool.

### 4.2 Shielded Note, Commitment & Ring-Buffer State
All shielded balances inside the pool are represented as UTXO notes over the BN254 (alt_bn128) elliptic curve:

```rust
struct ShieldedNote {
    asset_id: u256,       // ERC-20 contract address (e.g., Base.USDC)
    amount: u64,          // Token amount (atomic units)
    quorum_hash: [u8; 32],// Poseidon4(pk_ed_1, pk_ed_2, pk_ed_3, pk_nullifier) — bound spend + nullifier policy
    pk_view: [u8; 32],    // BabyJubJub public key for recipient decryption
    rho: [u8; 32],        // Nullifier uniqueness seed
    r: [u8; 32],          // Blinding factor for hiding commitment
}
```

* **Note Commitment:**
  $$C = \text{Poseidon6}(\text{asset\_id}, \text{amount}, \text{quorum\_hash}, \text{pk\_view}, \text{rho}, \text{r})$$
* **Merkle State & Shuttle-Safe Ring Buffer:** Commitments are appended to an on-chain **Incremental Merkle Tree** of depth 32. To prevent queued proofs from expiring during 4–12 hour "Shuttle" batching windows under 1,000+ daily cover transactions, the contract maintains a ring buffer of **`ROOT_HISTORY_SIZE = 4,096`** historical state roots and **`256`** historical Association Set (ASP) roots.
* **Nullifier Computation:** To prevent double-spending without revealing which note was spent:
  $$\text{Nullifier} = \text{Poseidon3}(sk_{nullifier}, \text{rho}, \text{commitment\_index})$$

### 4.3 Privacy-Preserving Dual-Recipient Encryption & Hidden Gateway Keys
To avoid heavy ElGamal bit-unpacking constraints in-circuit and **prevent chain analytics from stripping out retail cover traffic via public gateway keys**, encryption uses **BabyJubJub ECDH + Poseidon Sponge AEAD** with **private gateway verification**:

1. **Circuit-Efficient ECDH + Poseidon Encryption:**
   * Using an ephemeral secret $k_{eph}$ (with public $R_{eph} = k_{eph} \cdot G_{\text{BabyJubJub}}$), the sender computes BabyJubJub ECDH curve points $S_{rec} = k_{eph} \cdot pk_{view\_rec}$ and $S_{comp} = k_{eph} \cdot pk_{gateway\_epoch}$, then derives symmetric keys from their affine coordinates:
     $$K_{rec} = \text{Poseidon2}(S_{rec}.x, S_{rec}.y), \quad K_{comp} = \text{Poseidon2}(S_{comp}.x, S_{comp}.y)$$
   * Both ciphertexts $C_{rec}$ and $C_{comp}$ encrypt the exact same note plaintext via Poseidon sponge encryption inside the circuit (~5–10x fewer constraints than naive ElGamal).
2. **Hidden `pk_gateway_epoch` (Anti-Filter Protection) & Epoch Key Rotation:**
   * **Never expose `pk_gateway_epoch` as a Public Input.** If a company's gateway key were public on-chain, analytics tools could immediately isolate that company's transactions from retail cover traffic (`k = 1`).
   * Instead, an on-chain **Approved Gateway Registry Merkle Root ($G_{root}$)** (updated on-chain to contain only currently active epoch leaves) is passed as a Public Input, while `(gateway_id, epoch, pk_gateway_epoch)` and its Merkle proof in $G_{root}$ are **Private Inputs**.
   * **Epoch Rotation:** Gateways publish rotating epoch keys (e.g., monthly/quarterly) in $G_{root}$, ensuring that even if a historical key is subpoenaed or compromised, blast radius is strictly bounded to that epoch rather than exposing years of payroll history.
   * **Cover Traffic Parity:** Verified retail cover transfers encrypt $C_{comp}$ to an active ASP/Gateway epoch key from the **same $G_{root}$**, making B2B and retail transactions structurally indistinguishable on-chain.

#### The ZK Proof of Equivalence & Quorum Circuit
The circuit validates all constraints in zero-knowledge:
1. **2-of-3 Threshold Spend & Nullifier Binding:** Two valid BabyJubJub EdDSA signatures from distinct keys in `(pk_ed_1, pk_ed_2, pk_ed_3)` sign the spend digest $\mu$, `Poseidon1(sk_nullifier) == pk_nullifier`, and `Poseidon4(pk_ed_1, pk_ed_2, pk_ed_3, pk_nullifier) == note.quorum_hash`.
2. **State & Clean Association Set:** The spent note exists in state root $R$ and its deposit commitment belongs to the approved **Clean Association Set ($ASP_{root}$)**.
3. **Gateway Registry Inclusion:** `pk_gateway_epoch` belongs to the active on-chain Gateway Registry root $G_{root}$ for the current valid epoch.
4. **Dual-Encryption Equivalence:** $C_{rec}$ and $C_{comp}$ encrypt the exact same $(\text{asset\_id}, \text{amount}, \text{quorum\_hash}, \text{rho}, \text{r})$ note payload under $K_{rec}$ and $K_{comp}$ respectively.
5. **Nullifier & Value Conservation:** `Nullifier == Poseidon3(sk_nullifier, rho, commitment_index)` and $\sum \text{inputs} = \sum \text{outputs} + \text{withdrawal\_amount} + \text{relayer\_fee}$.

```
Public Inputs:
├── State Merkle Root (R)           [Checked against 4,096-root ring buffer]
├── Clean Association Set (ASP_root)[Checked against 256-root ring buffer]
├── Gateway Registry Root (G_root)  [Hides which gateway/epoch was used!]
├── Nullifier(s) & Output Commitment(s)
├── Ephemeral Public Key (R_eph)
├── Ciphertext_Recipient (C_rec) & Ciphertext_Compliance (C_comp)
└── Public Withdrawal Digest (amount, recipient, relayer, fee — 0 for internal transfers)

Private Inputs:
├── Note details (asset_id, amount, rho, r, quorum_hash)
├── Quorum Public Keys (pk_ed_1, pk_ed_2, pk_ed_3) + 2 EdDSA Signatures (sig_a, sig_b)
├── sk_nullifier, pk_nullifier & Merkle Path / ASP Path
├── pk_gateway_epoch + Gateway Merkle Path in G_root
└── Ephemeral scalar k_eph
```

### 4.4 The "ReadOnly" Auditor Interface (0/3 Signing Power)
1. **Profile Export:** The enterprise exports a JSON payload containing $sk_{view}$ (never $sk_{ed,i}$ and never $sk_{nullifier}$) and the gateway attestation identifier.
2. **Auditor Client:** The auditor (PwC, internal compliance, tax inspector) loads this profile into an **Auditor-Mode Vultisig Desktop/Web Client**.
3. **Deterministic Decryption:** The auditor client performs BabyJubJub ECDH ($S_{rec} = sk_{view} \cdot R_{eph}$) and decrypts matching $C_{rec}$ payloads:
   $$K_{rec} = \text{Poseidon2}(S_{rec}.x, S_{rec}.y), \quad \text{Decrypt}(K_{rec}, C_{rec})$$
4. **Instant Audit Report:** Successfully decrypted notes reconstruct the full incoming/outgoing invoice journal (timestamp, counterparty identifier, exact USDC amount, invoice memo).
   * **Security Guarantee:** $sk_{view}$ cannot generate nullifiers (as `sk_nullifier` is separate and never exported) and cannot produce the 2-of-3 BabyJubJub EdDSA signatures required by the circuit. Even total compromise of the auditor's machine leaves treasury funds 100% unspendable.

### 4.5 Own-Pool Fork Economics, Amount-Leak Prevention & Mobile Proving
Integrating directly with an external third-party Railgun deployment imposes a hardcoded **0.25% shield + 0.25% unshield** protocol tax plus third-party broadcaster markups, while upstream audits do not cover custom dual-encryption or 2-of-3 EdDSA circuits. Vultisig addresses both security and unit economics with four strict engineering rules:

1. **Clean-Room Fork + Samourai-Immune Zero-`%` Withdrawal Policy + Independent Audit:** Fork MIT-licensed `0xbow-io/privacy-pools` / `railgun-privacy/contract` foundations (`0%` external DAO tax and **`0%` Vultisig protocol percentage toll on shielded deposits, internal transfers, or withdrawals**). Eliminating percentage-based fees on private withdrawals removes the core *Samourai Wallet* coordinator-fee enforcement vector: Vultisig monetizes B2B SaaS software licenses and standard asset swaps, while relayer nodes earn only **flat gas/batch-slot relay fees** (identical whether moving `$500` or `$500,000`). Because adding 2-of-3 EdDSA + hidden gateway verification modifies the Circom/Noir circuits, **Phase 2 Mainnet launch explicitly gates on a dedicated circuit + contract audit and a Phase-2 Trusted Setup ceremony (or Noir/UltraHonk)**.
2. **Native Mobile Proving (No SnarkJS in WebView):** Generating ~60k–90k constraint proofs in `snarkjs`/JS on budget Android phones in LatAm/Africa takes 15–30+ seconds and risks OOM crashes. Production mobile SDKs bundle **native C++/Rust `rapidsnark` (or Noir Barretenberg)** bindings alongside `@vultisig/lib-dkls` (built in Phase 2 once Stage-Gate #1 proves demand), targeting `< 2–3s` client proving times.
3. **Two-Mode Payouts (Eliminating Odd-Amount Unshield Fingerprints):**
   * *Internal Shielded Payroll (Zero Amount Leakage):* Exact odd amounts (e.g., `$3,420.50`) transfer **shielded-to-shielded** inside the pool (`Note_Company -> Note_Employee`). No amount ever touches public L2 calldata.
   * *Public EOA Unshields (Strict Denomination Quantization):* Whenever funds exit the pool to a public address, the SDK enforces **fixed quantized denominations** (`$100`, `$500`, `$1,000`, `$5,000`) dispatched across **randomized cross-window jitter** inside the "Shuttle" queue—never dumping `$1,000 + $1,000 + $1,000 + $420.50` in the same batch. Any odd remainder stays inside the corporate shielded note as internal change.
4. **Separation of Duties for Permissionless `$VULT` Nodes vs Independent ASPs:**
   * **Permissionless `$VULT` Infrastructure Nodes (Phase 3, Post-Critical Mass):** Verify public ASP inclusion proofs, execute ERC-4337 Paymaster / FastVault Co-signer SLA duties, and run the dynamic "Shuttle" batch queue for **flat per-tx/slot gas fees** under deterministic Timeout Proof slashing.
   * **Independent Accredited ASPs (Non-Node Governance):** Licensed compliance entities—never Vultisig Labs and never anonymous gas relayers—sign periodic updates to `ASP_root` and `G_root`, isolating tainted deposits while shielding relayer operators from discretionary AML/MSB liability.

---

## 5. Turnkey Neobank WaaS: The "Grandma-Friendly" MPC Architecture

```
  ┌─────────────────────────────────────────────────────────────┐
  │                 RETAIL USER (SMARTPHONE)                    │
  │  - iOS / Android Native App                                 │
  │  - Biometric Secure Enclave (FaceID / TouchID)              │
  │  - Holds: MPC Key Share #1                                  │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                 (P2P Encrypted Session via WebRTC)
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │                   NEOBANK BACKEND SERVER                    │
  │  - High-Availability Docker / K8s MPC Worker                │
  │  - Evaluates Risk Engine, Daily Limits & Anti-Fraud         │
  │  - Holds: MPC Key Share #2                                  │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │                 DECENTRALIZED CLOUD BACKUP                  │
  │  - User's Personal iCloud / Google Drive                    │
  │  - AES-256-GCM Encrypted via User Password                  │
  │  - Holds: MPC Key Share #3 (Cold / Disaster Recovery Only)  │
  └─────────────────────────────────────────────────────────────┘
```

### 5.1 Three-Party Threshold Breakdown (2-of-3 FastVault)
* **Share 1 (User Device):** Generated in the local device runtime via `@vultisig/lib-dkls` (compiled to WebAssembly / Mobile C-bindings). Key-material is secured in the iOS Secure Enclave / Android KeyStore.
* **Share 2 (Neobank Server):** Maintained in a hardened Hardware Security Module (HSM) or secure cloud enclave. The server will only participate in an MPC signing ceremony if the user passes 2FA, biometric push, and velocity checks.
* **Share 3 (Disaster Recovery):** Encrypted using a password known only to the user and saved to their personal cloud storage. It is never accessed during day-to-day operations.

### 5.2 Key Resharing on Device Loss (Zero Loss, Zero Fund Movement)
Traditional seedless wallets face catastrophic failure if a device is destroyed without a seed backup. In this architecture:
1. User loses their phone.
2. User installs the Neobank app on a new device and completes Tier-2 Identity Verification (e.g., biometric selfie match against passport database).
3. The Neobank authenticates the user and authorizes an **MPC Resharing Protocol** (supported natively by DKLS23):
   * **Active Quorum:** Neobank Server (Share 2) + Disaster Cloud Backup (Share 3).
   * **Output:** A new Share 1 is generated and installed on the new device.
   * **Invalidation:** The old Share 1 on the lost device is mathematically invalidated.
4. **Key Benefit:** The blockchain wallet address (Base/Arbitrum) **does not change**. Assets are not transferred, triggering zero tax events and zero network transaction fees.

### 5.3 Gasless Account Abstraction (Paymaster Integration)
The user holds only stablecoins (e.g., Base.USDC). They never buy ETH for gas:
* The transaction is wrapped as an ERC-4337 UserOperation or routed via an MPC Gas Relay.
* The neobank sponsors the negligible L2 gas fee ($0.001 on Base) or automatically deducts a $0.05 fee directly in USDC.
* **UX Result:** The user opens the app, enters "Send $20 to Maria", confirms with FaceID, and the transfer settles in 1 second.

### 5.4 WaaS & B2B Multi-Chain SaaS Monetization (Stage 1 Revenue Engine)
Unlike ZK-Shielded Pools—which require external circuit audits, multi-platform native provers, and initial $k$-anonymity liquidity—**Neobank WaaS and B2B Multi-Chain Treasury SaaS leverage Vultisig’s existing production DKLS stack across BTC, SOL, and EVM** and monetize immediately with higher unit margin and zero mixing/withdrawal-fee risk:

| Revenue Stream | Pricing Structure | Example Partner Economics (50k MAU Neobank / B2B Stack) |
| :--- | :--- | :--- |
| **1. WaaS & B2B Treasury SaaS** | `$2,000 – $5,000 / mo` WaaS license; `$400 – $1,500 / mo` B2B multi-chain & ERP suite | `$3,000 / mo` WaaS baseline (or `$500/mo` per B2B team) |
| **2. Active Wallet Usage (MAU)** | `$0.10 – $0.20 / active user / month` above 10k free tier | `(50k - 10k) × $0.15 = $6,000 / mo` |
| **3. Clean Swap / Bridge / FX Routing** | **`0.25% – 0.40%` (25–40 bps)** standard UI/router fee on FX, swaps & cross-chain rebalancing (`BTC↔SOL↔USDC`) | `$15M/mo swap volume × 0.30% = $45,000 / mo` |

* **Why Clean Swaps + SaaS Beats Withdrawal Tolls (`3x–4x` Margin, `4x` Lower CAPEX):** On `$100M` of volume, keeping `0.30%` on standard swaps yields **`$300k`** for the protocol vs **`$100k`** from a `0.10%` protocol share of a `0.25%` ZK withdrawal fee—with **zero** DOJ *Samourai* coordinator-fee exposure and **4–5x lower upfront R&D** (`$125k–$165k` vs `$565k–$810k`).
* **Why B2B Teams & Fintechs Pay:** Regional neobanks avoid `$500k+` in-house MPC R&D, while Web3 treasuries get **native BTC + SOL + EVM custody with 1-click cross-chain swaps** (impossible in EVM-only Safe multisigs).
* **The Flywheel Effect:** Stage 1 builds recurring USDC revenue and seeds **both** the resting L2 USDC TVL and the KYC-attested transaction density needed before unlocking Phase 2 ZK privacy and Phase 3 nodes.

---

## 6. Two-Stage Business Model, Samourai-Safe Fees & $VULT Tokenomics

Vultisig avoids two fatal Web3 traps: (1) forcing corporate CFOs to hold volatile tokens, and (2) launching idle "yield nodes" before transaction volume exists—or extracting risky `%` tolls on private withdrawals. Instead, value capture follows a clean **two-stage sequence**:

```
  [ STAGE 1 (M1–M4): B2B SaaS + WaaS + CLEAN SWAPS ]   (ZERO NODES NEEDED YET)
  • WaaS & B2B Treasury SaaS + MAU + ERP Connectors
  • 0.25%–0.40% Clean Swap/Bridge Routing (BTC/SOL/EVM)
                        │
        ┌───────────────┴───────────────┐
        ▼                               ▼
  [ PROGRAMMATIC $VULT BUYBACKS ]   [ OPTIONAL SWAP & SAAS ]
  (25% Net USDC Profit -> $VULT)    [  FEE-DISCOUNT LOCKS  ]
                        │
  ══════════════════════▼══════════════════════════════════════════════════
  [ STAGE 2–3 (POST-CRITICAL MASS): ZK PRIVACY + INFRASTRUCTURE NODES ]
  • Shielded Pool Protocol Toll: 0% (Zero % tax on private withdrawals!)
  • B2B Privacy monetized via Enterprise SaaS + ERP Compliance Suite
  • Permissionless Nodes (Bond 100k $VULT) perform REAL paid work:
     1. Decentralized FastVault Co-signer & ERC-4337 Paymaster -> Flat Gas Spread + SLA
     2. Stateless ZK Shuttle Batcher -> Flat $0.75–$2.00/slot (Amount-Agnostic!)
```

### 6.1 Stage 1 Value Capture (No Idle Nodes) vs Stage 3 Permissionless Nodes
* **Why Zero Nodes in Stage 1:** Paying passive "node rewards" from SaaS or swap revenue when nodes do no infrastructural work wastes margin and creates securities (Howey) risk. In Stage 1, 100% of surplus value flows cleanly into **$VULT via programmatic open-market buybacks (`25%` of net USDC revenue)** and **optional `$VULT` discount locks**.
* **When Permissionless Nodes Launch (Stage 3 — Post-Critical Mass):** Once WaaS and B2B clients generate sustained transaction volume, anyone can bond **100,000 `$VULT`** in an on-chain registry contract on Arbitrum/Base to earn real USDC for two concrete infrastructure services (without touching user custody or breaking local DKLS Keysign):
  1. **Decentralized FastVault Co-signer & ERC-4337 Paymaster Relayer:** Eliminates single-server reliance on Vultisig Labs for WaaS Share #2 policy checks and sponsors L2 gas—earning **flat per-UserOp USDC gas spreads (`~$0.03–$0.05` net)** plus **Co-signer SLA infrastructure rewards** funded from the WaaS SaaS budget.
  2. **Stateless ZK Shuttle Batcher (Zero-`%` Withdrawal Toll):** Submits batched ZK-proofs and pays L2 gas under deterministic **Timeout Proof slashing** (`7–14 day` jail + `1%–10%` bond burn for dropped/stalled jobs). Nodes earn a **flat per-slot relay fee (`$0.75–$2.00` per batch slot, independent of `$500` or `$500,000` inside)**—functioning strictly as a standard ERC-4337/Flashbots bundler rather than a `%` mixing toll.

### 6.2 Optional Fee-Discount Locks (Zero Mandatory Corporate Buying)
Corporations are **never required** to buy or hold `$VULT`. Pure USDC billing is the default; locking `$VULT` is strictly an **optional optimization** on **clean Swap/Bridge routing and SaaS quotas** (never tied to private withdrawal percentages):
* **Default Enterprise Rate (Zero Lock):** Standard **`0.30%–0.35%`** swap/FX router fee + flat USD/USDC SaaS subscription (`0%` shielded withdrawal fee).
* **Optional Tier 1 (Lock 50,000 $VULT):** Reduced **`0.22%`** swap/bridge router tier + included B2B SaaS seat/MAU quota.
* **Optional Tier 2 (Lock 100,000 $VULT):** Lowest **`0.15%`** swap/bridge router tier + priority Paymaster/Shuttle SLA routing for high-volume PSPs, WaaS partners, and OTC desks.
* **Enterprise Privacy & ERP Suite (ZK-Pool Add-On):** `$500 – $1,500 / month` SaaS add-on for automated QuickBooks/Xero/NetSuite ReadOnly (`sk_view`) audit log connectors and gateway compliance orchestration.

### 6.3 Realistic Revenue & Token Sink Scenarios (Stage-Gated Math)
Because monetization rests on **B2B/WaaS SaaS + clean Swap Routing** (rather than taxing private withdrawals), Vultisig generates higher net protocol revenue from Month 6 onward with zero *Samourai* exposure, while Phase 3 nodes earn real USDC yield from flat high-frequency gas spreads and SLA slots:

| Metric | Stage 1 Live (Month 6 — Pre-Nodes) | Base Case (Month 15 — Full Flywheel) | Scale Case (Month 24) |
| :--- | :--- | :--- | :--- |
| **Active WaaS & B2B SaaS Clients** | 3 WaaS (`60k` MAU) + 15 B2B teams | 12 WaaS (`300k` MAU) + 60 B2B teams | 30 WaaS (`1M` MAU) + 180 B2B teams |
| **Monthly Clean Swap / FX Volume** | `$20M / mo` (`0.30%`) | `$85M / mo` (`0.30%`) | `$280M / mo` (`0.25%`) |
| **SaaS + MAU + ERP Suite Revenue** | **`$20,000 / mo`** | **`$95,000 / mo`** | **`$285,000 / mo`** |
| **Clean Swap / Bridge Router Revenue** | **`$60,000 / mo`** | **`$255,000 / mo`** | **`$700,000 / mo`** |
| **Total Protocol Net Revenue (USDC)** | **`$80,000 / mo`** (`$960k/yr`) | **`$350,000 / mo`** (`$4.2M/yr`) | **`$985,000 / mo`** (`$11.8M/yr`) |
| **Shielded Pool `%` Withdrawal Tax** | `N/A` (`0%` always) | **`0%`** (Flat bundler fee only) | **`0%`** (Flat bundler fee only) |
| **Node Operator USDC Yield (Flat Paymaster/SLA + Batch Slots)** | `$0` (Nodes not launched yet) | **`~$65,000 / mo`** (`~$7.8k/yr` per node) | **`~$215,000 / mo`** (`~$11.7k/yr` per node) |
| **Annualized `$VULT` Buyback Pressure (25% Net Revenue)** | **`$240,000 / yr`** | **`$1,050,000 / yr`** | **`$2,955,000 / yr`** |
| **Permissionless Nodes (`100k $VULT` each)** | `0` (Launched in Phase 3) | 100 nodes (`10.0M $VULT`) | 220 nodes (`22.0M $VULT`) |
| **Optional Partner Swap/SaaS Locks (`50k–100k`)** | 6 partners (`0.5M $VULT`) | 25 partners (`2.0M $VULT`) | 60 partners (`5.0M $VULT`) |
| **Total Supply Locked (% of 100M Cap)** | **`0.5M (0.5%)`** | **`12.0M (12.0%)`** | **`27.0M (27.0%)`** |

**Key Economic Takeaway:** Stage 1 reaches **`~$80k/mo`** with zero ZK circuits, zero nodes, and zero withdrawal-tax risk—proving product-market fit before Phase 2 ZK capital is deployed. At scale, buybacks (`$1.05M–$2.95M/yr`), optional swap/SaaS locks, and real-yield Paymaster/Batcher nodes lock `12%–27%` of supply sustainably.

---

## 7. Comparative Technical Benchmark

| Feature | Safe (Gnosis) | Fireblocks | MPCVault | Tornado Cash | **Vultisig Enterprise** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Cryptographic Scheme** | Smart Contract Multi-Sig | MPC (Cloud Enclaves) | 3-of-3 MPC (Azure/GCP) | Single Private Key | **DKLS TSS + 2-of-3 ZK EdDSA** |
| **On-Chain Anonymity** | None (Public) | None (Public) | None (Public) | High (Tainted) | **High (Compliant ZK, Hidden Gateway)** |
| **Auditing Protocol** | Public Explorer | SaaS DB Logs | SaaS DB Logs | None (Mixer) | **ECDH+Poseidon Dual View-Keys** |
| **Sanctions Screened** | Front-end only | Centralized API | Centralized API | None (Toxic) | **Independent ASP Proof of Innocence** |
| **Regulatory Status** | Neutral | Heavy Licensee | Heavy Licensee | Sanctioned / OFAC | **Exempt Non-Custodial Software** |
| **Cross-Chain Breadth** | EVM Only | Multi-Chain | Multi-Chain | EVM Only | **WaaS: BTC/SOL/EVM; ZK-Pool: EVM L2** |
| **Turnkey Neobank WaaS** | No (Too complex) | Enterprise Only | Enterprise Only | No | **Native FastVault 2-of-3 SDK** |

---

## 8. Realistic Cost Structure, Open-Source Map & Phased Budget Breakdown

A realistic engineering view is critical here: while **~75–85% of foundational primitives** (Merkle trees, Poseidon, EdDSA verifiers, EAS, DKLS) exist in audited MIT/Apache repositories, **modifying circuits to add 2-of-3 EdDSA + hidden gateway inclusion + dual ECDH encryption invalidates upstream circuit audits**. Building a production-grade WaaS platform alongside audited ZK-privacy infrastructure takes **12–14 months** and **`$565,000 – $810,000`** in gross capital (with Phase 1 WaaS revenue and L2 ecosystem grants offsetting a substantial share of Phase 2–3 costs).

### 8.1 Open-Source Cryptographic Asset Map

| Functional Layer | Battle-Tested Open-Source Repository | License | Implementation Role |
| :--- | :--- | :--- | :--- |
| **Shielded Pool Smart Contracts** | [railgun-privacy/contract](https://github.com/railgun-privacy/contract) & [0xbow-io/privacy-pools](https://github.com/0xbow-io) | MIT | Base EVM contracts for Merkle trees (expanded to 4,096-root ring buffer), nullifiers, and ASP root registries. |
| **ZK Circuits & Mobile Prover** | [iden3/circomlib](https://github.com/iden3/circomlib) + [iden3/rapidsnark](https://github.com/iden3/rapidsnark) (or Noir/UltraHonk) | GPL-3.0 / MIT | Audited `EdDSAPoseidonVerifier`, BabyJubJub ECDH, Poseidon sponge AEAD, and native C++/ARM proving (`<3s` on mobile). |
| **On-Chain Attestations** | [ethereum-attestation-service/eas-contracts](https://github.com/ethereum-attestation-service/eas-contracts) | MIT | Standardized contract infrastructure for licensed gateway KYB passes and epoch key registries. |
| **Threshold Core (TSS)** | [vultisig/lib-dkls](https://github.com/vultisig/lib-dkls) & [@vultisig/sdk](https://github.com/vultisig/vultisig-windows) | Apache-2.0 | Vultisig's existing WASM/native DKLS23 engine + P2P WebRTC quorum coordination. |

### 8.2 Detailed Stage-Gated Budget & Cost Breakdown

To avoid building unvalidated ZK infrastructure without paying customers, capital is strictly **stage-gated**: Phase 1 commits only **`$125k–$165k`** on existing DKLS rails to win paying WaaS/B2B clients; Phase 2 ZK engineering & audits unlock **only after Stage-Gate #1 is passed**.

#### Phase 1 (Months 1–4): Lean Commercial WaaS + B2B Treasury SaaS + Swaps (Existing DKLS) — **`$125,000 – $165,000`**
| Line Item | Scope & Market Assumptions | Cost Range |
| :--- | :--- | :--- |
| **Senior MPC / Mobile SDK Engineer** | 4 months × `$12k–$14k/mo` (`@vultisig/waas-sdk`, FastVault Share-2 HSM worker, Key Resharing) | `$48,000 – $56,000` |
| **Full-Stack / Paymaster / Swap Engineer** | 4 months × `$9k–$11k/mo` (ERC-4337 USDC Paymaster, `25–40 bps` BTC/SOL/EVM Swap Router) | `$36,000 – $44,000` |
| **B2B Treasury SaaS & ERP Connector Dev** | 2–3 months sprint (Multi-chain approval policies, QuickBooks/Xero export, `$VULT` buyback/discount hooks) | `$18,000 – $28,000` |
| **Paymaster / Router Security Review & Infra** | Focused review of ERC-4337 Paymaster & swap router (`$12k–$20k`) + RPCs, cloud HSM, test devices | `$23,000 – $37,000` |

> **Stage-Gate #1 (End of Month 4):** Phase 2 ZK capital commits **only** upon securing `2–3` paying WaaS/B2B pilots (`$40k+/mo` SaaS+swap run-rate) or signed B2B enterprise LOIs.

#### Phase 2 (Months 5–10): Customer-Gated ZK-Core, Independent Audits & B2B Mainnet — **`$325,000 – $480,000`**
| Line Item | Scope & Market Assumptions | Cost Range |
| :--- | :--- | :--- |
| **ZK Circuit & Pool Engineering** | 4–5 months senior ZK/Solidity (`2-of-3 EdDSA` + `pk_nullifier` + hidden `G_root` + 4,096-root pool) | `$55,000 – $70,000` |
| **Custom ZK Circuit & Pool Audit** | Top-tier / specialized ZK firm (Veridise, zkSecurity, ABDK, Trail of Bits); 8–12 weeks incl. queue | `$120,000 – $220,000` |
| **Ceremony + Initial Bug Bounty** | Phase-2 Trusted Setup ceremony (`$20k–$35k`, **$0 if Noir/UltraHonk**) + Immunefi launch reserve | `$30,000 – $60,000` |
| **4-Platform Native Prover & Auditor UI** | Native `rapidsnark` / Noir bindings (iOS/Android/Desktop/Web `<3s`), Auditor UI, ASP/Gateway APIs | `$105,000 – $110,000` |
| **Legal & Gateway Structuring** | Non-custodial + zero-`%`-withdrawal legal opinion (US/EU) + agreements with 2–3 gateways/ASPs | `$15,000 – $20,000` |

#### Phase 3 (Months 11–14): Permissionless `$VULT` Co-Signer, Paymaster & Shuttle Nodes — **`$115,000 – $165,000`**
| Line Item | Scope & Market Assumptions | Cost Range |
| :--- | :--- | :--- |
| **Staking & Slashing Contract Audit** | Audit of 100k `$VULT` registry, Timeout Proof slashing, flat-fee Paymaster/Batcher & SLA splitter | `$35,000 – $55,000` |
| **Node Daemon & Tokenomics Dev** | 4 months (Go/Rust Co-signer/Paymaster/Shuttle daemon, slashing watcher, optional swap/SaaS lock tiers) | `$70,000 – $95,000` |
| **Node Testnet Incentives & Ops** | Incentivized testnet for external operators + SLA telemetry/alerting infrastructure | `$10,000 – $15,000` |

#### Summary Capital Plan (14-Month Full-Stack Total)
* **Upfront At-Risk Capital (Phase 1 Only):** **`$125,000 – $165,000`** (already builds a standalone, revenue-generating B2B/WaaS + Swap product on existing DKLS).
* **Full 3-Phase Gross Total (If Stage-Gate #1 Unlocks ZK & Nodes):** **`$565,000 – $810,000`** across 12–14 months, with Phase 2–3 heavily co-funded by Stage 1 WaaS/Swap cashflow (`~$80k+/mo` by Month 6) and Base/Arbitrum ecosystem grants (`$75k–$200k`).

---

## 9. Strategic Horizon (Post-Mainnet R&D — Outside Core 14-Month Budget): Zero-Trust Identity & Anti-Deepfake Protocol

*Scope Note: The capabilities in this section represent long-term post-Mainnet R&D expansion and are explicitly **excluded** from the Phase 1–3 (`$565k–$810k`) execution budget above.*

Vultisig’s underlying technology is not merely a financial transfer mechanism—it is a **decentralized, threshold-secured cryptographic identity layer**. In an era where Generative AI has destroyed the trustworthiness of audio and video communications, Vultisig possesses the exact mathematical architecture to secure corporate communications and critical data assets.

```
       [ Inbound Video Call / Zoom Request ]
                         │
        (Generative AI Voice & Face Clone)
                         │
                         ▼
        ┌───────────────────────────────────┐
        │     Vultisig Background Agent     │
        │      (Zero-Trust Handshake)       │
        └─────────────────┬─────────────────┘
                          │
             Sends Ephemeral Nonce Challenge
                          │
                          ▼
        ┌───────────────────────────────────┐
        │   Physical CFO iPhone / Device    │
        │   (Holds Hardware MPC Key Share)  │
        └─────────────────┬─────────────────┘
                          │
            Signs Nonce inside Secure Enclave
                          │
                          ▼
    ┌───────────────────────────────────────────┐
    │  Green Badge: "Identity Verified via MPC" │
    │  (AI can fake video, but NOT the physics) │
    └───────────────────────────────────────────┘
```

### 9.1 Anti-Deepfake Voice & Video Authentication
Recent high-profile exploits (such as the high-profile impersonation attacks against crypto leaders and the $25 Million Hong Kong multinational video-conference deepfake fraud) demonstrate that visual and vocal identification is obsolete.

* **The Problem:** Attackers use real-time voice synthesis and video deepfakes to impersonate executives, authorizing fraudulent transfers or requesting emergency credentials.
* **The Vultisig MPC Solution:** An ephemeral, client-side zero-knowledge challenge-response protocol integrated into corporate communication tools (Slack, Zoom, Google Meet).
  1. When an executive initiates an operational request or executive video call, the caller's desktop client broadcasts an ephemeral cryptographic challenge (nonce).
  2. The executive’s smartphone—holding an isolated Vultisig MPC share in its hardware Secure Enclave—prompts a silent biometric approval (FaceID).
  3. The device signs the nonce off-chain and returns the cryptographic proof.
  4. The receiver's screen displays an immutable **"Identity Verified via Physical MPC Hardware"** badge.
* **Why Attackers Fail:** Generative AI can clone pixels and audio frequencies, but it cannot synthesize an off-chain cryptographic signature from a physical hardware enclave located miles away.

### 9.2 Threshold-Encrypted Enterprise Data Vaults
Beyond financial transactions, global institutions face catastrophic liabilities from cloud database breaches and compromised storage repositories.

* **Decentralized Data Sharding:** Sensitive corporate assets (trade secrets, M&A agreements, intellectual property) are encrypted using a 2-of-3 threshold encryption scheme.
* **Zero Single Point of Breach:** One key share resides with the employee, one with the company's internal compliance server, and one in an offline recovery enclave.
* **Breach Immunity:** Even if an attacker completely compromises the enterprise's AWS cloud or internal file servers, the stolen files are mathematically indistinguishable from random noise. Decryption requires a live, policy-governed MPC quorum across physical devices.

---

## 10. Detailed Stage-Gated Launch Sequence & 14-Month Engineering Roadmap

```
PHASE 1: Lean Commercial B2B SaaS + Neobank WaaS + Clean Swaps (Months 1–4) [$125k–$165k]
├── [M1–M2 | WaaS Core] Package @vultisig/waas-sdk (existing DKLS BTC/SOL/EVM), FastVault Share-2 HSM worker & Key Resharing.
├── [M2–M3 | Monetization] Ship ERC-4337 Paymaster (USDC gas), 25–40 bps Swap/Bridge Router & B2B Treasury SaaS + ERP export.
├── [M3–M4 | Token Flywheel] Activate 25% net USDC revenue -> $VULT buybacks & optional swap/SaaS discount locks (zero nodes yet).
└── [M4    | STAGE-GATE #1] Onboard 2–3 paying neobank/B2B pilots (~$40k+/mo run-rate or signed LOIs) before committing ZK capital.

PHASE 2: Customer-Gated ZK-Core, Audits & B2B Mainnet (Months 5–10) [$325k–$480k | Co-Funded by WaaS + Grants]
├── [M5–M6 | ZK Build] Implement 2-of-3 EdDSA + pk_nullifier quorum_hash + hidden G_root circuits & Base Sepolia 4,096-root pool.
├── [M6–M8 | Audits & Prover] Run 8–12w circuit/contract audit (Veridise/ABDK/Trail of Bits) + 4-platform rapidsnark/Noir (<3s).
├── [M7–M9 | Compliance] Execute non-custodial + 0%-withdrawal-fee legal opinion; integrate 2–3 licensed gateways/ASPs (ASP_root/G_root).
└── [M10   | Mainnet Launch] Layer 0%-tax ZK-Pool onto active Phase 1 WaaS resting TVL & gasless retail cover (initial partner relayers).

PHASE 3: Permissionless $VULT Infrastructure Nodes (Months 11–14) [$115k–$165k]
├── [M11–M12 | Contracts] Build & audit 100k $VULT Registry, Timeout Proof Slashing & flat-fee Paymaster/Batcher/SLA splitters.
├── [M12–M13 | Node Ops] Ship Go/Rust Co-signer/Paymaster/Shuttle daemon & run incentivized operator testnet under SLA stress tests.
└── [M14     | Full Network] Activate Mainnet permissionless nodes earning real USDC via flat gas/batch slots & Co-signer SLA fees.
```

---

## 11. Conclusion: The Strategic Path Forward

The Web3 market does not need another speculative token or an unvalidated `$800k` ZK build with zero customers. At the same time, purely public corporate wallets leave businesses exposed—and legacy privacy pools charging `%` withdrawal tolls carry unacceptable *Samourai/Tornado* legal exposure.

By sequencing **B2B Multi-Chain SaaS + Neobank WaaS + Clean Swaps first (`$125k–$165k`, Months 1–4)** on Vultisig’s existing DKLS stack, Vultisig captures immediate, high-margin revenue (`3x–4x` cleaner margin than withdrawal tolls), drives programmatic `$VULT` buybacks from Day 1, and builds the resting L2 liquidity and retail cover flow organically. Only once **Stage-Gate #1** proves commercial traction does Phase 2 unlock the audited `2-of-3 EdDSA` ZK-Pool (`0%` withdrawal toll), followed in Phase 3 by permissionless `100k $VULT` infrastructure nodes earning sustainable USDC yield from flat gas/batching spreads and Co-signer SLA fees.

**Recommended Action for Core Leadership:** Authorize **Phase 1 (`$125k–$165k`, Months 1–4)** to package `@vultisig/waas-sdk`, B2B Treasury SaaS, and `25–40 bps` swap routing for `2–3` pilot partners—keeping Phase 2 ZK audits strictly gated on signed customer demand.

---

## 12. Appendix: Alternative Execution Sequence via THORWallet Infrastructure

While the primary specification above is architected for Vultisig’s core stack, **the same commercial B2B payroll and compliance architecture can be executed on an accelerated, lower-cost timeline via THORWallet’s existing production infrastructure**—because THORWallet has already solved the hardest part of Stage 1 (live fiat off-ramps, card rails, and Arbitrum KYC attestations).

### 12.1 Why THORWallet’s Starting Position Changes the Sequencing
Unlike a pure wallet protocol starting from zero on fiat rails, THORWallet already operates key Stage 1 building blocks in production:
1. **Live Non-Custodial Card & Swiss IBAN Rails:** Through **Fiat24** (FINMA-regulated, Arbitrum Account NFTs) and **Unblock** (global coverage across 175+ countries, Apple Pay / Google Pay), employees paid in stablecoins can spend directly via Mastercard or settle via Swiss IBAN—**eliminating the "1st/15th payday CEX dump" correlation vector without building neobank partnerships from scratch**.
2. **Existing KYC-Attested Arbitrum Base:** Active THORWallet cardholders already hold on-chain **Fiat24 Account NFTs** on Arbitrum and generate continuous swap/top-up flow.
3. **Existing Native Swap Engine & USDC Yield Tokenomics (`$TITN`):** Native cross-chain swaps (including private swap routes) are already live, and **`$TITN` on Base already distributes 70% of swap fees in USDC to stakers**—removing the need to invent and audit a brand-new node-staking economy.

### 12.2 Alternative Launch Timeline (Months 1–9 | `$355k–$530k` Full Stack, `$65k–$110k` Upfront)

```
TRACK 1 (THORWallet Stage 1): B2B Payroll UI + Card/IBAN Off-Ramp (Months 1–3) [$65k–$110k]
├── [M1–M2 | B2B Dashboard] Build web B2B Payroll & Treasury Console on ported Apache-2.0 DKLS / MPC quorum
│                           with approval limits, batch invoicing, and QuickBooks/Xero accounting export.
├── [M2–M3 | Card Payroll]  Integrate batch employee onboarding to THORWallet Mastercard (Fiat24 / Unblock)
│                           + Swiss IBAN + native BTC/SOL/ETH -> USDC treasury swaps (25–40 bps).
└── [M3    | STAGE-GATE #1] Monetize immediately via B2B SaaS + Swap/FX fees routed into $TITN USDC yield;
                            validate 2–3 paying B2B payroll clients before touching custom ZK circuits.

TRACK 2 (Partner-Cleared Stage 2): Bank-Coordinated Arbitrum ZK-Pool (Months 4–9) [$290k–$420k]
├── [M4    | Bank Sign-Off] Secure explicit compliance pre-clearance from Fiat24 / Unblock:
│                           (a) 0% protocol withdrawal toll (Samourai-immune),
│                           (b) Fiat24 Account NFT / EAS whitelist entry gate,
│                           (c) Mandatory ReadOnly sk_view disclosure to issuing bank for Source-of-Funds.
├── [M4–M6 | ZK Build]      Deploy Arbitrum 4,096-root MIT pool fork with 2-of-3 EdDSA + pk_nullifier quorum_hash
│                           and hidden G_root epoch keys (reusing Fiat24/Unblock as accredited gateways).
└── [M7–M9 | Audit & Live]  Complete Veridise/Trail of Bits audit + native mobile prover; launch corporate
                            shielded payrolls settling into employee wallets & pre-cleared Mastercard spend.

SKIPPED (Phase 3 Saved): No Custom $VULT-Style Node Economy Needed [$0]
└── Uses lightweight flat-fee ERC-4337 Arbitrum bundlers ($0.75–$2.00/slot) while routing B2B swap/SaaS
    surplus directly into the existing $TITN USDC staking pool—saving ~$115k–$165k and 4 months.
```

### 12.3 Critical Compliance Guardrail for THORWallet (Card Program Protection)
Because THORWallet relies on regulated card and IBAN partners (Fiat24 under FINMA; Unblock/Mastercard), **card transactions themselves cannot be pushed into an opaque mixer—and card issuers will freeze any flow whose Source of Funds is hidden from the issuing bank**:
* **What stays hidden:** Employee salaries, contractor rates, and vendor invoices are shielded **from public block explorers (Arbiscan/Etherscan), competitors, and MEV bots** inside the Arbitrum ZK-Pool.
* **What stays verifiable for Fiat24 / Unblock:** Every corporate depositor completes partner KYB, every retail/employee card user enters via a verified Fiat24/Unblock credential, and whenever shielded payroll funds top up a Mastercard or Swiss IBAN, the issuing bank receives automated **ReadOnly (`sk_view`) Source-of-Funds verification**—proving the exact audited corporate origin of funds without exposing it publicly on-chain.

### 12.4 Side-by-Side Strategic Comparison (Vultisig vs. THORWallet)

| Dimension | Primary Path: **Vultisig** | Alternative Path: **THORWallet** |
| :--- | :--- | :--- |
| **Upfront Stage 1 Cost & Time** | **`$125k – $165k`** (Months 1–4): Build WaaS SDK + B2B SaaS + sign fintech partners | **`$65k – $110k`** (Months 1–3): Build B2B Payroll/Treasury UI on existing cards/IBAN & swaps |
| **Full-Stack Total (Incl. ZK)** | **`$565k – $810k`** across 12–14 months (incl. Phase 3 `$VULT` nodes) | **`$355k – $530k`** across 8–9 months (skips card build & Phase 3 custom node staking) |
| **Strongest Structural Edge** | Purpose-built `2-of-3` DKLS multi-device corporate quorum + pure open-source perimeter (zero banking partners at risk) | Live **Mastercard + Swiss IBAN** off-ramp in 175+ countries, active Arbitrum Fiat24 NFTs, and live `$TITN` USDC Real Yield |
| **Primary Execution Risk** | Must win 2–3 WaaS/B2B pilots first to create L2 retail cover flow & off-ramps | Must build B2B quorum UX and secure explicit **Fiat24/Unblock compliance clearance** before enabling any ZK-Pool |
| **Token Value Mechanism** | Stage 1 `$VULT` buybacks (25% net revenue) → Stage 3 `100k $VULT` Co-signer/Paymaster/Batcher nodes | Immediate routing of B2B swap/FX & SaaS surplus into existing **`$TITN` (70% USDC Fee Return)** staking + discounts |

**Bottom Line for Both Teams:** Vultisig holds the stronger **pure-play infrastructure & multi-device corporate MPC** foundation with zero banking-partner dependency, while THORWallet holds the faster **go-to-market wedge for B2B payroll-to-card** because its Mastercard/IBAN rails and Arbitrum KYC base are already live. In either architecture, **shipping B2B SaaS + swaps + off-ramps first (Track 1) and gating ZK-privacy behind signed corporate demand and a strict `0%` withdrawal-tax policy** eliminates upfront capital waste and *Samourai*-style regulatory risk.

