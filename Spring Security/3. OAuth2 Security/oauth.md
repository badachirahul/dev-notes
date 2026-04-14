# OAuth2

### 1. What is OAuth?
* `OAuth is an authorization framework that allows third-party application to obtain limited access to user's protected resources without exposing their credentials`

* **The platform giving out `client_id` and `client_secret` is called the `Authorization Server`.**
* **Terminologies:**
    | Real world | OAuth2 term |
    |-----------|-------------|
    | Google | Authorization Server |
    | Eraser | Client Application |
    | You (the user) | Resource Owner |
    | Google's API giving your profile | Resource Server |


---


### 2. Full Detailed Flow:
```
YOUR BROWSER/FRONTEND          ERASER SERVER                    GOOGLE
        │                           │                              │
        │                           │                              │
        │  1. Click "Login          │                              │
        │     with Google"          │                              │
        │ ─────────────────────────►│                              │
        │                           │                              │
        │  2. Here's the Google     │                              │
        │     redirect URL          │                              │
        │ ◄─────────────────────────│                              │
        │                           │                              │
        │  3. Browser redirects to Google                          │
        │ ────────────────────────────────────────────────────────►│
        │    (client_id,                                           │
        │     redirect_uri,                                        │
        │     scope, state,                                        │
        │     response_type=code)                                  │
        │                           │                              │
        │  4. Google shows          │                              │
        │     account chooser       │                              │
        │     + consent screen      │                              │
        │ ◄────────────────────────────────────────────────────────│
        │                           │                              │
        │  5. You click "Continue"  │                              │
        │ ────────────────────────────────────────────────────────►│
        │                           │                              │
        │  6. Google sends          │                              │
        │     AUTH CODE to          │                              │
        │     browser               │                              │
        │ ◄────────────────────────────────────────────────────────│
        │    (?code=4/0AX4XfWh...   │                              │
        │      &state=AMbdmDk...)   │                              │
        │                           │                              │
        │  7. Frontend sends        │                              │
        │     code to               │                              │
        │     Eraser backend        │                              │
        │ ─────────────────────────►│                              │
        │                           │                              │
        │                           │  8. Server-to-Server call    │
        │                           │ ────────────────────────────►│
        │                           │    (code,                    │
        │                           │     client_id,               │
        │                           │     client_secret)           │
        │                           │                              │
        │                           │  9. Google verifies          │
        │                           │     all 3 ✅                 │
        │                           │     sends back:              │    
        │                           │                              │ 
        │                           │ ◄────────────────────────────│
        │                           │    (access_token,            │
        │                           │     id_token,                │
        │                           │     refresh_token)           │
        │                           │                              │
        │                           │  10. Eraser reads id_token   │
        │                           │      knows: who you are,     │
        │                           │      your email, name,       │
        │                           │      profile picture         │
        │                           │                              │
        │  11. Eraser creates       │                              │
        │      your session         │                              │
        │      logs you in ✅       │                              │
        │ ◄─────────────────────────│                              │
        │                           │                              │
        │  12. You see              │                              │
        │      Eraser dashboard 🎉  │                              │
        │                           │                              │
```

---

## Key things to notice in this diagram

| Step | Who acts | What happens |
|------|----------|-------------|
| 1-2 | Browser ↔ Eraser | Eraser builds the Google URL |
| 3-6 | Browser ↔ Google | Login, consent, get code |
| 7 | Browser → Eraser | Code delivered to backend |
| 8-9 | Eraser ↔ Google | Secret server-to-server exchange |
| 10-12 | Eraser → Browser | Session created, you're logged in |

**Notice that in **steps 8 and 9** your browser is completely idle — it has no idea this server-to-server call is even happening. That's what makes this flow secure! 🔒**

---

<br>

# OAuth2 — Tokens Deep Dive

## 1. Tokens Google Returns

When Eraser's server exchanges the code with Google, Google sends back 3 tokens:

| Token | Purpose | Expires |
|-------|---------|---------|
| `id_token` | WHO you are — email, name, picture | 1 hour |
| `access_token` | Call Google APIs on your behalf | 1 hour |
| `refresh_token` | Get new access_token when expired | Never (until revoked) |

---

## i. id_token — Who You Are

This is a **JWT (JSON Web Token)**. It tells Eraser who just logged in.

A JWT has 3 parts separated by dots:

```
HEADER.PAYLOAD.SIGNATURE
```

### Header
```json
{
  "alg": "RS256",    ← algorithm used to sign
  "kid": "1e9gdk7"  ← which Google key was used
}
```

### Payload (the important part!)
```json
{
  "iss": "https://accounts.google.com",        ← issued by Google
  "sub": "114999553405419384753",              ← YOUR unique Google ID
  "aud": "941057276634-8r1m18n7...",           ← issued for Eraser
  "email": "rahul@gmail.com",                  ← your email
  "email_verified": true,                      ← Google confirmed this
  "name": "Rahul Badachi",                     ← your name
  "picture": "https://photo.google.com/photo", ← your profile pic
  "iat": 1700000000,                           ← issued at (timestamp)
  "exp": 1700003600                            ← expires at (1 hour later)
}
```

### Signature
```
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
Google's digital stamp — tamper-proof.
If even one character in the payload changes → signature fails.

---

## ii. access_token — Your API Key

Used by Eraser to call Google APIs **on your behalf**.

Example use cases:
- Read your Google Drive files
- Access your Google Calendar
- Send emails via Gmail

```
Eraser calls Google API:
GET https://www.googleapis.com/drive/v3/files
Authorization: Bearer ya29.xxxxx   ← access_token here
```

> Note: Eraser only calls the APIs you gave **consent** to during the consent screen.

---

## iii. refresh_token — The Long-Lived Key

The `access_token` expires in 1 hour. When it expires:

```
access_token expired ❌
        ↓
Eraser uses refresh_token to get a new one
        ↓
Server-to-server call to Google:
POST https://oauth2.googleapis.com/token
  grant_type=refresh_token
  refresh_token=1//xxxxx
  client_id=941057276634...
  client_secret=GOCSPX-xxxxx
        ↓
Google sends back a fresh access_token ✅
User never has to log in again!
```

---

## How Eraser Trusts the id_token

JWT uses **Asymmetric Encryption** — two mathematically linked keys:

| Key | Who has it | Purpose |
|-----|-----------|---------|
| Private Key | Google only, never shared | SIGNS the token |
| Public Key | Published openly by Google | VERIFIES the token |

Google's Public Keys are available at:
```
https://www.googleapis.com/oauth2/v3/certs
```

### Verification logic:
```
id_token received by Eraser:
┌─────────┬─────────┬───────────────┐
│ HEADER  │ PAYLOAD │   SIGNATURE   │
└─────────┴─────────┴───────────────┘
     │          │              │
     │          │              │
     ▼          ▼              │
Eraser takes                   │
Header + Payload               │
     +                         │
Google's Public Key            │
     │                         │
     ▼                         │
Generates NEW signature        │
     │                         │
     ▼                         ▼
  ┌──────────────────────────────┐
  │   new signature == original? │
  └──────────────────────────────┘
          │              │
          ▼              ▼
        YES ✅          NO ❌
      Trust it!       Reject it!
      Token is        Token is
      genuine         fake/tampered
```

### Why a hacker can't fake a token:
```
Hacker has Public Key  ✅
Hacker does NOT have Private Key ❌
        ↓
Cannot produce a valid signature
        ↓
Eraser rejects fake token ✅
```

---

## Complete Trust Chain

```
✅ Token received via secure server-to-server call
✅ Signature verified using Google's Public Key
✅ "iss" field = "https://accounts.google.com"
✅ "aud" field = Eraser's own client_id
= ERASER FULLY TRUSTS THIS TOKEN 🔒
```

---

## Real World Analogies

| Concept | Analogy |
|---------|---------|
| `id_token` | Your Aadhar card — proves who you are |
| `access_token` | Your office key card — lets you access things |
| `refresh_token` | Ability to get a new key card when it expires |
| Private Key | RBI's currency printing press — only RBI can print |
| Public Key | UV light — anyone can verify the note is real |

---

## Q&A

1. **Is the id_token encrypted?**
* No! It is only Base64 encoded. Anyone can decode and read the payload.
The SIGNATURE makes it tamper-proof, not encryption.

2. **Can Eraser modify the id_token payload?**
* No. If they change even one character, the signature verification fails
and Google rejects it.

3. **Why does access_token expire in 1 hour?**
* Security. If someone steals the access_token, it is only valid for 1 hour.
The refresh_token stays safe on the server and is never exposed.

4. **Can the refresh_token expire?**
* Yes, in these cases:
    - User manually revokes access from Google Account settings
    - User has not used the app for 6 months
    - App's refresh tokens exceed Google's limit
    - Google detects suspicious activity

5. **Why use asymmetric encryption for JWT?**
* So anyone can verify tokens using the Public Key without needing the Private Key. The Private Key never leaves Google — making it impossible
to fake tokens.

6. **What is the difference between id_token and access_token?**
* id_token = tells you WHO the user is (identity).
   access_token = lets you DO things on the user's behalf (authorization).