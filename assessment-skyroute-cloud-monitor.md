# Assessment — SkyRoute Cloud Monitor: Weak Auth + Advanced SSRF + Filter Bypass

---

## Question 1 — MCQ

**Which vulnerability allowed the attacker to gain initial admin access?**

- A) SQL Injection on the login form
- B) JWT token forgery
- C) Session fixation
- D) **Weak authentication — guessable admin credentials** ✅

> **Answer:** D — The admin account uses a predictable password (`Admin@01234`) that succeeds on a brute-force or educated guess attempt.

---

## Question 2 — MCQ

**Why was the initial SSRF attempt using `http://127.0.0.1/` blocked?**

- A) The server crashed
- B) Strong authentication was required
- C) **Basic input filtering blocks the literal strings `127.0.0.1` and `localhost`** ✅
- D) DNS resolution failed

> **Answer:** C — The application performs a simple string-match blacklist check, not blocking alternative representations of loopback addresses.

---

## Question 3 — MCQ

**Which alternative localhost representation bypassed the SSRF filter?**

- A) `http://0x7f000001/`
- B) `http://localhost.localdomain/`
- C) `http://127.0.0.0.1/`
- D) **`http://127.1/`** ✅

> **Answer:** D — `127.1` is a valid shorthand for `127.0.0.1` on Linux. It is not matched by the literal blacklist but resolves to the loopback interface.

---

## Question 4 — Fill in the Blank

**What alternative IP representation is used to bypass the SSRF blacklist and reach internal services?**

**Answer:** `127.1`

> The blacklist checks for `127.0.0.1` and `localhost` as literal strings. `127.1` is a valid shorthand for `127.0.0.1` on Linux but is not matched by the filter, allowing the SSRF request to reach internal services at `http://127.1/skyroute/internal/api/v1/status.php`.

---

## Question 5 — Fill in the Blank

**What is the internal admin key returned by the internal status API endpoint accessed via SSRF?**

**Answer:** `SKY-INT-9921-SECRET`

> Accessing `http://127.1/skyroute/internal/api/v1/status.php` via the SSRF-vulnerable diagnostic tool returns a JSON response containing `"admin_key": "SKY-INT-9921-SECRET"` along with the active database shard identifier.

---

*Lab target:* `http://localhost:8083`
