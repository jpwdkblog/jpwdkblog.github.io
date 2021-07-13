---
title: Windows 11 HLK の Create project のダイアログの "Is Windows Driver Project?" にチェックは入れないでください
date: 2021-07-13 00:00:00
categories:
- Hardware Lab Kit
tags:
- Windows 11 HLK
---
Windows PE で IOCTL サンプルを動作させる方法についてご紹介します。
<!-- more -->
<br>

***
Windows 11 HLK では、「[手順 4:プロジェクトの作成](https://docs.microsoft.com/ja-jp/windows-hardware/test/hlk/getstarted/step-4-create-a-project)」の際に、以下のような "Is Windows Driver Project?" というチェックボックスがある “Create project” のダイアログが表示されます。  
 
 <img src="https://jpwdkblog.github.io/images/HLK-create-project/CreateProject.png" width=400px><br clear="left">  
 
誠に恐れ入りますが、Windows 11 用の認定や署名の取得のために、Hardware Dev Center Dashboard に提出される場合には、このチェックボックスに <u>**チェックは入れない**</u> ようにお願いいたします。  
 
このチェックボックスは、本来 Dashboard へのご提出用を目的としておらず、弊社開発部門等と協業されているお客様が、その担当部門より指示があった場合にご利用されるものとなっており、将来削除予定となっております。  
 
もしこれをチェックしてプロジェクトを作成して提出された場合、Hardware Dev Center Dashboard 側ではそれを処理できず、ご提出は Pass せず止まったまま、もしくは、Dashboard 側の担当部門にて Cancel の取り扱いとなります。  
この場合、Windows 11 HLK のご提出をご要望される場合は、以下のいずれかの対処策があります。  

- (a) 上記プロジェクトの作成をチェックボックスにチェックを入れずにやり直し、[DF - InfVerif INF Verification (bbcc1b46-d0bf-46c8-85b4-2cd62df34a20)](https://docs.microsoft.com/en-us/windows-hardware/test/hlk/testref/bbcc1b46-d0bf-46c8-85b4-2cd62df34a20) のみ実施し、上記の Pass しなかったパッケージを、HLK Studio の Package タブの [Add Supplemental Folder] ボタンで Supplemental folder に追加した、提出パッケージを作成します。  

- (b)	万が一、何らかの理由で (a) の方法がうまくいかない場合は、上記プロジェクトの作成からチェックを入れずにやり直しし、テストを全て再度ご実施いただきます。  
 
上記の問題を弊社は認識しておりますため、改善に向けたアクションを進めております。ただ、誠に恐れ入りますが、現時点では、上記の回避策があるため、Windows 11 HLK そのものの修正は行われず、次のバージョンのリリースに修正が含まれる予定です。そのため、誠にお手数ではございますが、Windows 11 HLK につきましては、上記の対処でのご対応をお願い申し上げます。  
 
***
`変更履歴`  
`2021/07/13 created by Tsuda`

※ 本記事は 「[jpwdkblog について](https://jpwdkblog.github.io/blog/2020/01/01/aobut-jpwdkblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpwdkblog.github.io/blog/)」 および 「[記事一覧](https://jpwdkblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  