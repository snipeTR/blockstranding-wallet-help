## 🔁 BlockStranding Wallet Restore Guide (Private Key → Base64 → Browser)

### 🎯 Scenario:

You have a **Solana Private Key in Base58 format**. This key was either previously generated for you by the `blockstranding.com` website or obtained from somewhere else.
Your goal is to convert this private key into the format used by BlockStranding and **inject it back into your browser**, so you can resume the game with the same wallet.

---

### ✅ 1. Convert Base58 Private Key to Base64 Format

#### 🧰 What You Need:

* A computer
* A web browser (Chrome, Edge, Brave, etc.)
* A place to run Python code:

  * 🔗 [https://www.programiz.com/python-programming/online-compiler](https://www.programiz.com/python-programming/online-compiler)
  * 🔗 [Direct code link](https://www.programiz.com/online-compiler/9apvKWZzpe5HO)

#### 📜 Steps:

1. Copy and paste the code below into the Python editor.
2. Replace the value of the `base58_input` variable with **your Base58 Private Key** (in quotes).
3. Run the code and copy the resulting **Base64 string**.

---

### 🔢 Python Code (Base58 → Base64 Conversion)

```python
import base64

BASE58_ALPHABET = '123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz'

def base58_decode(base58_str):
    num = 0
    for char in base58_str:
        num *= 58
        if char not in BASE58_ALPHABET:
            raise ValueError(f"Invalid character: {char}")
        num += BASE58_ALPHABET.index(char)

    # Add padding for leading '1's (which represent 0x00 bytes)
    n_pad = 0
    for char in base58_str:
        if char == '1':
            n_pad += 1
        else:
            break

    byte_data = num.to_bytes((num.bit_length() + 7) // 8, 'big')
    return b'\x00' * n_pad + byte_data

def base58_to_base64(base58_str):
    byte_data = base58_decode(base58_str)
    return base64.b64encode(byte_data).decode('utf-8')

if __name__ == "__main__":
    # Paste your Base58 private key here (inside double quotes)
    base58_input = "21YVu4RdZcbXJQ5wh4VF2FGgy6fanMTVHoi86FpPsTqqEo9FyZQcE3z9hchQe6GvoHhE3kJCyAzDrWiSdgwet3dh"

    base64_output = base58_to_base64(base58_input)
    print("Base64 output for WALLET_KEYPAIR:")
    print(base64_output)
```

> ✅ Example output:

```
Base64 output for WALLET_KEYPAIR:
Mnw+MEzzDdLS58xTidj5enyCObZn0n5+L1cQwJOAiPpUP0hl6y+2PrbwCpRwILcG6Gi9fv3uAuG2NYOxwHI4YA==
```

---

### ✅ 2. Import the Base64 Value into the Browser (LocalStorage)

#### 🧰 What You Need:

* A computer and a Chrome/Edge browser

#### 🔧 Steps:

1. Open the site `https://blockstranding.com/` in your browser.
2. Press **F12** on your keyboard to open Developer Tools.
3. Go to the **Application** tab at the top.
4. In the left-hand menu, follow this path:

   ```
   Application > Storage > Local Storage > https://blockstranding.com
   ```
5. Right-click in the table on the right → choose **Add item**, or locate the existing `WALLET_KEYPAIR` entry and **double-click it**.
6. If the `WALLET_KEYPAIR` entry exists, **replace its value** with your new Base64 string.
7. **Refresh the page** (F5).

---

### 🚀 Result

After refreshing the site, your previous wallet will be restored, and the game will continue using the same wallet as before.

---

### ⚠️ Warnings

* This method is intended **only for restoring your own wallet**.
* Entering someone else’s private key is unethical and illegal.
* If your browser's LocalStorage is cleared or cookies are deleted, you will need to import the key again.
* It's a good idea to back up any existing value before making changes.
