---
title: XAMPP及Joomla基本設定
date: 2018-04-11 10:56:31
categories:
- 課堂學習
tags:
- XAMPP
- Joomla
thumbnail: 
---

## XAMPP相關設定

* httpd.conf檔
    * 修改完此檔後需要重開Server
    * conf中註解為#
    * Listen後接要監聽的Port號
    * DocumentRoot後為設定Server的根目錄
    * <Directory "此處設定資料夾路徑"> </Directory>，標籤中輸入Options Indexes FollowSymLinks Include ExecCGI及AllowOverride All及Require all granted即可將該資料夾下所有路徑內容都開放給使用者查閱，若將Options Indexes FollowSymLinks Include ExecCGI(開發模式)，改為Options FollowSymLinks Include ExecCGI(上線模式)則不會顯示目錄內容，會改為顯示預設的403錯誤訊息
* .htaccess檔
    * XAMPP錯誤訊息頁面都要於此設定，其他類型架站軟體或站台都在httpd.conf檔設定即可
    * ErrorDocument 404 後接該錯誤訊息頁面的檔案路徑，其他錯誤訊息設定方式與此相同，修改錯誤訊息代號即可
* config.inc.php檔
    * 於phpMyAdmin中修改密碼後仍需於此檔案中設定
    * 認證方式(auth_type)有三種
        * config：預設為此，需搭配於user處輸入使用者名稱及password處輸入密碼
        * cookie：會從瀏覽器中抓取本網站的Cookie來讀取使用者名稱及密碼
        * http：每次讓使用者手動輸入使用者名稱及密碼
* php.ini檔
    * 修改完此檔後需要重開Server
    * Resource Limits
        * max_execution_time：可以設定單一腳本最大執行時間，建議使用240，單位為秒，盡量不超過300
    * File Uploads
        * upload_max_filesize：可以設定單一檔案上傳大小，建議10M
        * output_buffering：可以設定輸出緩衝，直接給數字即可，若要關閉則用Off
        * display_errors：可以設定是否顯示錯誤，開啟用On，關閉用Off

## Joomla相關設定

* 初次設定
    * 直接於瀏覽器網址輸入'localhost/joomla資料夾名稱'，接下來依照畫面步驟設定即可
    * 第一步設定使用者名稱及密碼是管理Joomla後台的，與phpMyAdmin的管理MySQL的不同，建議使用者名稱不要相同
    * 第二步才是輸入資料庫(MySQL)的使用者名稱及密碼
    * 後續範例建議使用部落格(在教學上內容較豐富，實際應用可視個人需求而定)
    * 建議設定處建議依照Joomla建議去修改php.ini檔
    * 若卡在安裝畫面無法成功安裝可能是php.ini檔的max_execution_time設定太短
* 資料夾說明
    * administrator：後台放置於此
    * components：大型元件放置於此
    * images：Joomla相關圖片放置於此
    * plugins：插件放置於此
    * templates：樣板放置於此
    * tmp：暫存放置於此
* 後台說明
    * Title Bar
        * System：系統
        * Users：使用者，可以註記使用者或寄信等
        * Menus：選單
        * Content：內容，最重要，因為Joomla是屬於內容管理系統
        * Components：大型元件
        * Extensions：擴充套件，次要重要，安裝各種東西都於此
        * Help
        * 網站名稱：可直接查看前台
    * 中文化
        * 點按Extensions>Language(s)>Installed>Install Languages>安裝zh-TW繁體中文
        * 返回將Extensions>Language(s)>Installed之下，將Default欄位的繁體中文列按下星星即可更改為繁體中文(注意上方選擇為Site是修改前台，選擇Administrator是修改後台)
    * 啟動樣式預覽
        * 點按擴充套件>佈景>樣式>選項>預覽模組位置>啟用中>儲存&關閉或儲存
        * 此時在擴充套件>佈景>樣式處即可點按樣式下的眼睛查看樣板的樣式並可發現其套用的定位名稱及擺放位置與樣式等設定