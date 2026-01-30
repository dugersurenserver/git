```cmd
netsh wlan add filter permission=allow ssid="lab.exe_WiFi7" networktype=infrastructure
netsh wlan add filter permission=denyall networktype=infrastructure
netsh wlan set profileparameter name="lab.exe_WiFi7" connectionmode=auto
netsh interface ip set dns name="Wi-Fi" static 8.8.8.8
netsh wlan show filters
netsh interface show interface
netsh interface ip set address name="Wi-Fi 2" static 10.10.80.201 255.255.255.0 10.10.80.1


```

```cmd
103.10.20.10
103.10.22.254
```



http://10.10.10.8:8888/

IT Olimpiad




## ОНОВЧТОЙ ШИЙДЭЛ (Windows 11 Home-д 100% ажиллана)
### Арга A — Wi-Fi Allow / Deny filter (CMD ашиглах)

Бусад Wi-Fi-г харагдахгүй болгоно, зөвхөн LAB.exe үлдэнэ

#### Яагаад энэ нь хамгийн сайн вэ?

* Group Policy шаардахгүй (Home edition OK)
* User-д Wi-Fi list харагдахгүй
* Restart хийсэн ч хадгалагдана
* Скрипт хэлбэрээр 30–50 PC дээр амархан хэрэгжинэ

## 1️⃣ LAB.exe Wi-Fi-г allow хийх

CMD-ийг Administrator эрхтэй нээнэ

```cmd
netsh wlan add filter permission=allow ssid="LAB.exe" networktype=infrastructure
```

## 2️⃣ БУСАД бүх Wi-Fi-г хориглох (харагдахгүй болгох)

```cmd
netsh wlan add filter permission=denyall networktype=infrastructure
```

📌 Үүний дараа:

* Зөвхөн LAB.exe харагдана
* Бусад SSID бүр мөсөн алга болно

## 3️⃣ LAB.exe-д автоматаар холбогдох (заавал)

```cmd
netsh wlan set profileparameter name="LAB.exe" connectionmode=auto
```

## 4️⃣ Шалгах команд

```cmd
netsh wlan show filters
```


Хариу нь ойролцоогоор ийм байна:

Allow List:
----------------
    LAB.exe

Deny All Networks
----------------
    Infrastructure

## 🟢 Үр дүн

|Шалгуур|Үр дүн|
|---|---|
|LAB.exe харагдах	|✅|
|Бусад Wi-Fiхарагдах	|❌|
|User өөрчлөх боломж	|❌|
|Restart-д тэсвэртэй	|✅|
|Windows 11 Home	|✅|

### ⚠️ ХЭРЭВ хэзээ нэгэн цагт цуцлах шаардлагатай бол

```cmd

netsh wlan delete filter permission=denyall networktype=infrastructure
netsh wlan delete filter permission=allow ssid="LAB.exe" networktype=infrastructure
```

### 🧩 НЭМЭЛТ (Лабораторийн орчинд илүү хатуу болгох)

✔ Wi-Fi profile устгах (хуучин хадгалагдсан бол)

```cmd
netsh wlan delete profile name=*
```

✔ LAN-аар Wi-Fi тохиргоо өөрчлөхийг хориглох

(Settings → Network → Advanced → Network reset-ийг admin-аас л хийх)

🔐 Илүү дээд түвшний хамгаалалт (Сүлжээний талаас)

Хэрэв Router / MikroTik-д хандалт байгаа бол:

MAC Address whitelist

LAB.exe-д WPA2-Enterprise (RADIUS)

VLAN тусгаарлалт (Exam VLAN)

хослуулбал 100% лабораторийн хяналт болно.

📌 Дүгнэлт

CMD → netsh wlan filter
* Windows 11 Home дээр
* Лабораторийн компьютерийг
* ганц Wi-Fi-д түгжих хамгийн оновчтой шийдэл

Хэрэв хүсвэл:

.bat файл бэлдэж өгье

Task Scheduler-т Logon дээр автоматаар ажиллахаар тохируулж өгье

30+ PC-д mass deployment хийх схем гаргаж өгье
