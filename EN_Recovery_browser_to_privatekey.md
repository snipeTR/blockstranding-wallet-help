## 🔐 BlockStranding Private Key Recovery Guide

### 🎯 Scenario:

You logged into the BlockStranding website, and the game automatically created a **Solana wallet** for you. However, the private key was stored in your browser without being shown to you, and you lost access to it.

You can still access the site and play the game, but without the private key, you cannot use the wallet elsewhere. That’s why you need to **recover this information** from your browser.

---

### ✅ 1. Recover Wallet Info from Your Browser

#### 🧰 Requirements:

* A computer
* A browser like Chrome, Edge, or Brave

#### 🔎 Steps:

1. Open the website `https://blockstranding.com/` in your browser.
2. Press **F12** on your keyboard to open the Developer Tools.
3. Click on the **Application** tab in the top menu.
4. In the left-hand menu, navigate to:

   ```
   Application > Storage > Local Storage > https://blockstranding.com
   ```
5. Look at the list on the right. You should see an entry named `WALLET_KEYPAIR`.
6. **Copy** the full value of this entry (for example: `Mnw+MEzzDdLS58x...`, a long Base64-encoded string).

---

### 🧪 2. Convert the Data to Base58 Format

#### 🌐 Online Converter:

You can use an online Python compiler to run the code below. We recommend:

🔗 [https://www.programiz.com/python-programming/online-compiler](https://www.programiz.com/python-programming/online-compiler)
🔗 [Direct link to the code](https://www.programiz.com/online-compiler/1c2ZM7Fv9gtBU)

#### 📜 What to Do:

1. Click the link above to open the compiler.
2. Paste the **entire code** below into the editor.
3. In the code, find line 32: `base64_input = "..."`. Replace the content inside the quotes with your own `WALLET_KEYPAIR` value.
4. Click the **RUN** button.
5. You’ll see the output on the right panel, displaying your **Solana Private Key** in Base58 format.

---

### 🔢 Python Code (Paste and Edit)

```python
import base64

# Base58 character set (Bitcoin standard)
BASE58_ALPHABET = '123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz'

def base58_encode(byte_data):
    """bytes → Base58 string"""
    num = int.from_bytes(byte_data, 'big')
    encode = ''
    while num > 0:
        num, rem = divmod(num, 58)
        encode = BASE58_ALPHABET[rem] + encode

    # Convert leading zero bytes to '1'
    n_pad = 0
    for b in byte_data:
        if b == 0:
            n_pad += 1
        else:
            break
    return '1' * n_pad + encode

def base64_to_base58(base64_str):
    """Base64 encoded string → Base58 encoded string"""
    byte_data = base64.b64decode(base64_str)
    return base58_encode(byte_data)

if __name__ == "__main__":
    # Insert your WALLET_KEYPAIR here (inside double quotes)
    base64_input = "Mnw+MEzzDdLS58xTidj5enyCObZn0n5+L1cQwJOAiPpUP0hl6y+2PrbwCpRwILcG6Gi9fv3uAuG2NYOxwHI4YA=="

    base58_output = base64_to_base58(base64_input)
    print("Base58 output:")
    print(base58_output)
```

---

### ✅ Sample Output

If you use the sample input above, your result might look like this:

```
Base58 output:
21YVu4RdZcbXJQ5wh4VF2FGgy6fanMTVHoi86FpPsTqqEo9FyZQcE3z9hchQe6GvoHhE3kJCyAzDrWiSdgwet3dh
```

This is your **Solana Private Key** in Base58 format. You can now **import** this key into any Solana wallet application (such as Phantom, Solflare, or Sollet) to access the same wallet.

---

### 📌 Warnings

* This method only works if the wallet was created on **your current browser** and hasn't been cleared.
* **Never share** this information with others. Anyone with your private key can take control of your wallet.
* Use this recovery method **only for your own account**. Do not use it unethically on someone else’s device.
