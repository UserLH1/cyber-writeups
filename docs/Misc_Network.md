# Misc & Network Write-ups

## 1. 100 Notes Later

**Task:** Connect to a music service.

**Solution:**

The challenge provided a netcat command: `nc 91.99.1.179 60001`.

1. Attempted to redirect output to a file:

    ```bash
    nc 91.99.1.179 60001 > music.mp3
    ```

2. **Obstacle:** The connection timed out.

3. **Diagnosis:** The high port (60001) was blocked by my local network firewall.

4. **Fix:** Switched to a mobile hotspot (cellular data) to bypass the restriction.

**Status:** Still experiencing connection issues, likely due to server-side problems.

## 2. Online Encryption
**Challenge:** Analyze a PCAP file related to an insecure "online encryption" service.

**Solution:**
The challenge description warned against trusting online encryption.
1.  **Traffic Analysis:** Opened the `.pcap` file in **Wireshark**.
2.  **Filtering:** Applied the filter `http.request.method == POST` to inspect data submitted by the user. Since the protocol was HTTP (not HTTPS), all form data was visible in plaintext.
3.  **False Leads:** I observed multiple requests to an AES encryption endpoint. I initially tried to manually reproduce the encryption using the keys found in the packets (e.g., "here it is man"). However, the key lengths (14 or 19 characters) were invalid for AES-128, which strictly requires 16 characters. This indicated that re-encrypting was not the solution.
4.  **Discovery:** I found a specific packet where the `input` field contained a Base64 string (identifiable by the `==` padding at the end).
5.  **Decoding:**
    - **Base64:** Decoded the string to reveal: `RPFP{qq545sos12sq608qnn8p2....}`.
    - **Crypto Analysis:** The prefix `RPFP` seemed like a shifted version of the expected flag format.
    - **ROT13:** Applied a Caesar Cipher (ROT13) shift, which successfully decrypted the flag.

**Flag:** `ECSC{dd545fbf12fd608daa8c2...}`