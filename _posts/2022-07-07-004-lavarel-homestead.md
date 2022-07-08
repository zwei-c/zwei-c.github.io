---
title: Windows部署Laravel Homestead
date: 2022-07-07
categories:
- Server
tags:
- Laravel
- Vagrant
---

---

# 前言

以前寫的PHP專案都是用`CodeIgniter`當框架來開發，但最近公司要轉用`Laravel`來當之後的開發框架，因為之前虛擬環境都是用`Vagrant`來架設，估狗*windows 開發 laravel*裡面找到[lavavel.tw](https://laravel.tw/)裡面就有答案了。殊不知年輕人就是年輕人想得太少了...原來`lavavel.tw`這個官方文檔對應的`laravel`版本是`5.3`。看了一下公司專案的版本...`8.x`!會這樣講，是因為我又是裝完之後才發現的，~~因為要Build好開發環境才能看專案阿XD~~都怪我沒仔細看專案內容QQ，但裝了都裝了，我還是要記錄一下`Laravel Homestead`的架設過程。
<!-- more -->
# Vagrant

`Vagrant`是一個用`Vagrantfile`來設定的虛擬環境軟體，要使用你要先安裝：
- [Vagrant](https://www.vagrantup.com/)
- [VirtualBox](https://www.virtualbox.org/)
<div class="alert">
Vagrant跟VirtualBox的版本有對應關係，需要各自安裝對的版本才能正常運行！
</div>

具體上的教學網路有很多且不是本文重點，我就不贅述了。

# Laravel Homestead

>Laravel Homestead 是一個官方預載的 Vagrant box，提供一個美好的開發環境，不需要在本機電腦安裝 PHP、HHVM、網頁伺服器或任何伺服器軟體。不用擔心搞亂你的系統！Vagrant box 可以搞定一切。如果有什麼地方搞爛掉了，還可以在幾分鐘內快速地砍掉並重建虛擬機器！

## 安裝 Homestead Vagrant Box

當 VirtualBox / VMware 以及 Vagrant 安裝完成後，可以在終端機執行下列指令將 `laravel/homestead` 這個 box 安裝進你的 Vagrant 程式中。下載 box 會花一點時間，時間長短將依據你的網路速度決定：

```
vagrant box add laravel/homestead
```

## 安裝 Homestead

可以簡單地透過複製(clone)儲存庫的方式來安裝 Homestead。建議可將儲存庫複製至你的「home」目錄中的 Homestead 資料夾，如此一來 Homestead box 將能提供主機服務給你所有的 Laravel 專案：

```
git clone https://github.com/laravel/homestead.git Homestead
```

複製完 Homestead 儲存庫，即可在 Homestead 目錄中執行 `bash init.sh` 指令來建立 `Homestead.yaml` 設定檔。 `Homestead.yaml` 檔案將會被放置在`homestead`目錄中：

```
./init.bat
```
## 設定 Homestead

### 設定提供者
在 `/homestead/Homestead.yaml` 檔案中的 `provider` 是用來設定想要使用哪一個 Vagrant 提供者，像是：`virtualbox`、`vmware_fusion`、`vmware_workstation` 或 `parallels`。這邊我們要填寫:

```
provider: virtualbox
```

### 設定共享目錄

在 `Homestead.yaml` 檔案的 `folders` 屬性裡列出所有想與 Homestead 環境共享的目錄。這些目錄中的檔案若有更動，它們將會同步更動在你的本機電腦與 Homestead 環境。可以將多個共享目錄都設定於此：

```
folders:
    - map: C:\Code
      to: /home/vagrant/Code
```

### 設定 Nginx 網站
`sites` 屬性幫助你可以輕易的指定「網域」對應至 homestead 環境中的目錄。在 `Homestead.yaml` 檔案中已包含一個網站設定範例。同樣的，你可以增加數個網站到 Homestead 環境中。Homestead 可以為每個你正在開發中的 Laravel 專案提供方便的虛擬化環境：
```
sites:
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public
```
在配置 Homestead box 之後，若有更改 sites 屬性，你應該重新執行配置指令 `vagrant reload --provision` 來更新虛擬機裡的 Nginx 設定。

### Hosts 檔案

你必須為 Nginx 網站在你機器中的 `hosts` 檔案增加「網域」。`hosts` 檔案會將你對 Homestead 網站的請求重導至 Homestead 機器。在 Mac 或 Linux 上，在 Windows 上，則存放於 `C:\Windows\System32\drivers\etc\hosts`。你增加至該檔案的內容看起來會像這樣：
```
192.168.10.10  homestead.app
```
務必確認 IP 位置與你的 `Homestead.yaml` 檔案中設定相同。一旦將網域設定在 hosts 檔案之後，你就可以透過網頁瀏覽器造訪網站！
```
http://homestead.app
```

## 啟動 Vagrant Box

當你編輯完 `Homestead.yaml`後，開啟終端機，進入 Homestead 目錄，並執行 `vagrant up` 指令。Vagrant 就會自將虛擬主機啟動並自動設定共享目錄和 Nginx 網站。

如果要移除虛擬機器，可以使用 `vagrant destroy --force` 指令。

## 透過 SSH 連接

你可以在終端機裡進入你的 Homestead 目錄，並執行 `vagrant ssh` 指令藉此以 SSH 連上你的虛擬主機。

# 參考資料

[laravel.tw/docs/5.3/homestead](https://laravel.tw/docs/5.3/homestead)