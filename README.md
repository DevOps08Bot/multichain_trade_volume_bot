## Project Summary & Capabilities

This dashboard manages multi‑wallet operations on BSC and ETH: Four.meme integrations, DEX sniping, batch transfers, and a BSC volume generator. It is built with Next.js 16 (App Router) + React 19 and streams real‑time logs via SSE.

- Four.meme (BSC)
  - Contract Buy (helper.tryBuy resolves `tokenManager`, `amountFunds`, `amountMsgValue`)
  - SmartSwapByOrderId
  - Contract Sell (percent/range, auto‑approve, optional WBNB unwrap)
  - Wave/Batch buys with job logs
- Sniping (BSC, BSC Testnet, ETH, ETH Testnet/Sepolia)
  - Pending tx + Block fallback listener
  - Wave buy when a target function is detected
  - Path finder: WETH→Token or WETH→Stable→Token (on Sepolia you can configure `ETH_TEST_STABLES`)
- Transfers
  - BSC Transfer: BNB/Token batch
  - ETH Transfer: ETH/Token batch
  - Custom Transfer: Send any ERC‑20 using a single private key
- Volume Generator (BSC)
  - Temp wallets: fund → buy → sell → rollover
  - Dynamic gas reserve, auto fund/sweep, live logs
- Manual Contract Call
  - Call any contract function with a private key (static preflight)

---

## Frontend Pages & Usage

### Dashboard

- Chain snapshots, total balances, and quick actions.

### BSC Section

- BSC Wallets
  - Choose a group, list wallets, and refresh balances only for the selected ones.
  - Select/Deselect All and export JSON are available.
- Four.meme
  - Contract Buy: enter token, BNB range, and password; select wallets and start. `helper.tryBuy` resolves the correct signature; staticCall preflight prevents blind reverts.
  - SmartSwap: direct call with `orderId`.
  - Contract Sell: percent/range; auto‑approve and optional WBNB unwrap.
  - Batch/Wave Buy: configure wave size/delays; “Show Logs” opens job logs.
- BSC Snipe
  - Select functions (enableTrading/openTrading/startTrading or custom) and trigger a wave buy when seen.
  - Networks: BSC / BSC Test.
- BSC Transfer
  - Mode: BNB / Token; enter target and password; affects selected wallets only.

### ETH Section

- ETH Snipe
  - Same UI as BSC; Networks: ETH or ETH Testnet (Sepolia).
  - On Sepolia, add stables in `ETH_TEST_STABLES` if the token routes through a stable; path finder tries WETH→Token and WETH→Stable→Token.
- ETH Transfer
  - Mode: ETH / Token batch transfer
  - Custom Transfer: send ERC‑20 from a single private key with the following fields:
    1. Private Key, 2) Token Address, 3) Target Address, 4) Amount, plus Network (ETH/ETH Test/BSC/BSC Test)

### Volume (BSC)

- Normal (24h) and Fast (6h). Auto‑fund from source; temp wallets perform buy→sell→rollover. On Stop, sweep remaining funds back to the source.

### Manual Contract Call

- Provide Network, Contract, Function Signature, Args, Value, and Private Key; a static preflight is executed before the tx is sent.

---

## Security Notes

- Wallet DB private keys are encrypted with AES‑256‑GCM. The key entered in “Custom Transfer” is used in‑memory only for that single call.
- Static preflight is used for contract calls; any revert reason is surfaced to the UI logs.
- RPC erişim anahtarlarını `.env` dosyasında saklayın. Depoya koymayın.
- Üretime almadan önce BSC/ETH test ağlarında (BSC Testnet / Sepolia) senaryolarınızı mutlaka deneyin.
