# UDP Command

* `command  0 ` - 列印出資料庫裡的DEVICE & LINK 表.
* `command  1 ` - 開啟 M02(Zigbee Module) JOIN 功能60秒.
* `command  2 ` - 綁定帳號.
* `command  3 ` - 註冊新帳號.
* `command  4 ` - 確認碼輸入.
* `command  5 ` - 解除綁定.
* `command  6 ` - 進入IRM-201(IR Module) 學習功能.
* `command  7 ` - 退出IRM-2-1(IR Module) 學習功能.
* `command  8 ` - IRM-201(IR Module) 控制協定.
* `command  9 ` - 啟動OTA功能.
* `command 10 ` - 掃描無線網路(WIFI SCAN).
* `command 11`  - 保留.
* `command 12`  - 查詢CID及IP.
* `command 13`  - 設定無線網路(WIFI SETTING).
* `command 14`  - 查詢CID.
* `command 15`  - 忘記密碼，進行密碼修改.
* `command 16`  - 輸入驗證碼及新的密碼.
* `command 17`  - 刪除ZIGBEE裝置.
* `command 18`  - 將WIFI設定為AP MODE.
* `command 19`  - 將WIFI設定為STA MODE.

   
## Command 1
    {"command":"1"}
## Command 2
    {
    "command":"2",
    "userid":"cynthiawang0418@gmail.com",
    "password":"12345678"
    }


Required JSON Parameter

Parameter    | Default       | Description
------------ | ------------- | ------------
userid       | NONE          | 使用者帳號
password     | NONE          | 使用者密碼

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| ok           | NONE              | 綁定成功     |
| no device    | NONE              | 找無此機器(已被其他帳號綁定)   |
| error        | NONE              | 密碼錯誤 或 帳號與密碼無法匹配 |

## Command 3
    {
    "command":"3",
    "userid":"cynthiawang0418@gmail.com",
    }


Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| ok           | NONE              | 執行成功 |
| user error   | NONE              | 帳號存在 |
| device error | NONE              | 機器己綁定 |

## Command 4
    {
    "command":"4",
    "userid":"cynthiawang0418@gmail.com",
    "check_code":"1234",
    "password":"12345678"
    }


Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號
|check_code   | NONE          | 當執行COMMAND 3 之後，SERVER會MIAL確認碼
|password     | NONE          | 輸入使用者所設定的密碼

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| ok           | NONE              | 執行成功 |
| user error   | NONE              | 帳號存在 |
| device error | NONE              | 機器己綁定 |
| check_code error |NONE           | 確認碼錯誤 |

## Command 5
    {
    "command":"5",
    "device_id":"44334c4f92c4"
    }


Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|device_id    | NONE          | gateway的序號

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| ok           | NONE              | 解除綁定成功 |
| user error   | NONE              | 此gateway 無綁定 |

## Command 6(測試用)
    {
    "command":"6",
    }

將IRM-201設定為學習模式


## Command 7(測試用)
    {
    "command":"7",
    }

將IRM-201退出學習模式


## Command 8
```         json     
     {
     "command":"8",
     "info":
           {
           "devicetype":"DM9200H",
           "contentinfo"
                        {
                         "IRcommand":8,
                         "KeySelect":1,
                         "DeviceSelect":0
                        }
            }
     }
```

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|devicetype   | DM9200H       | 目前只有DM9200H有IRM-201模組
|IRcommand    | NONE          | 6:進入學習模式，7:退出學習模式，8執行按鍵功能
|KeySelect    | NONE          | KEY選擇(0-65)
|DeviceSelect | NONE          | KEY DEVICE選擇(0-95)，每個DEVICE可以學習66個KEY

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| INTO LRN OK  | NONE          | 進入學習模式 |
| INTO LRN FAIL| NONE          | 進入學習模式失敗 |
| LRN OK       | NONE          | 學習按鍵/裝置(KEY & DEVICE)已設定成功 |
| LRN FAIL     | NONE          | 學習按鍵輸入失敗 |
| LRN KEY RELEASE           | NONE              | 偵測到學習按鍵已經放開 |
| EXIT LRN OK  | NONE              | 離開學習完成 |
| PRESS OK     | NONE              | 執行按鍵成功 |
| IR LRN TIMEOUT     | NONE              | 學習逾時，15秒內沒有接收到要學習的遙控器訊號 |


## Command 9
     {
     "command":"9",
     }

系統更新，當收到此COMMAND後，會將系統更新的旗標舉起，這時候系統就會連到更新SERVER去檢查是否有更新的資訊，如果有就會開始下載並將系統進行更新  
1.POST mac=CID & model=Hi3518-TEST 到OAT SERVER(http://pistory2013.com/android_APK/2014model/TEST/online.php)  
2.OTA SERVER會回傳 version & file path & md5_root & md5_uImg & md5_data  
3.去OAT SERVER提供的file path去下載rootfs_64k.jffs2 & uImage & data_64k.jffs2. & benny.sh & mtd_debug  
4.比對檔案MD5是否正確  
5.運行benny.sh EX:benny.sh datafs_64k.jffs2  

## Command 10
    {
    "command":"10",
    }

系統會去運行/mnt/data/WifiScan.cgi，並將SCAN的結果輸出到/mnt/data/wifi_info  

系統會以JSON格式返回wifi_info內容，如下
``` json
     {
     "APINFO":[  
              {"ESSID":"dlink-3A10","Encryption":"on","Quality":"100/100","EncryptMode0":"WPA","Group0":"TKIP","PairwiseCiphers00":"CCMP","PairwiseCiphers01":"TKIP","AuthenticationSuites0":"PSK","EncryptMode1":"WPA2","Group1":"TKIP","PairwiseCiphers10":"CCMP","PairwiseCiphers11":"TKIP","AuthenticationSuites1":"PSK"},  
              {"ESSID":"SUNNIC","Encryption":"on","Quality":"37/100 ","EncryptMode0":"WPA2","Group0":"CCMP","PairwiseCiphers00":"CCMP","PairwiseCiphers01":"","AuthenticationSuites0":"802.1x","EncryptMode1":"","Group1":"","PairwiseCiphers10":"","PairwiseCiphers11":"","AuthenticationSuites1":""}  
              ]  
     }
```


## Command 12
使用廣播方式去獲取同網域上DM9200的 CID & IP  

    {
    "command":"12",
    }

Response JSON format
``` json
    {
    "CID":"xxxxxxxxxxxx",
    "IP":"xxx.xxx.xxx.xxx"
    }
```

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| CID          | NONE          | gateway ID   |
| IP           | NONE          | IP Address   |


## Command 13
系統會將WIFI設為STA MODE並進行WIFI連線，為了CHECK系統有連到使用者指定網路，只用者必須將CLIENT端切換成同網段，然後下Command 12去確認gateway已連上USER指定AP，如果沒有在90秒內
確認，系統會自動切換回AP MODE  
``` json
    {
    "command":"13",
    "ESSID":"BuffaloBenny",
    "Encryption":"on",
    "EncryptMode0":"WPA",
    "Group0":"",
    "PairwiseCiphers00":"",
    "PairwiseCiphers01":"",
    "AuthenticationSuites0":"PSK",
    "EncryptMode1":"",
    "Group1":"",
    "PairwiseCiphers10":"",
    "PairwiseCiphers11":"",
    "AuthenticationSuites1":"",
    "WifiPassword":"1234567890"
    } 
```
Required JSON Parameter  
須將COMMAND 10 所獲的的WIFI INFO中所有參數傳進gateway，並多加欲連線AP的密碼  

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|WifiPassword | NONE          | AP 的密碼

系統會將WIFI設為STA MODE並進行WIFI連線，為了CHECK系統有連到使用者指定網路，只用者必須將CLIENT端切換成同網段，然後下Command 12去確認gateway已連上USER指定AP，如果沒有在90秒內確認，系統會自動切換回AP MODE  


## Command 14
在AP MODE可以透過此COMMAND去獲取gateway ID

    {
    "command":"14",
    }

Response JSON format
``` json
    {
    "CID":"xxxxxxxxxxxx",
    }
```

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| CID          | NONE          | gateway ID   |



## Command 15
忘記使用者密碼時可以透過此COMMAND去更換密碼

    {
    "command":"15",
    "userid":"cynthiawang0418@gmail.com"
    }

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號

Response 

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 執行成功 |
| user error   | NONE          | 帳號不存在 |


## Command 16
當使用COMMAND 15後，系統會發送驗證碼到使用者註冊的MAIL，再透過此COMMAND去修改使用者密碼

    {
    "command":"16",
    "userid":"cynthiawang0418@gmail.com",
    "check_code":"xxxx",
    "change_password":"xxxxxxxx"
    }

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號
|check_code   | NONE          | 確認碼
|change_password  | NONE      | 設定新的密碼

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 執行成功 |
| user error   | NONE          | 帳號不存在 |
| check_code error   | NONE    | 確認碼錯誤 |
| session error   | NONE       | 網路SESSION錯誤|

## Command 17

    {
    "command":"17",
    "DeviceNo":"1"
    }

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|DeviceNo     | NONE          | 每個ZIGBEE裝置都會配發一個Number，可以透過此COMMAND去刪除改裝置

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 執行成功 |
| error        | NONE          | 沒有該裝置號碼可以刪除 |

## Command 18(測試用)
運行/mnt/data/wifi_script_ap ， 將系統切換成AP MODE  

    {
    "command":"18"
    }

## Command 19(測試用)
運行/mnt/data/wifi_script_ ， 將系統切換成STA MODE  

    {
    "command":"19"
    }


