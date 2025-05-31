## Windows'ta NASM ile Assembly Yazmak: Lain Evrenine Adım Atmak :3

> *"Connecting to the Wired... initializing build protocols..."*

Eğer *Serial Experiments Lain*'deki gibi makinelerle daha "doğrudan" bir iletişim kurmak istiyorsan, NASM ile Assembly yazmak sana göre olabilir :3 Bu rehberi, kendi deneyimimden yola çıkarak hazırladım. Amacım başkalarının da kolayca başlayabilmesini sağlamak. Wired'a giden bu yolda elimden geldiğince yardımcı olmak istiyorum.

---

### 🧱 Gerekli Araçları Kurarak Başlayalım

#### 1. **Chocolatey yoksa önce onu kur**

Windows'ta eksiksiz bir terminal deneyimi için Chocolatey gerekiyor. PowerShell'i yönetici olarak açıp şunu yap:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; \
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; \
iwr https://community.chocolatey.org/install.ps1 -UseBasicParsing | iex
```

#### 2. **Scoop'u da kuralım**

Aynı şekilde, `scoop` da çok işine yarayacak. PowerShell'e şunu yaz:

```powershell
iwr -useb get.scoop.sh | iex
```

---

### ⚙️ NASM ve MinGW Kurulumu

```bash
choco install nasm
scoop install mingw
```

* `nasm`: Assembly kodlarını objeye çevirmenizi sağlar.
* `mingw`: Objeyi `.exe` haline getirmek için GCC içerir.

---

### 🧩 VSCode Eklentisi (İsteğe Bağlı Ama Tatlı)

Kod yazarken renkli bir görünüm istiyorsan bu eklentiyi kur:

🔗 [dan-c-underwood.arm](https://marketplace.visualstudio.com/items?itemName=dan-c-underwood.arm)

Bu eklenti sayesinde VSCode, `.asm` dosyalarını daha okunabilir hale getiriyor.

---

### 🗃️ Proje ve Task Ayarları

#### 1. Proje klasörü oluştur:

```bash
mkdir lain-nasm
cd lain-nasm
```

#### 2. `.vscode` klasörü oluştur:

```bash
mkdir .vscode
```

#### 3. `tasks.json` dosyasını ekle:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build All",
      "type": "shell",
      "dependsOn": ["Assemble", "Link"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "Assemble",
      "type": "shell",
      "command": "nasm",
      "args": ["-f", "win64", "main.asm", "-o", "main.obj"]
    },
    {
      "label": "Link",
      "type": "shell",
      "command": "gcc",
      "args": ["main.obj", "-o", "main.exe"]
    }
  ]
}
```

#### 4. `launch.json` dosyasını da oluştur:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "lldb",
      "request": "launch",
      "name": "Debug",
      "program": "${workspaceFolder}/main.exe",
      "args": [],
      "cwd": "${workspaceFolder}"
    }
  ]
}
```

---

### 🚀 Kodunu Çalıştırmak

1. `main.asm` dosyanı proje kök dizinine koy.
2. `Ctrl + Shift + B` ile build et (derlenip .exe oluşur).
3. `F5` ile çalıştır veya debug et.

---

### 💡 Son Söz

Eğer *Wired*’a gerçekten bağlanmak istiyorsan, aşağı seviyeye inmek şart. Bu yol kolay değil ama çok öğretici.

Umarım bu rehber senin için faydalı olmuştur. Yardımcı olabildiysem ne mutlu bana. Soruların olursa çekinmeden yaz.

> *"Because in the Wired, we are all connected..."*

**– Ferivonus**
