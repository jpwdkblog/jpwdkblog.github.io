---
title: UWP アプリから nul ポートが指定されているプリンターに印刷できない
date: 2022-01-10 17:30:00
categories:
- printscan
tags:
- Windows 10
---
この記事では、Windows 10 における印刷における問題について説明します。
<!-- more -->
<br>

***
### 概要
現在、Windows 10 の [2021 年 12 月 14 日— KB5008212 (OS ビルド 19041.1415、19042.1415、19043.1415、および 19044.1415)](https://support.microsoft.com/ja-jp/topic/2021-%E5%B9%B4-12-%E6%9C%88-14-%E6%97%A5-kb5008212-os-%E3%83%93%E3%83%AB%E3%83%89-19041-1415-19042-1415-19043-1415-%E3%81%8A%E3%82%88%E3%81%B3-19044-1415-b46200db-74c3-450e-b200-51013957312a) を適用した環境において、UWP アプリから `nul:` ポートが指定されているプリンターで印刷が失敗すると、失敗することが確認しています。

現象
- プリンターのプロパティにて、プリンター ポートとして次のように `nul:` が指定されている状態です。
   <div align="left">
   <img src="https://jpwdkblog.github.io/images/printer-null-port-issue/printerprops.png">
   </div>
- [フォト] などの UWP ベースのアプリから印刷すると、次のようなエラーが発生します。
   <div align="left">
   <img src="https://jpwdkblog.github.io/images/printer-null-port-issue/print-error.png">
   </div>

<br>

***
### 回避方法
お手数おかけいたしますが、この問題に遭遇した場合、次の手順に沿ってプリンターポートの変更を行い問題を回避してください。

(1) まず、OS を最新の状態にするため、Windows Update で最新の更新プログラムを適用します。  
現象の回避のためには、[2022 年 2 月 15 日 — KB5010415 (OS ビルド 19042.1566、19043.1566、および 19044.1566) プレビュー](https://support.microsoft.com/ja-jp/topic/2022-%E5%B9%B4-2-%E6%9C%88-15-%E6%97%A5-kb5010415-os-%E3%83%93%E3%83%AB%E3%83%89-19042-1566-19043-1566-%E3%81%8A%E3%82%88%E3%81%B3-19044-1566-%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC-5a644b82-83f4-4cc2-a0e7-85f643252386) 以降の更新プログラムが適用されている必要があります。

(2) 管理者権限で Windows PowerShell を起動します。

(3) 次の順でコマンドを入力します。 
```powershell
PS> Add-PrinterPort -Name <"任意のポート名">
PS> Set-Printer -Name <"プリンター名"> -PortName <"Add-PrinterPort で追加したポート名">
```
例  
   <div align="left">
   <img src="https://jpwdkblog.github.io/images/printer-null-port-issue/powershell.png">
   </div>
  
(4) プリンターのプロパティを確認して、以下のように変更が適用されていれば完了です
   <div align="left">
   <img src="https://jpwdkblog.github.io/images/win11-usbscan-issue/printerprops-fix.png">
   </div>

<br>

***
### 状況

この現象については、Windows 10 の問題であると認識しており、現在調査中となります。

<br>



***
`変更履歴`  
`2022/03/10 created by riwaida`

※ 本記事は 「[jpwdkblog について](https://jpwdkblog.github.io/blog/2020/01/01/aobut-jpwdkblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpwdkblog.github.io/blog/)」 および 「[記事一覧](https://jpwdkblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  
