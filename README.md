ELK For Docker Compose
======================

目前執行的IP: 140.123.106.243

Proxmox
-------

### Update Proxmox
```bash
sudo apt-get update
sudo apt-get upgrade
```

### Git
```bash
sudo apt install git
git --version
```

### Docker
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt install docker-ce docker-ce-cli containerd.io
docker --version
```
https://hackmd.io/@sfagnU0PRimo51yNhxS4JQ/BybieZGuU

### docker-compose


Install Docker Compose
----------------------
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```


1. Install WSL2
1. Download and install [Docker](https://docs.docker.com/install/)
1. 在此路徑下執行
```bash
# Start Docker Compose
docker-compose up -d

# End Docker Compose
docker-compose down
```

Test Tcp And Http
-----------------
```bash
# Ubuntu
echo "log test" | nc localhost 5055

# Ubuntu
curl -X POST http://localhost:5056 -d "your message here" -H "Content-Type: text/plain"

# PowerShell
$headers = @{ "Content-Type" = "text/plain" }
Invoke-WebRequest -Uri "http://localhost:5056" -Method POST -Headers $headers -Body "your message here"

```

```shell
%{DATA:id},%{IPV4:ip},%{DATA:time},%{DATA:page},(%{DATA}-%{DATA:university}-%{DATA:department}.html|%{DATA:msg})。

# ZZ...
ZZ,%{IPV4:ip},%{DATA:time},%{DATA:page},%{GREEDYDATA:msg}。

# B1...
B1,%{IPV4:ip},%{DATA:time},%{DATA:page},%{NUMBER:year},%{DATA}：(%{DATA:Department1}-%{DATA:Department2}，共%{NUMBER:count}系組|%{DATA:msg})。。
```
常用的 Grok 模式筆記
-------------------
| 正則表達式     | 說明                                                       | 示例                       |
|----------------|------------------------------------------------------------|----------------------------|
| `+`            | 匹配一次或多次                                             | `a+` 會匹配 a、aa、aaa 等     |
| `{n}`          | 精確匹配 n 次                                              | `a{3}` 會匹配 aaa          |
| `{n,}`         | 匹配至少 n 次                                              | `a{3,}` 會匹配 aaa、aaaa 等  |
| `{n,m}`        | 匹配 n 到 m 次                                            | `a{3,5}` 會匹配 aaa、aaaa、aaaaa |
| `^`            | 匹配行的開頭                                               | `^abc` 會匹配 abc123        |
| `$`            | 匹配行的結尾                                               | `abc$` 會匹配 123abc        |
| `.`            | 匹配任意字符（除換行符）                                   | `a.b` 會匹配 axb、a1b 等     |
| `[]`           | 字符集，匹配集合中的任意字符                              | `[abc]` 會匹配 a、b 或 c    |
| `|`            | 邏輯 "或"，匹配左邊或右邊的任意一個模式                  | `abc|def` 會匹配 abc 或 def |
| `()`           | 捕獲分組，將模式分組並保存匹配的內容                      | `(abc)` 會匹配 abc 並捕獲為第一個分組 |
| `(?:...)`      | 非捕獲分組，將模式分組但不保存匹配的內容                 | `(?:abc|def)` 會匹配 abc 或 def，但不會捕獲它們 |
| `\`            | 轉義字符，使特殊字符失去特殊意義                          | `\\` 會匹配字面上的反斜線    |
| `(?=...)`      | 正向先行斷言，匹配某個位置後面跟隨某特定模式但不消耗字符  | `a(?=b)` 會匹配 a，且後面跟著 b |
| `(?!...)`      | 負向先行斷言，匹配某個位置後面不跟隨某特定模式            | `a(?!b)` 會匹配 a，但只有在 a 後面不跟著 b 時 |
| `\b`           | 單詞邊界，匹配字母、數字和非字母/數字字符之間的邊界      | `\bword\b` 會匹配獨立的 word |
| `\B`           | 非單詞邊界，匹配字母/數字之間或非字母/數字之間的地方     | `\Bword\B` 會匹配 sword 或 wording |


### 基本類型模式

| 模式             | 描述                            |
|------------------|---------------------------------|
| TIMESTAMP_ISO8601 | 用於解析 ISO 8601 格式的時間戳  |
| IPV4             | 用於解析 IPv4 地址              |
| USER             | 用於解析用戶名                  |
| NUMBER           | 用於解析數字                    |
| HOSTNAME         | 用於解析主機名稱                |
| GREEDYDATA       | 捕獲任意多字符直到行尾，通常用於捕獲長文本 |
| DATA             | 捕獲任意字符，通常用於處理非結構化數據 |

### 常用的日期和時間模式

| 模式        | 描述                            |
|-------------|---------------------------------|
| HTTPDATE    | 用於解析 HTTP 標準日期格式      |
| DATE        | 用於解析一般的日期格式          |
| TIME        | 用於解析時間                    |
| TIMESTAMP   | 用於解析一般的時間戳（不帶時區）|

### 其他常見的模式

| 模式      | 描述                               |
|-----------|------------------------------------|
| UUID      | 用於解析 UUID（通用唯一標識符）    |
| LOGLEVEL  | 用於解析日誌級別（如 INFO, WARN, ERROR 等） |
| WORD      | 用來解析單個單詞                   |
| MAC       | 用於解析 MAC 地址                  |
| CURRENCY  | 用於解析貨幣格式                   |

### 常用的 IP 地址和網絡模式

| 模式     | 描述                          |
|----------|-------------------------------|
| IPV4     | 用於解析 IPv4 地址             |
| IPV6     | 用於解析 IPv6 地址             |
| HOST     | 用於解析主機名或 IP 地址       |
