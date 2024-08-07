---
title: Print Support App (PSA) にて用紙の設定が既定の値にならない
date: 2024-08-07 00:00:00
categories:
- printscan
tags:
- printer
---
この記事では Print Support App で発生する問題について説明します。
<!-- more -->
<br>

***
### 概要
PSA の `PrintSupportExtensionSession.PrintDeviceCapabilitiesChanged` イベント ハンドラーで PrintDeviceCapabilities に追加の PageMediaType オプションを追加すると、既定のオプションが選ばれない現象が発生します。

***
### 現象
例えば `PageMediaType` Feature で `ContosoMediaType` というのを追加し、次のような PrintTicket を構成した場合 `default="true"` が指定されている `AutoSelect` がオプションとして既定の設定となることが期待されます。

```
<!-- media-type-supported -->
<psk:PageMediaType psf2:psftype="Feature">
    <psk:AutoSelect psf2:psftype="Option" psf2:default="true"/>
    <psk:PhotographicGlossy psf2:psftype="Option" psf2:default="false"/>
    <psk:Photographic psf2:psftype="Option" psf2:default="false"/>
    <contoso:ContosoMediaType psf2:psftype="Option" psf2:default="false"/>
</psk:PageMediaType>
```

しかし、実際には次の画像のように別のオプションが PrintTicket の既定で選択される現象が発生します。本現象は PSA の印刷設定画面の最初の表示時に発生します。

<img src="https://jpwdkblog.github.io/images/psa-default-value/taskman1.png" width=600px align="left" border="1"><br clear="left">


***
### 状況

この現象については現在調査中であり、アップデートがあり次第、更新いたします。
  


<br>

***
### 関連ドキュメント

[PrintSupportExtensionSession.PrintDeviceCapabilitiesChanged イベント](https://learn.microsoft.com/ja-jp/uwp/api/windows.graphics.printing.printsupport.printsupportextensionsession.printdevicecapabilitieschanged?view=winrt-26100)
