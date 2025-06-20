## 🔐 BlockStranding Private Key Kurtarma Rehberi

### 🎯 Senaryo:

BlockStranding sitesine giriş yaptınız, oyun size otomatik bir **Solana cüzdanı (wallet)** oluşturdu. Fakat private key (özel anahtar) size gösterilmeden tarayıcıda saklandı ve siz onu kaybettiniz.

Siteye girip oyun oynayabiliyorsunuz, fakat private key olmadan başka bir yerde bu cüzdanı kullanamıyorsunuz. O yüzden bu bilgiyi tarayıcıdan **geri almanız** gerekiyor.

---

### ✅ 1. Tarayıcıdan Cüzdan Bilgisini Kurtarın

#### 🧰 Gerekenler:

* Bilgisayar
* Chrome, Edge veya Brave gibi bir tarayıcı

#### 🔎 Adımlar:

1. Web tarayıcınızda `https://blockstranding.com/` sitesini açın.
2. Klavyenizden **F12** tuşuna basarak geliştirici araçlarını açın.
3. Üst sekmelerden **Application** sekmesine geçin.
4. Sol menüden şu yolu izleyin:

   ```
   Application > Storage > Local Storage > https://blockstranding.com
   ```
5. Sağdaki listeye göz atın. `WALLET_KEYPAIR` isminde bir anahtar göreceksiniz.
6. Bu değerin tamamını **kopyalayın** (örneğin: `Mnw+MEzzDdLS58x...` gibi bir Base64 formatında uzun bir string).

---

### 🧪 2. Bu Bilgiyi Base58 Formatına Çevirin

#### 🌐 Online Dönüştürücü:

Aşağıdaki Python kodunu çalıştırabileceğiniz bir Python çalıştırma sitesi öneriyorum:

🔗 [https://www.programiz.com/python-programming/online-compiler](https://www.programiz.com/python-programming/online-compiler)
🔗 [Direkt Kodun link'i](https://www.programiz.com/online-compiler/1c2ZM7Fv9gtBU)

#### 📜 Yapmanız Gerekenler:

1. Yukarıdaki linke tıklayın ve siteyi açın.
2. Aşağıdaki kodun tamamını oraya **yapıştırın**.
3. Kodun içinde 32. satırdaki `base64_input = "..."` kısmına kendi `WALLET_KEYPAIR` değerini yazın (çift tırnak içinde).
4. Ardından **RUN** butonuna tıklayın.
5. Sağdaki konsolda Base58 formatında **Solana Private Key** çıktısını göreceksiniz.

---

### 🔢 Python Kodu (Yapıştırın ve Düzenleyin)

```python
import base64

# Base58 karakter alfabesi (Bitcoin standardı)
BASE58_ALPHABET = '123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz'

def base58_encode(byte_data):
    """bytes → Base58 string"""
    num = int.from_bytes(byte_data, 'big')
    encode = ''
    while num > 0:
        num, rem = divmod(num, 58)
        encode = BASE58_ALPHABET[rem] + encode

    # Baştaki sıfır byte'ları '1' karakterine çevir
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
    # Buraya kendi WALLET_KEYPAIR değerinizi girin (çift tırnak içinde)
    base64_input = "Mnw+MEzzDdLS58xTidj5enyCObZn0n5+L1cQwJOAiPpUP0hl6y+2PrbwCpRwILcG6Gi9fv3uAuG2NYOxwHI4YA=="

    base58_output = base64_to_base58(base64_input)
    print("Base58 çıktısı:")
    print(base58_output)
```

---

### ✅ Örnek Çıktı

Eğer örnek veriyi kullandıysanız çıktı şu şekilde olabilir:

```
Base58 çıktısı:
21YVu4RdZcbXJQ5wh4VF2FGgy6fanMTVHoi86FpPsTqqEo9FyZQcE3z9hchQe6GvoHhE3kJCyAzDrWiSdgwet3dh
```

Bu, sizin **Solana Private Key**’inizdir ve artık bu anahtarı kullanarak aynı cüzdanı başka bir Solana cüzdan uygulamasında (örneğin Phantom, Solflare, Sollet) **import (içe aktarım)** yapabilirsiniz.

---

### 📌 Uyarılar

* Bu işlem sadece kendi tarayıcınızda oluşturulmuş ve hala silinmemiş bir cüzdan için geçerlidir.
* Bilgilerinizi başka kişilerle **asla paylaşmayın**. Private key’e sahip olan biri cüzdanınızı boşaltabilir.
* Bu yöntem yalnızca **kendi hesabınız** için kullanılmalı, başkasının cihazında etik olmayan bir şekilde kullanılmamalıdır.
