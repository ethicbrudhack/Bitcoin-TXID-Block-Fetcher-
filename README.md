# 📦 Bitcoin TXID Block Fetcher  
`fetch_txids_from_blocks.py`

> ⚙️ Automatically collects transaction IDs (TXIDs) from sequential Bitcoin blocks  
> using multiple redundant public APIs with intelligent fallback and resume capability.

---

## 🚀 Overview

This utility fetches **transaction IDs** from Bitcoin blockchain blocks.  
It supports several major public blockchain explorers and automatically switches between them  
if a connection fails or rate limits occur.

Progress is stored locally — so you can safely stop and resume scanning at any time.

---

## 🧩 Features

| Feature | Description |
|----------|-------------|
| 🌐 **Multi-API redundancy** | Supports Blockchair, Blockchain.info, Blockstream, and Mempool.space |
| 🔁 **Automatic fallback** | Automatically switches to another API when errors occur |
| 💾 **Persistent resume** | Remembers the last processed block (`last_block.txt`) |
| 📜 **Continuous TXID logging** | Saves all found TXIDs to `txids.txt` |
| ⚡ **Rate-limiting control** | Adds short delays to prevent bans |
| 🧠 **Smart error handling** | Detects and retries failed JSON or HTTP responses |

---

## 🧱 Supported APIs

| Service | Example Endpoint |
|----------|------------------|
| **Blockchair** | `https://api.blockchair.com/bitcoin/raw/block/{block_height}` |
| **Blockchain.info** | `https://blockchain.info/rawblock/{block_height}` |
| **Blockstream** | `https://blockstream.info/api/block-height/{block_height}` |
| **Mempool.space** | `https://mempool.space/api/block-height/{block_height}` |

---

## 📂 Files Used

| File | Purpose |
|------|----------|
| `last_block.txt` | Stores the last processed block height for auto-resume |
| `txids.txt` | Appends all discovered TXIDs |
| *(temporary memory)* | Internal API state management and retry counters |

---

## ⚙️ How It Works

1. The script checks `last_block.txt` — resumes from the saved block height.
2. Selects the first working API endpoint (`find_working_api()`).
3. Requests the block JSON or raw data for the current height.
4. Extracts all **TXIDs** from that block.
5. Saves TXIDs to `txids.txt` and updates `last_block.txt`.
6. On 3 consecutive API failures → switches to a new API automatically.
7. Continues until `END_BLOCK` is reached.

---

## 🧩 Example Configuration

Inside the script:
```python
DEFAULT_START_BLOCK = 1
END_BLOCK = 165000
You can modify these to start and end at any block height you prefer.

📊 Example Output
▶️ Starting fetch from block 120000 to 165000.

📦 Fetching transactions for block 120000...
✅ Found 2487 transactions in block 120000.

📦 Fetching transactions for block 120001...
⚠️ API rate-limited (429) for block 120001, retrying later.
🔁 Switching API...

📦 Fetching transactions for block 120002...
✅ Found 3012 transactions in block 120002.


When finished:

🎯 Found 458,231 total transactions.

⚠️ Error Handling
Error	Script Response
429 (Too Many Requests)	Waits, retries, then switches API
JSON decode error	Skips block and logs warning
Timeout or ConnectionError	Switches API after 3 failures
Missing or corrupted last_block.txt	Resets to starting block
🧠 Internal Logic
API Selection

Each API is tested on a known block height.

The first responding endpoint is selected globally.

On repeated failures → next API is chosen automatically.

Persistent State

last_block.txt is updated after every block processed.

If interrupted, scanning resumes from the next block.

File Append Mode

TXIDs are streamed line-by-line into txids.txt (safe for large runs).

⚙️ Run Instructions
python3 fetch_txids_from_blocks.py


You’ll see live block progress and TXID counts per block.

💾 Output Example (txids.txt)
f91e1f2cba13d994af12debc8cf95d0c4cb75d99b9b8026c7b2a36e41058a9b3
ab8f53b41ed3d9914b53247698ce6f2357d1aa91f62a98d754a41239815d1b42
...

⚡ Performance Notes
Range	Estimated Time	Notes
1,000 blocks	~2–4 minutes	Stable multi-API mode
10,000 blocks	~20–30 minutes	May switch APIs occasionally
Full chain scan	Several hours	Recommended with persistent resume
🔒 Ethical Use

This tool is for blockchain data analysis, auditing, and research.
It uses publicly available API endpoints and does not access private data.

You may:

Collect TXIDs for analytical or research purposes

Build datasets for signature or entropy analysis

You must not:

Spam or overload public APIs

Use it for unauthorized scraping beyond fair-use limits

⚖️ Always respect API rate limits and terms of service.

🪪 License

MIT License
© 2025 — Author: [Ethicbrudhack]

💡 Summary

A robust, resumable, and API-aware Bitcoin block TXID collector.
It’s the foundation step in your ECDSA or blockchain transaction analysis pipeline —
feeding raw transaction IDs for deeper cryptographic processing.

“Every signature begins with a TXID.”
— [Ethicbrudhack]

BTC donation address: bc1q4nyq7kr4nwq6zw35pg0zl0k9jmdmtmadlfvqhr
