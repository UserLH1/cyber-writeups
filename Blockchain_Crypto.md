# Blockchain & Cryptography Write-ups

## 1. Evil Vending Machine
**Target:** Ethereum Smart Contract Bytecode
**Solution:**
1.  **Decompilation:** Used an online EVM decompiler (Dedaub) to convert bytecode to readable logic.
2.  **Logic Analysis:** The contract required a specific `CALLVALUE` (Ether amount) to satisfy an equation:
    $$(x \times 87) - 15235 = 168,790,173,608$$
3.  **Math:** Solved for X:
    $$x = \frac{168,790,173,608 + 15,235}{87}$$
    $$x = 1,940,117,289$$
4.  **Flag:** Converted the result to Hexadecimal: `0x73a3d729`.
**Flag:** `UVT{0x73...}`
