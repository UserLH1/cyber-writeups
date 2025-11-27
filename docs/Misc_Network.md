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