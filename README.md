# SSRStatus

* SSRStatus是一個可以線上監控Shadowsocks/ShadowsocksR帳號的雲探針、雲監控探針~，該雲監控的網頁檔案基於ServerStatus（ https://github.com/ToyoDAdoubi/ServerStatus-Toyo/ ）項目。
* 線上示範：https://sstz.toyoo.ml/
* 我的部落格：https://doub.io/shell-jc5/

# 更新說明：

* 20170520: 修改網頁檔案窄屏/移動設備的顯示效果，修改JS刷新時間
* 20170519: 發布正式版本

# 安裝教學：     

執行下面的程式碼下載並執行腳本。
```Bash
wget -N --no-check-certificate https://raw.githubusercontent.com/david082321/doubi/master/ssrstatus.sh && chmod +x ssrstatus.sh && bash ssrstatus.sh

# 如果上面這個腳本無法下載，嘗試使用備用下載：
wget -N --no-check-certificate https://softs.fun/Bash/ssrstatus.sh && chmod +x ssrstatus.sh && bash ssrstatus.sh
```
下載並執行腳本後會出現腳本操作選單，選擇並輸入` 1 `就會開始安裝。

一開始會提示你輸入 網站伺服器的域名和埠，如果沒有域名可以直接回車代表使用` 本機IP:8888 `。

## 簡單步驟：

首先安裝服務端，安裝過程中會提示：

``` bash
是否由腳本自動設定HTTP服務(服務端的線上監控網站)[Y/n]

# 如果你不懂，那就直接回車，如果你想用其他的HTTP服務自己設定，那麼請輸入 n 並回車。
# 注意，當你曾經安裝過 服務端，同時沒有移除Caddy(HTTP服務)，那麼重新安裝服務端的時候，請輸入 n 並回車。
```

然後會提示你輸入網站伺服器的域名和埠，如果沒有域名可以直接回車代表使用` 本機IP:8888 `。

然後部署完 HTTP服務，就會讓你設定 檢測間隔時間。

``` bash
請選擇你要設置的ShadowsocksR帳號檢測時間間隔（如帳號很多，請不要設定時間間隔過小）
1. 5分鐘
2. 10分鐘
3. 20分鐘
4. 30分鐘
5. 40分鐘
6. 50分鐘
7. 1小時
8. 2小時
9. 自訂輸入

(預設: 2. 10分鐘):
```
我們還需要設定一下ShadowsocksR子目錄變數，打開腳本檔案

``` bash
vi ssrstatus.sh
# 按 I鍵 進入編輯模式，然後修改後按 ESC鍵 退出編輯模式，並輸入 :wq 儲存並退出
```
然後我們找到 第16行 的 `SSR_folder="/root/shadowsocksr/shadowsocks"` 參數，修改引號內的ShadowsocksR目錄名，必須設定為 ShadowsocksR子目錄的絕對路徑，並且最後一位不能加上 `"/"`。

注意：如果你用的是我的ShadowsocksR一鍵腳本，那麼位置即是：`/usr/local/shadowsocksr/shadowsocks`

最後 添加帳號設定即可。

# 使用說明：

進入下載腳本的目錄並執行腳本：

``` bash
# 管理選單
./ssrstatus.sh

# 檢測所有帳號設定（快捷參數）
./ssrstatus.sh t

# 檢測單獨帳號設定（快捷參數）
./ssrstatus.sh o

# 檢測自訂帳號設定（快捷參數）
./ssrstatus.sh a

# 查看日誌輸出（快捷參數）
./ssrstatus.sh log
```

然後選擇你要執行的選項即可。

``` bash
SSRStatus 一鍵安裝管理腳本 [vx.x.x]
-- Toyo | doub.io/shell-jc4 --

0. 升級腳本
————————————
1. 安裝 依賴及Web網頁
2. 移除 依賴及Web網頁
————————————
3. 測試 所有帳號
4. 測試 單獨帳號
5. 測試 自訂帳號
————————————
6. 設定 設定訊息
7. 查看 設定訊息
8. 查看 執行日誌
9. 設定 定時間隔
————————————

目前狀態: Web網頁 已安裝

請輸入數字 [0-9]:
```
## 其他操作

### Caddy（HTTP服務）：

* 啟動：service caddy start
* 停止：service caddy stop
* 重啟：service caddy restart
* 查看狀態：service caddy status
* Caddy設定檔案：/usr/local/caddy/Caddyfile

預設腳本只能一開始安裝的時候設定設定檔案，更多的Caddy使用方法，可以參考這些教學：https://doub.io/search/caddy

——————————————————————————————————————

* 網頁檔案：/usr/local/SSRStatus
* 設定檔案：ssr_status.conf（和腳本在同一個目錄中）
* 查看日誌：cat ssr_status.log（和腳本在同一個目錄中）

# 其他說明

## 修改網頁標題或公告

如果要修改網頁標題或者網頁頂部公告內容，打開 `/usr/local/SSRStatus/web/index.html` 檔案修改即可，很顯眼。

## 批次添加帳號設定

如果要批次添加帳號設定，那麼用腳本反而速度慢，可以按一下格式寫入設定檔案：

``` bash
ss/ssr連結###名稱###位置###禁用狀態

# 範例：
ssr://xxxxxxxx###DOUBI###Japen###fales

# fales代表禁用狀態否，即啟用，true 反之。
```

然後可以這樣快速寫入設定檔案：

``` bash
echo -e "ssr://xxxxxxxx###DOUBI1###Japen###fales
ssr://yyyyyyyy###DOUBI2###Hong Kong###true
ssr://zzzzzzzz###DOUBI3###洛杉磯(支援中文，只要你系統支援顯示和輸入)###fales" >> ssr_status.conf
```

# 相關開源項目： 

* ServerStatus：https://github.com/ToyoDAdoubi/ServerStatus-Toyo/
* ssr_check.sh: https://github.com/ToyoDAdoubi/doubi
