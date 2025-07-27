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
この記事では、Windows 11 24H2 および Windows Server 2025 上での印刷処理において発生するメモリリークについて説明します。
<!-- more -->  
<br>

***
### 概要
Windows Version 24H2 (OS Build 26100) において、v3 プリンター ドライバーを使ってドキュメントを印刷すると、スプーラー サービス (spoolsv.exe) でメモリリークが発生することを確認しております。　　

この現象は、v3 プリンター ドライバーの DrvEnableSurface コールバック関数以外のコールバック関数で [EngCreateBitmap 関数](https://learn.microsoft.com/ja-jp/windows/win32/api/winddi/nf-winddi-engcreatebitmap) を呼び出している場合に発生します。例えば、DrvBitBlt や DrvStretchBlt などのようなビット ブロック転送機能を実装するドライバーのコールバック関数で EngCreateBitmap / [EngDeleteSurface 関数](https://learn.microsoft.com/ja-jp/windows/win32/api/winddi/nf-winddi-engdeletesurface) の呼び出しで発生します。  
印刷するドキュメントの内容によって、ドライバーで実行されるコードパスが変わるため、事象の発生は 100 %ではありません。
<br>  

  
***  
### 現象
印刷を繰り返すと、spoolsv.exe のメモリ使用量 (タスク マネージャーのワーキング セットやコミット サイズ) の値が徐々に増加します。
   <img src="https://jpwdkblog.github.io/images/print-memleak-issue/taskmgr.png" align="left" border="1"><br clear="left">
<br>

  
また、実際にメモリの解放が失敗した時には、EngDeleteSurface 関数の呼び出しが失敗 (FALSE を返す) ことが確認されています。ドライバー側の処理で、EngDeleteSurface 関数の戻り値を確認していただくことでも、事象の発生有無を確認することができます。
<br>  


***  
### 回避方法
現状、この問題を確実に回避する方法はありませんが、プリンタードライバーの分離機能を有効化することで、常時実行されているスプーラー サービスで発生するメモリリークの問題を別のプロセス (PrintIsolationHost.exe) に分離することができます。この PrintIsolationHost.exe は常駐で実行されるものではなく、一定時間使用されなくなるとプログラムが自動で終了します。確保されていたメモリはプログラム (.exe) が終了すると、強制的に解放されるため、結果的にメモリリークしていたメモリも解放さることになります。  
プリンタードライバーの分離機能を有効化するためには、次の手順を実行します。

1. [印刷の管理] を起動します。  
<img src="https://jpwdkblog.github.io/images/print-memleak-issue/printmgmt.png" align="left" border="1"><br clear="left">  

2. 左側のペインで [プリント サーバー] - [<PC 名> (ローカル)] - [ドライバー] の順に展開します。対象のプリンタードライバーを右クリックして [ドライバーの分離の設定] - [分離] の順にクリックして機能を有効化します。
<img src="https://jpwdkblog.github.io/images/print-memleak-issue/isolation.png" align="left" border="1"><br clear="left">    
<br>


***
### 状況
本問題は、Windows における GDI の問題であると認識しており、現在修正作業行っております。  
現時点で、Windows 11 24H2 に対する修正は 2025 年 9 月の提供を目標に作業中です。Windows Server OS については、クライアント OS の提供後、約 4 カ月後となるため 2026 年 1 月あるいは 2 月に提供となる見込みです。なお、修正プログラムの提供は、他の修正プログラムの影響なども受けるため、状況によって予定は変更となる場合がございます。今後の状況については、アップデートがあり次第こちらで更新します。

2025 年 7 月 28 日更新
本問題の修正が含まれる累積更新プログラム全体のテスト パスが開始中しています。現段階では作業に遅延は確認されていません。

<!--Original 57827011-->
<!--Parent 55401107/2025.08D-->

<br>



***
`変更履歴`  
`2025/07/10 created by riwaida`  
`2025/07/28 updated by riwaida`  

※ 本記事は 「[jpwdkblog について](https://jpwdkblog.github.io/blog/2020/01/01/aobut-jpwdkblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpwdkblog.github.io/blog/)」 および 「[記事一覧](https://jpwdkblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  
