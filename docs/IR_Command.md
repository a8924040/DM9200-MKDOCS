#流程
```json
    下逹 command 至 Gateway 
    +--------+       +----------------------------+  
    |        |       | 更新 command JSON          |  
    | Mobile +-----> | update_command_encode.php  |  
    +--------+       +----------------------------+  
  
    Gateway 接收 comamnd :  
    Gateway先利用show_command_timestamp.php 取得command 更新時間,  
    若時間較新再利用show_command_encode.php 取得對Gateway所下逹命令,若時間就舊則不處理  
  
    +---------+   +----------------------------+           +---------+   +------------------------+  
    |         |   | 取得 command 下逹時間       | 時間較新   |         |   | 取得 command JSON      |  
    | Gateway |-->| show_command_timestamp.php |---------> | Gateway |-->| show_command_encode.php|  
    +---------+   +-------+--------------------+           +---------+   +------------------------+        
                                               |  時間較舊 +---------+  
                                               +---------> |  無動作 |  
                                                           | Gateway |  
                                                           +---------+  
```

## 學習KEY的步驟

* `update_command_encode.php`


```json
   {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "info": 
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":6,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":1}
                      }
         }
   }
```

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號
|cid          |               | gateway ID
|timestamp    | DM9200H       | 目前系統時間(UTC)
|devicetype   | DM9200H       | 目前只有DM9200H有IRM-201模組
|IRcommand    | NONE          | 6:進入學習模式，7:退出學習模式，8執行按鍵功能
|KeySelect    | NONE          | KEY選擇(0-65)
|DeviceSelect | NONE          | KEY DEVICE選擇(0-95)，每個DEVICE可以學習66個KEY
|handshake    | 1             | 交握旗標，用來判斷gateway是否否有正確執行IR COMMAND

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 已將COMMAND送到SERVER端 |


Gateway收到IR Command後會將執行結果透過update_command_encode.php讓client端知道

```json
    {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "command":"1",
   "timezone":"+8",
   "info":
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":6,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":2}
                      }
         }
   }

```
| handshake 所代表的意義  | Description  |
| --------------------    | -------------|
| ----------2------------ | 已進入學習模式，並已選擇要學習的KEY |
| ----------3------------ | 當被學習的遙控器按鍵放開 |
| ----------4------------ | 學習失敗 |
| ----------5------------ | 成功離開學習 |
| ----------6------------ | 成功執行按鍵 |

## 退出學習的步驟

* `update_command_encode.php`


```json
   {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "info": 
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":7,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":1}
                      }
         }
   }
```

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號
|cid          |               | gateway ID
|timestamp    | DM9200H       | 目前系統時間(UTC)
|devicetype   | DM9200H       | 目前只有DM9200H有IRM-201模組
|IRcommand    | NONE          | 6:進入學習模式，7:退出學習模式，8執行按鍵功能
|KeySelect    | NONE          | KEY選擇(0-65)
|DeviceSelect | NONE          | KEY DEVICE選擇(0-95)，每個DEVICE可以學習66個KEY
|handshake    | 1             | 交握旗標，用來判斷gateway是否否有正確執行IR COMMAND

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 已將COMMAND送到SERVER端 |


Gateway收到IR Command後會將執行結果透過update_command_encode.php讓client端知道

```json
    {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "command":"1",
   "timezone":"+8",
   "info":
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":7,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":5}
                      }
         }
   }

```
| handshake 所代表的意義  | Description  |
| --------------------    | -------------|
| ----------2------------ | 已進入學習模式，並已選擇要學習的KEY |
| ----------3------------ | 當被學習的遙控器按鍵放開 |
| ----------4------------ | 學習失敗 |
| ----------5------------ | 成功離開學習 |
| ----------6------------ | 成功執行按鍵 |

## 執行KEY的步驟

* `update_command_encode.php`


```json
   {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "info": 
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":8,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":1}
                      }
         }
   }
```

Required JSON Parameter

|Parameter    | Default       | Description
|------------ | ------------- | ------------
|userid       | NONE          | 使用者帳號
|cid          |               | gateway ID
|timestamp    | DM9200H       | 目前系統時間(UTC)
|devicetype   | DM9200H       | 目前只有DM9200H有IRM-201模組
|IRcommand    | NONE          | 6:進入學習模式，7:退出學習模式，8執行按鍵功能
|KeySelect    | NONE          | KEY選擇(0-65)
|DeviceSelect | NONE          | KEY DEVICE選擇(0-95)，每個DEVICE可以學習66個KEY
|handshake    | 1             | 交握旗標，用來判斷gateway是否否有正確執行IR COMMAND

Response

| Parameter    | Default       | Description  |
| ------------ | ------------- | ------------ |
| OK           | NONE          | 已將COMMAND送到SERVER端 |


Gateway收到IR Command後會將執行結果透過update_command_encode.php讓client端知道

```json
    {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "timestamp":"2016-09-20 20:01:29",
   "command":"1",
   "timezone":"+8",
   "info":
         {
         "devicetype":"DM9200H",
         "contentinfo":
                      {
                      "IRcommand":8,
                      "KeySelect":1,
                      "DeviceSelect":0,
                      "handshake":6}
                      }
         }
   }

```
| handshake 所代表的意義  | Description  |
| --------------------    | -------------|
| ----------2------------ | 已進入學習模式，並已選擇要學習的KEY |
| ----------3------------ | 當被學習的遙控器按鍵放開 |
| ----------4------------ | 學習失敗 |
| ----------5------------ | 成功離開學習 |
| ----------6------------ | 成功執行按鍵 |

## 儲存KEY資訊

* `slave_device_update.php` - 此API提供一個資料庫去儲存已設定過的KEY資訊

```json
   {
   "userid":"benny_wang@sunmoretek.com",
   "cid":"44334c4f92c4",
   "mac":"44334c4f92c4",
   "timestamp":"2016-11-09 10:00:00",
   "info":[
             {"DeviceIndex":"0","KeyIndex":"1","KeyName":"aaa"," timestamp","2016-11-09 10:00:00"},
             {"DeviceIndex":"0","KeyIndex":"2","KeyName":"bbb"," timestamp","2016-11-09 10:00:00"}
          ]
   } 

```








