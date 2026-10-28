# mustafakman711 Seelen UI kurulumu (v2.8.8)

macOS 26 "Tahoe" tarzında cam (glass) dock ve şeffaf menü çubuğu temasıyla birlikte ayarlar.

| Dosya | Hedef |
| --- | --- |
| `macos.yml` | `%APPDATA%\com.seelen.seelen-ui\themes\macos.yml` |
| `settings.json` | `%APPDATA%\com.seelen.seelen-ui\settings.json` |

## Kurulum (PowerShell)

```powershell
$dst = "$env:APPDATA\com.seelen.seelen-ui"
Copy-Item "$dst\settings.json" "$dst\settings.json.bak" -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force "$dst\themes" | Out-Null
Copy-Item .\macos.yml "$dst\themes\macos.yml" -Force
Copy-Item .\settings.json "$dst\settings.json" -Force

# Seelen UI'yi yeniden başlat
Stop-Process -Name seelen-ui -Force -ErrorAction SilentlyContinue
$pkg = Get-AppxPackage Seelen.SeelenUI
Start-Process "shell:AppsFolder\$($pkg.PackageFamilyName)!App"
```

Tema kimliği `@mustafakman711/macos`. `settings.json` içinde `activeThemes` listesinde
`@default/theme`'den **sonra** gelmeli (temalar bu sırayla CSS katmanı olarak uygulanıyor).

## Notlar

- Temadaki `styles:` blokları ham CSS olarak okunur (SCSS derlenmez).
- backdrop-filter, sonsuz ya da scroll'a bağlı animasyon yok; sadece sabit gradyan ve gölge kullanılıyor.
- Dock `hideMode: Never`: dock çalışma alanını ayırır ve pencereler dock'un altına girmez.
- Bilinen sorun (v2.8.8): animasyonlu tepsi simgeleri (ör. senkronize olurken Google Drive)
  Seelen'de "Mutex lock timed out" panic'lerine ve widget'ların yeniden yüklenmesine yol açabiliyor.
  Upstream v2.8.10'da düzeltildi (eythaann/Seelen-UI#1907).
