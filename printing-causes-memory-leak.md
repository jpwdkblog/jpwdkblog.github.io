---
title: Windows 11 24H2 で印刷を行うとメモリリークが発生することがある
date: 2025-07-09 17:00:00
categories:
- printscan
tags:
- print
- Windows 11 24H2
- Windows Server 2025
---
この記事では、Windows 11 24H2 および Windows Server 2025 における印刷におけるメモリリークについて説明します。
<!-- more -->
<br>

***
### 概要
Windows Version 24H2 (OS Build 26100) において、v3 プリンター ドライバーを使ってドキュメントを印刷すると、スプーラー サービス (spoolsv.exe) でメモリリークが発生することを確認しております。　　

この現象は、v3 プリンター ドライバーの DrvEnableSurface コールバック関数以外のコールバック関数で EngCreateBitmap 関数を呼び出している場合に発生します。例えば、DrvBitBlt や DrvStretchBlt などのようなビット ブロック転送機能を実装するドライバーのコールバック関数で EngCreateBitmap / EngDeleteSurface の呼び出しで発生します。  
<br>  

  
### 現象
印刷を繰り返すと、spoolsv.exe のメモリ使用量 (タスク マネージャーのワーキング セットやコミット サイズ) の値が徐々に増加します。
   <img src="https://jpwdkblog.github.io/images/print-memleak-issue/taskmgr.png" align="left" border="1"><br clear="left">
<br>


***
### 状況
本問題は、Windows における GDI の問題であると認識しており、現在修正に向けて作業中です。  
Windows 11 24H2 については 2025 年中の修正を予定しております。今後アップデートがあり次第、更新いたします。


<br>



***
`変更履歴`  
`2025/07/10 created by riwaida`  

※ 本記事は 「[jpwdkblog について](https://jpwdkblog.github.io/blog/2020/01/01/aobut-jpwdkblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpwdkblog.github.io/blog/)」 および 「[記事一覧](https://jpwdkblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  
