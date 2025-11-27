# Reverse Engineering & Game Hacking

## 1. Insert a Coin to Play (GTA 5 Clone)

**Target:** Unity Game

**Goal:** Collect 10 coins (Game limits to 5).

**Solution:**

This was a memory corruption challenge.

1. Used **Cheat Engine** to attach to the game process.

2. Scanned for the integer value of coins (4 bytes).

3. Collected a coin, refined the scan, and identified the memory address.

4. Modified the value to `10`. The game logic triggered and revealed the flag.

---

## 2. Insert a Coin to Play 2 (GTA 6 Clone)

**Target:** Unity Game

**Goal:** Collect 10 coins. Memory editing was patched.

**Solution:**

The hint "who dare to listen" suggests a network listener.

1. **Static Analysis:** Decompiled the game code (`Assembly-CSharp.dll`) using **dnSpy**.

2. **Code Review:** Found the `GameManager` class. The "listening" part was a metaphor for a decryption function.

3. **Decrypt:** Found the encrypted flag string `jldkgssdglkjsdlkj` and the logic (subtract 5 from ASCII value of each char).

4. Wrote a Python script to decrypt the string:

    ```python
    encrypted = "jldkgssdglkjsdlkj"
    flag = ''.join(chr(ord(c) - 5) for c in encrypted)
    print(flag)
    ```

**Flag:** `UVT{Wh4t?!...}`

---

## 3. Random

**Target:** Linux Shared Library (`.x-sharedlib`) and Remote Service.

**Solution:**

1. **Analysis:** Ran the binary locally; it asked for a number.

2. **Tracing:** Used `ltrace ./main.x-sharedlib` to intercept library calls.

    - Observed `srand(time(0))`: The random seed was based on the current timestamp (Predictable PRNG).

3. **Disassembly:** Used `objdump -d -M intel` to inspect the code.

    ```bash
    objdump -d -M intel main.x-sharedlib
    ```

    - Found a comparison: `cmp eax, 0x539`.
    - `0x539` in hex = **1337** in decimal.

4. **Exploit:** Connected to the server and sent `1337`.

**Flag:** `flag{l33t_m3...}`