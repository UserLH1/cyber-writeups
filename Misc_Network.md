# Misc & Network Write-ups

## 1. 100 Notes Later
**Task:** Connect to a music service.
**Solution:**
The challenge provided a netcat command: `nc 91.99.1.179 60001`.
- Attempted to redirect output to a file: `nc ... > music.mp3`.
- **Obstacle:** The connection timed out.
- **Diagnosis:** The high port (60001) was blocked by my local network firewall.
- **Fix:** Switched to a mobile hotspot (cellular data) to bypass the restriction. Still have problems, probably because of the server.
