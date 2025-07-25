---
title: ビューポートの meta 要素の使用
short-title: ビューポートの meta 要素
slug: Web/HTML/Guides/Viewport_meta_element
l10n:
  sourceCommit: e9b6cd1b7fa8612257b72b2a85a96dd7d45c0200
---

{{HTMLSidebar}}

この記事では、 `<meta>` "viewport" タグを使用して、ビューポートのサイズと形状を制御する方法について記述しています。

## 背景

ブラウザーの{{glossary("viewport", "ビューポート")}}は、ウェブコンテンツを見ることができるウィンドウの領域です。これはレンダリングされたページと同じサイズではないことが多く、その場合、ブラウザーはユーザーがスクロールしてすべてのコンテンツにアクセスできるよう、スクロールバーを提供します。

モバイル端末やその他の狭い画面では、通常画面よりも広い仮想ウィンドウまたはビューポートでページをレンダリングし、レンダリング結果を縮小して、すべてを一度に見ることができるようにするものがあります。ユーザーは、ページのさまざまな部分をより詳しく見ていくために、ズームやパンを行うことができます。例えば、モバイル画面の幅が 640px の場合、ページは 980px の仮想ビューポートでレンダリングされ、 640px の空間に収まるように縮小されるかもしれません。

これは、すべてのページがモバイル用に最適化されているわけではなく、小さなビューポート幅でレンダリングすると壊れてしまう（あるいは少なくとも見た目が悪くなる）ために行われます。この仮想ビューポートは、モバイル端末に最適化されていないサイト全般を、画面が狭い端末でも見やすくするための方法です。

しかし、この仕組みは、[メディアクエリー](/ja/docs/Web/CSS/CSS_media_queries)を使用して狭い画面に最適化されているページにはあまり向いていません。例えば、仮想ビューポートが 980px の場合、640px や 480px 以下で起動するメディアクエリーは絶対に使用せず、こうしたレスポンシブデザイン手法の有効性を限定してしまうのです。ビューポートの `<meta>` 要素は、このような画面の内側が狭い端末における仮想ビューポートの問題を緩和するものです。

## ビューポートの基本

viewport は機能と値のペアをカンマで区切ったリストです。一般的なモバイルに最適化されたサイトには、以下のようなものがあります。

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

すべての端末が同じ幅というわけではありません。画面のサイズや方向が大きく異なる場合でも、ページがうまく動作するようにする必要があります。

`<meta>` "viewport" 要素の基本的な属性は以下の通りです。

- `width`
  - : ビューポートの（最小の）大きさを制御します（[ビューポートの幅と画面の幅](#ビューポートの幅と画面の幅)を参照）。これは `width=600` のような具体的なピクセル数で設定するか、特別な値 `device-width` で、CSS ピクセル数による端末の画面の物理的なサイズを設定します。この値は [`vw`](/ja/docs/Web/CSS/length#ビューポートのパーセント値による寸法) の単位を確立します。最小値は `1` です。最大値は `10000` です。負の値は無視されます。
- `height`
  - : ビューポートの（最小の）大きさを制御します（[ビューポートの幅と画面の幅](#ビューポートの幅と画面の幅)を参照）。これは `height=400` のような具体的なピクセル数で設定するか、特別な値 `device-height` で、CSS ピクセル数による端末の画面の物理的なサイズを設定します。この値は [`vh`](/ja/docs/Web/CSS/length#ビューポートのパーセント値による寸法) の単位を確立します。最小値は `1` です。最大値は `10000` です。負の値は無視されます。
- `initial-scale`
  - : ページを最初に読み込んだときの表示倍率を制御します。最小値は `0.1` です。最大値は `10` です。既定値は `1` です。負の値は無視されます。
- `minimum-scale`
  - : ページ上でどの程度ズームアウトを許可するかを制御します。最小値は `0.1` です。最大値は `10` です。既定値は `0.1` です。負の値は無視されます。
- `maximum-scale`
  - : ページで許可される拡大率を制御します。 3 より小さい値を設定するとアクセシビリティに違反します。最小値は `0.1` です。最大値は `10` です。既定値は `10` です。負の値は無視されます。
- `user-scalable`
  - : ページ上でズームインとズームアウトのアクションが許可されるかどうかを制御する。有効な値は `0`, `1`, `yes`, `no` の何れかです。既定値は `1` で、これは `yes` と同じです。値を `0` （つまり `no` と同じ）に設定すると、ウェブコンテンツアクセシビリティガイドライン (WCAG) に違反することになります。
- `interactive-widget`
  - : 仮想キーボードなどの対話型 UI ウィジェットがページのビューポートに及ぼす効果を指定します。有効な値は `resizes-visual`、`resizes-content`、`overlays-content` です。既定値は `resizes-visual` です。

> [!WARNING]
> `user-scalable=no` を使用すると、弱視などの視覚的な障碍を持つユーザーに対して、アクセシビリティの問題を発生させる可能性があります。[WCAG](/ja/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable#ガイドライン_1.4_前景と背景の区別を含め、ユーザーがコンテンツを見たり聞いたりしやすくする) では少なくとも 2 倍まで拡大するように要求していますが、 5 倍の拡大を可能にすることが最善の習慣とされています。

## 画面の密度

画面解像度が上がり、人間の目では個々のピクセルが識別できないサイズになりました。例えば、スマートフォンの画面は、小さな画面でも 1920 〜 1080 ピクセル（〜 400dpi）以上の解像度を持っている場合がほとんどです。このため、多くのブラウザーは、 CSS 「ピクセル 」 1 つに対して複数のハードウェアピクセルを対応させることで、より小さな物理サイズでページを表示することができます。当初は、タッチ操作に最適化された多くのウェブサイトで、ユーザビリティと読み取り可能なサイズの問題が発生しました。

高 dpi の画面においては、`initial-scale=1` を指定したページは、ブラウザーに効果的に拡大表示されることになります。テキストは滑らかで鮮明に表示されますが、ビットマップ画像は画面の内側まで解像度を生かしきれないかもしれません。こうした画面においては、より鮮明な画像を実現するために、ウェブ開発者は画像やレイアウト全体を最終的なサイズよりも高い倍率で設計し、 CSS やビューポートプロパティを使用して縮小することが求められるかもしれません。

既定のピクセル比は、ディスプレイの密度に依存します。密度が 200dpi 未満のディスプレイでは、比率は 1.0 です。密度が 200 〜 300dpi のディスプレイでは、比率は 1.5 です。 300dpi 以上のディスプレイの場合、比率は整数の floor (_density_/150dpi) となります。既定の比率は、ビューポートの縮小率が 1 に等しい場合にのみ適用されることに注意してください。それ以外の場合は、 CSS ピクセルとデバイスピクセルの関係は、現在のズームレベルに依存します。

## ビューポートの幅と画面の幅

サイトでは、ビューポートを特定のサイズに設定することができます。例えば、`"width=320, initial-scale=1"` という定義は、縦長モードの小さな携帯電話のディスプレイに正確にフィットするように使用することができます。これは、ブラウザーがより大きなサイズでページを表示しない場合に問題が発生する可能性があります。これを修正するために、ブラウザーは要求された縮尺で画面を埋めるために必要であればビューポートの幅を拡大させます。これは、特に大画面の端末で使用する場合に有用です。

初期または最大の縮尺を設定したページでは、 `width` プロパティが実際には最小のビューポート幅に対応することを意味しています。例えば、レイアウトに少なくとも 500 ピクセルの幅が必要な場合、以下のようなマークアップを使用することができます。画面幅が 500 ピクセル以上ある場合、ブラウザーは画面に合わせるために（ズームインするのではなく）ビューポートを拡大します。

```html
<meta name="viewport" content="width=500, initial-scale=1" />
```

他にも利用できる[属性](/ja/docs/Web/HTML/Reference/Elements/meta#属性)として `minimum-scale`, `maximum-scale`, `user-scalable` があります。これらのプロパティは、初期の拡大縮小や横幅に影響を与え、また、ズームレベルの変更を制限します。

## 対話型 UI ウィジェットの影響

ブラウザーの対話型 UI ウィジェットは、ページのビューポートのサイズに影響を与えることができます。最も一般的な UI ウィジェットは仮想キーボードです。ブラウザーが使用するリサイズ動作をコントロールするには、`interactive-widget` プロパティを設定します。

許可されている値は次の通りです。

- `resizes-visual`
  - : {{Glossary("visual viewport", "視覚ビューポート")}}が対話型ウィジェットによってリサイズされます。
- `resizes-content`
  - : {{Glossary("viewport", "ビューポート")}}が対話型ウィジェットによってリサイズされます。
- `overlays-content`
  - : {{Glossary("viewport", "ビューポート")}}と{{Glossary("visual viewport", "視覚ビューポート")}}のどちらも対話型ウィジェットによってリサイズされません。

```html
<meta name="viewport" content="interactive-widget=resizes-content" />
```

{{Glossary("viewport", "ビューポート")}}がリサイズされると、初期の[包含ブロック](/ja/docs/Web/CSS/CSS_display/Containing_block)もリサイズされ、[ビューポート単位](/ja/docs/Web/CSS/length#ビューポートに基づく相対的な長さの単位)の計算されたサイズに影響を与えます。

## モバイル端末とタブレット端末の一般的なビューポートサイズ

どのモバイル端末とタブレット端末がどのようなビューポート幅を持っているかを知りたい場合は、[ここにモバイル端末とタブレット端末のビューポートサイズ](https://experienceleague.adobe.com/ja/docs/target/using/experiences/vec/mobile-viewports)の包括的なリストが掲載されています。ここでは、縦向きと横向きのビューポート幅、物理的な画面サイズ、オペレーティングシステム、デバイスのピクセル密度などの情報を提供しています。

## 仕様書

{{Specifications}}

## 関連情報

- 記事: [Android 版 Chrome で予定されているビューポートのサイズ変更に関する動作変更に備える](https://developer.chrome.com/blog/viewport-resize-behavior/)（英語）
