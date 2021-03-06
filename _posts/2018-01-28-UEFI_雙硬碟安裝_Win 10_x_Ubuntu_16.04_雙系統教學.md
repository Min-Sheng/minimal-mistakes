---
title: UEFI 雙硬碟安裝 Win 10 x Ubuntu 16.04 雙系統教學
date: 2018-1-28 13:21:00
excerpt: 終於灌好 Windows 和 Linux 的雙系統啦，高興之餘來分享經驗吧！
header:
  image: https://3.bp.blogspot.com/-PunkM3-PRDQ/Wm3nG3rKz8I/AAAAAAAAG4U/7tEAtApKwYwX4ex-7vf5nWeYjaOYIddcQCKgBGAs/s1600/2018-01-27_235109.png
  teaser: https://3.bp.blogspot.com/-zi4JvLxaDZU/Wm3nG6eiNEI/AAAAAAAAG4U/r4QJFQ8nMJwlArPQl34vhzZJAjxd1dlnQCKgBGAs/s1600/ubuntu-16.04-xenial-xerus.png
comments: true
keywords: 
  - 'teaching'
  - 'IT'
category: teaching
tags: [teaching]
---

{% include toc title = "目錄" %}

> 終於灌好 Windows 和 Linux 的雙系統啦，高興之餘來分享經驗吧！

先前我也曾經嘗試要灌 Windows 和 Linux 的雙系統，想說網路上的教學一堆，應該很容易，結果沒想到到處都是坑，差點把電腦搞成磚，結果是我自己沒靜下心來查看自己的電腦系統與那些教學的差異。這次多多查詢相關知識並仔細來做，發現其實沒啥阻礙且很快就能完工。有了 Linux ，往後在架設伺服器或者使用Python或機器學習相關套件將會如虎添翼。

# 環境 

我的電腦內已有 Win 10 安裝於 SSD（C槽），本教學只會說明如何將 HDD（D槽）分出一些空間（64 GB）安裝 Ubuntu 16.04　。

# 預備動作

## 1. 備份你的檔案

雖然這麼做比較保險，但是我想說既然 Ubuntu 是要灌在 D槽而 Win 10 在 C槽，兩顆不同磁碟只要不手殘點錯，應該沒問題，所以就偷懶沒事先備份，此舉好孩子不該學喔。

## 2. 先確定自己電腦的開機方式是否為 UEFI

按 Win + R 鍵以開啟 “ 執行 ” ，輸入 msinfo32 並 ENTER ，查看系統資訊。在 BIOS 模式中如果顯示 “ 傳統 ” ，表示系統啟動方式為 Legacy BIOS ；如果顯示 UEFI ，則為 UEFI 。

<figure>
    <img src="https://3.bp.blogspot.com/-J3WmMx9HUo0/Wm3ndzGSmnI/AAAAAAAAG4c/C2gUY0pxahYUjwguKzdXrv8Iq20z8c1sgCKgBGAs/s1600/2018-01-27_230011.png" height="25%" width="25%">
</figure>

## 3. 查看 HDD 的磁碟分區格式，必須為 GPT

在 “ 開始功能表 ” （螢幕左下方的 windows 按鈕）點擊滑鼠右鍵，選擇 “ 磁碟管理 ” 開啟磁碟管理工具，於 HDD 的磁碟代號上（我的是磁碟1）點擊滑鼠右鍵，選單中若有灰色的 “ 轉換成MBR磁碟 ” 則表示你的磁碟目前是 GPT 格式。

<figure>
    <img src="https://4.bp.blogspot.com/-yPk3NmsvZPQ/Wm3nmSwcNGI/AAAAAAAAG4g/40md4qeSUR0uFk8c41aQztmPYYcWbOldwCKgBGAs/s1600/Sequence%2B01.00_00_01_17.Still001.png" height="25%" width="25%">
</figure>

## 4. 在 HDD 的磁區分割出 64 GB 的空間（在附圖中我已經分出來了，沒意外的話你的 HDD 應該是完整的一個磁區）

在 HDD 的磁區點擊滑鼠右鍵，選擇壓縮磁區，在 “ 輸入要壓縮的空間大小 ” 填入 65536 (MB)，並靜待電腦執行壓縮，壓縮完成後會看到空出來的 64 GB。

<figure>
    <img src="https://3.bp.blogspot.com/-3GQ8mYeLSUY/Wm3nuyF3f0I/AAAAAAAAG4k/8l0CxD7DdHQ8tzEgbVN9Nn6t4JyN3txTQCKgBGAs/s1600/2018-01-27_232556.png" height="25%" width="25%">
</figure>

## 5. 關閉 Windows 的快速啟動

開啟 “ 控制台 ” >> “ 硬體和音效 ” >> “ 電源選項 ” >> “ 選擇按下電源按鈕時的行為 ” >> “ 變更目前無法使用的設定 ” >> 在 “ 開啟快速啟動 (建議選項) ” 取消勾選。

## 6. 進入 BIOS 設置關閉 Security Boot ，否則 Ubuntu 無法寫入引導程序

每一家公司的主機板設定 BIOS 的方式不盡相同，所以請針對自己的主機板自行 google 如何關閉。

## 7. 製作 Ubuntu 16.04 USB 開機碟

我使用 Rufus 這款免費、免安裝、簡單又快速的 ISO 光碟映像檔製作軟體：

(1) 準備一個「空的」 USB 隨身碟。
    
(2) 首先連到 [Ubuntu 的官方網站]( https://www.ubuntu-tw.org/modules/tinyd0/)，可依自己需要選擇適合的版本，在此我選擇發行板為 “ Ubuntu桌面版本 ” ，版本為 “ Ubuntu 16.04 LTS ” （最新長期支援（穩定）版），電腦架構為 “ 64 位元版本 ” ，並點選 “ 開始下載 ” 。

<figure>
    <img src="https://4.bp.blogspot.com/-XN3FzHuzjmc/Wm3oCYaTF6I/AAAAAAAAG4s/Tx9uRtmDQrMZWsbTKtpO77NPgy-62zRWgCKgBGAs/s1600/2018-01-28_125420.png" height="25%" width="25%">
</figure>

(3) 連到 [Rufus 的官方網站]( https://rufus.akeo.ie/?locale=zh_TW)，下向捲動找到下載的 “ Rufus 2.18 (945 KB) ” 或 “ Rufus 2.18 可攜版 (945 KB) ” ，點選下載。

<figure>
    <img src="https://2.bp.blogspot.com/-kgDHMJTXX7o/Wm3n6zDWZII/AAAAAAAAG4o/D7U3ER6xbMAv0og1lOef6phumXX8LTItACKgBGAs/s1600/2018-01-28_125949.png" height="25%" width="25%">
</figure>
    
(4) 下載完成後，將你的 USB 隨身碟插入電腦的 USB 插槽，並對著下載的檔案雙擊滑鼠左鍵，執行 Rufus 。
    
(5) 開始製作：

a. 在 “ 裝置 ” 選擇 “ 要製作的USB隨身碟位置 ” 。

b. “ 資料分割配置及系統類型 ” 使用預設的 “ MBR可相容BIOS和UEFI-CSM之資料分割 ” 。

c. 點選左下方的光碟機圖案以 “ 選取映像檔 ” ，選擇 “ 先前下載好的 Ubuntu 16.04 ISO 開機光碟映像檔 ” ，點選 “ 開啟 ” 。

d. 點選 “ 執行 ” ，若跳出 “ 下載擴充檔案 ” ，點選 “ 是 ” ；跳出 “ 使用預設選項 ” ，點選 “ OK ” ；跳出 “ 此動作將完全清除此裝置上的資料 ” 的警告，點選 “ 確定 ” 。

e. 等待綠色進度條跑完，製作完成後再關閉。

## 8.	再次進入 BIOS 設定開機順序為 USB 隨身碟優先。
  <br />

# 設置分區並安裝

## 1. 進入安裝介面

設置 USB 隨身碟優先啟動後，重新啟動電腦，進入 Ubuntu 安裝介面，照著上面的指示，一直到 “ 安裝類型 ” 步驟，選擇 “ 其他選項 ” 以自訂磁區安裝。

## 2. 配置

<span style="font-size:xx-large;font-weight:bold;">這裡很重要，要小心不要動到原有資料的磁區，不然可能會洗掉電腦中的資料！</span>

可以看到裝置清單中， /dev/sda 是 SSD ， /dev/sdb 是 HDD ，根據容量大小找到剛才在 Win 10 下預留的 64 GB 空磁區（找到大約是 65536 MB 即可，實際上大小可能會些微不同，這是因為壓縮時不一定恰恰好）。

在該分割出來的 64 GB 空磁區點一下滑鼠左鍵，並點選下方 “ + ” 號，依序下方說明配置：

(1) 用途為 “ EFI系統分區 ” ，選擇 “ 邏輯分區 ” 、 “ 此空間的開頭 ” 、 “ 500 MB（不要小於 256 MB ） ” 。

(2) 用途為 “ EXT4日誌式檔案系統 ” ，選擇 “ 邏輯分區 ” 、 “ 此空間的開頭 ” 、 “ 49152 MB（實際上填入剩下的減去預留給 swap 的 16 GB ） ”、掛載點為 “ / ” 。

(3) 用途為 “ 置換空間 ” （即為 swap ），選擇 “ 邏輯分區 ” 、 “ 此空間的開頭 ” 、 “ 16384 MB（ 16 GB ） ” 。

※	因為 EFI 是由 UEFI 引導，非傳統的 boot/grub 模式，所以不需設置 /boot。

## 3. 開始安裝

<span style="font-size:xx-large;font-weight:bold;">這裡很重要，要小心不要動到原有資料的磁區，不然可能會洗掉電腦中的資料！</span>

配置完後，在新建的 /dev/sdb 中的 “ EFI系統分區 ” 磁區雙擊滑鼠左鍵，並於下方 “ 用來安裝開機程式的裝置 ” 選單，選擇 /dev/sdb 中的 “ EFI系統分區 ” ，**此步驟要格外小心不要選錯！！！，此步驟要格外小心不要選錯！！！，此步驟要格外小心不要選錯！！！**

※	在 /dev/sda 中的 EFI系統分區可看到原本 Win 10 的 “ windows Boot Manager ” ，而 /dev/sdb 中的 EFI系統分區是剛剛建立的，**記得選後者，不要選錯！不然會悲劇！**

接下來按下立即安裝，一路安裝到完成。

# 調整開機順序

進入 BIOS 調整開機順序為新建立的 windows Boot Manager ，就大功告成啦（撒花～～～

<figure>
    <img src="https://2.bp.blogspot.com/-chobEMYdCIM/Wm3oNrRo8KI/AAAAAAAAG4w/7bfyCMiB89wUBRxvkKJi2YPQZE42dTD4gCKgBGAs/s1600/2018-01-27_235108.png" height="25%" width="25%">
</figure>

※	最後可以把先前關閉的 BIOS 的 Security Boot 再次設定開啟。

# Windows 和 Linux 的時差問題

後來我發現每次從 Ubuntu 切換回 Win 10 電腦中顯示的時間都會快8小時，原因是兩套作業系統使用不同的時間標準。

Windows　這套作業系統將電腦硬體時間當作本地時間（local time），所以在 Windows 中顯示的時間跟 BIOS 中顯示的時間是一樣的。

Linux（Unix-liked OS：如 Linux 和 Mac ）則把電腦硬體時間當作 UTC（Universal Time Coordinated，世界協調時間）， 在Linux　系統啟動後在該時間的基礎上，再加上額外設置的時區數（台灣的時區為 UTC+8 ），因此，Linux 系統中顯示的時間會比 Windows 系統中顯示的時間快 8 個小時。

而你在 Linux 系統中，把系統現實的時間設置正確後，實際上電腦硬體時間就是在這個時間上減去 8 小時，所以切換成 Windows 系統後，會發現時間慢了 8 小時。

所以我們只需將 Ubuntu 的 UTC 關閉即可，根據不同的 Ubuntu 版本，這個問題有不同的解法：

(1) 在 Ubuntu 16.04 版本以前，關閉 UTC 的方法是在終端機輸入：

```bash
sudo /etc/default/rcS
```

找到並將 UTC=yes 改成 UTC=no ，並重新開機即可。

(2) 在 Ubuntu 16.04 使用 systemd 啟動之後，時間改成了由 timedatectl 來管理，關閉 UTC 的方法則是在終端機輸入：

```bash
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```

並重新開機即可。

# 參考資料

(1)  USB 開機碟製作： <http://blog.xuite.net/yh96301/blog/450717778>

(2)  雙系統安裝： <http://www.cnblogs.com/willnote/p/6725594.html>

(3)  Windows 和 Linux 的時差問題： <https://www.zhihu.com/question/46525639>

