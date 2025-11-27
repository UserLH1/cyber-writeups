# Forensics Write-ups

## 1. Alternating

**Challenge:** The `Flag.txt` file appears empty (0 bytes), even in hexdump.

**Solution:** Discovering Alternate Data Streams (ADS) on NTFS.

**Steps:**

1. Enumerated the streams associated with the file in PowerShell:

    ```powershell
    Get-Item -Path .\Flag.txt.txt -Stream *
    ```

2. Observed a hidden stream, for example `real_flag.txt`, which I read as follows:

    ```powershell
    Get-Content -Path .\Flag.txt.txt -Stream real_flag.txt
    ```

3. The hidden content contained the flag.

## 2. Flag Shredder

**Challenge:** A disk image received from a formatted USB that appears empty.

**Solution:** Analysis with tools for recovering deleted files (carving).

**Steps:**

1. Explored the image with `7-Zip` to view visible content.

2. Used `foremost` to recover deleted files from the image:

    ```bash
    foremost -i image.img -o recovered_files
    ```

3. Among the recovered files, found a deleted PNG that contained the flag.

## 3. RAMblings of Cornelia

**Challenge:** Memory dump (`.raw`) from a system associated with a person named Cornelia.

**Solution:** Analysis with Volatility to extract files and volatile data.

**Steps:**

1. Ran Volatility plugins to find files in volatile memory (e.g., `windows.filescan`).

2. Used `windows.dumpfiles` to extract the found files, including a `company_intel.zip`.

3. Found a `.txt` file with instructions and a password; used the password to unlock the archive and extracted an image containing the flag.

**Flag:** `UVT{C0rn3l1a...}`

## 4. Dark Web Stories

**Challenge:** A PCAP file captured from a Tor exit node.

**Solution:** Traffic analysis, archive extraction, and flag extraction hidden in an image.

**Steps:**

1. Inspected HTTP streams in the PCAP and identified a download of a `.zip` archive obtained through SQL Injection.

2. The archive was password-protected; found the exchange of information and a series of MD5 hashes in the traffic.

3. Reconstructed the password using the pattern observed in the MD5 hashes (sequential strings) and cracking tools (`john`).

4. After unlocking the archive, extracted an image and used `zsteg` for LSB analysis; the result revealed the flag hidden in the bits.
