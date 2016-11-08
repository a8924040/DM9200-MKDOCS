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

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
