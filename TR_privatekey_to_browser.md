## 🔁 BlockStranding Cüzdanı Geri Yükleme Rehberi (Private Key → Base64 → Browser)

### 🎯 Senaryo:

Elinizde **Base58 formatında bir Solana Private Key** var. Bu anahtar, daha önce `blockstranding.com` sitesi üzerinde size atanmıştı veya başka bir yerden aldınız.
Amacınız bu private key’i, BlockStranding sitesinin kullandığı formata dönüştürüp **browser'a geri yazmak** ve oyunda kaldığınız yerden devam etmek.

---

### ✅ 1. Base58 Private Key → Base64 Formatına Dönüştürme

#### 🧰 Gerekli Araçlar:

* Bilgisayar
* Tarayıcı (Chrome, Edge, Brave vs.)
* Aşağıdaki Python kodunu çalıştırabileceğiniz bir ortam:

  * 🔗 [https://www.programiz.com/python-programming/online-compiler](https://www.programiz.com/python-programming/online-compiler)
  * 🔗 [Direkt kod link'i](https://www.programiz.com/online-compiler/9apvKWZzpe5HO)

#### 📜 Adımlar:

1. Python çalışma ortamına aşağıdaki kodu **kopyalayıp yapıştırın**.
2. `base58_input` değişkenine **elinizdeki Base58 Private Key’i** yazın.
3. Kodu çalıştırın ve çıkan **Base64 string’i kopyalayın**.

---

### 🔢 Python Kodu (Base58 → Base64 Dönüşüm)

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
    # Buraya elinizdeki Base58 Private Key’i yazın (çift tırnak içinde)
    base58_input = "21YVu4RdZcbXJQ5wh4VF2FGgy6fanMTVHoi86FpPsTqqEo9FyZQcE3z9hchQe6GvoHhE3kJCyAzDrWiSdgwet3dh"

    base64_output = base58_to_base64(base58_input)
    print("WALLET_KEYPAIR için Base64 çıktısı:")
    print(base64_output)
```

> ✅ Çıktı örneği:

```
WALLET_KEYPAIR için Base64 çıktısı:
Mnw+MEzzDdLS58xTidj5enyCObZn0n5+L1cQwJOAiPpUP0hl6y+2PrbwCpRwILcG6Gi9fv3uAuG2NYOxwHI4YA==
```

---

### ✅ 2. Base64 Veriyi Browser'a Import Etme (LocalStorage)

#### 🧰 Gerekenler:

* Bilgisayar ve Chrome/Edge tarayıcı

#### 🔧 Adımlar:

1. `https://blockstranding.com/` sitesini tarayıcıda açın.
2. Klavyenizden **F12** tuşuna basarak geliştirici araçlarını açın.
3. Üst sekmeden **Application** kısmına geçin.
4. Sol menüde şu yolu takip edin:

   ```
   Application > Storage > Local Storage > https://blockstranding.com
   ```
5. Sağdaki tabloya sağ tıklayın → **Add item** veya var olan `WALLET_KEYPAIR` girdisini bulup **çift tıklayın**.
6. `WALLET_KEYPAIR` anahtarı varsa, değerini yeni **Base64 çıktısı ile değiştirin**.
7. Sayfayı **yenileyin** (F5).

---

### 🚀 Sonuç

Site yenilendikten sonra, artık eski cüzdanınız geri yüklenmiş olacak ve oyun aynı cüzdanla kaldığı yerden devam edecektir.

---

### ⚠️ Uyarılar

* Bu işlem sadece **kendi cüzdanınızı** kurtarmak içindir.
* Başkasının cüzdan anahtarını girmeniz hem etik dışı hem de yasa dışıdır.
* LocalStorage temizlenirse veya tarayıcı çerezleri silinirse tekrar import yapmanız gerekir.
* Değeri değiştirmeden önce eski veriyi yedeklemeniz önerilir.
