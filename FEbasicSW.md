# OS

- メモリ管理
- ファイル管理
- HW管理

## ソフトウェア分類

```mermaid
graph LR;
  応用ソフト --- |DBなど|ミドルウェア
  応用ソフト --- 基本ソフトウェア
subgraph System Software
  ミドルウェア --- 基本ソフトウェア
end
  基本ソフトウェア --- HW
```

```mermaid
graph LR;
subgraph OS
  基本ソフトウェア-->制御プログラム
  基本ソフトウェア-->言語処理プログラム
  基本ソフトウェア-->サービスプログラム
    subgraph Kernel
      制御プログラム---Proccess
      制御プログラム---TaskManagement
      制御プログラム---MemoryManagement
    end
  
    言語処理プログラム---|翻訳|Java
    言語処理プログラム---|翻訳|C
    言語処理プログラム---|翻訳|etc..
  
  
    サービスプログラム
  
end
style OS fill:#8aa,stroke:#333,stroke-width:3px,color:fff
```

## API

>アプリケーション側からOSの機能を呼び出すためのインターフェース

- 開発効率アップ
- 操作性の統一
- 互換性の確保

## タスク管理

### Jobは実行単位であるtaskに分解される

```mermaid
graph LR
Job --> Task1
Job --> Task2
Job --> Task3
```

### Taskの状態遷移

```mermaid
graph LR
RUN --> |入出力中|WAIT
READY --> |優先権GET|RUN
WAIT --> |実行可能|READY
RUN --> |優先順位変更|READY
RUN --> |タスク消失|TERMINATE
```

タスクがたまっていき、使用権を得るまで待つ。
使用権を管理するのはディスパッチャの役割。

### ディスパッチャ

- 到着順
- 優先順
- ラウンドロビン...一定時間単位で使用権の付与、終わらないタスクは最後に回される

## マルチプログラミング

> CPUの遊休時間を減らすために処理を並行して行う

```mermaid
gantt
dateFormat  ss
title Multi Programming
section A section

Active CPU               :active,  des2, 1, 3s
Future task1               :         des3, after des2, 5s
Future task2               :         des4, after des3, 5s
Active CPU               :des2, a, 3s
```

## 実記憶管理

限られた主記憶空間を効率良く使うための管理方法

### 固定区画

>固定長のパーティションに区切り、プログラムを割り当てる

### 可変区画

>プログラムをロードする過程で必要なサイズに区切る

### フラグメンテーション＆メモリコンパクション

可変で良いように見えるがプログラムの終了が上から順とは限らない=> 断片化する

**これを解決するのがメモリコンパクション（ガベージコレクション）**
断片化した領域にプログラムを並べ直す。

### オーバーレイ方式

> そもそも実行したいプログラムが容量より大きいとロードできないので、プログラムをセグメント化し必要な文だけをメモリにロードする方法

### スワッピング方式

> 割り込み処理などの際、優先度の低いプログラムを補助記憶装置へ一旦退避させる

## 再配置可能なプログラム

メモリのアドレスが変わるけど大丈夫なのか問題。
=> **ベースアドレス方式**でロードされたときの先頭アドレスからの差分でアドレス位置を指定できる。

そのため、どこにロードされても大丈夫。このような性質のプログラムを再配置可能プログラムという。

- 再配置可能　どこにロードされても大丈夫
- 再使用可能　再ロードせず繰り返し実行できる
- 再入可能　データ部分をタスクごとに持つことで複数タスクから呼び出しても干渉しない

```mermaid
graph LR


  programA --> 手続き部分
  programB --> 手続き部分
subgraph programC
  手続き部分 --> データ部分A
  手続き部分 --> データ部分B
end

```

- 再帰的　実行中に自分自身を呼び出せる(仮にProgramAとすると) 

```mermaid
graph TD
subgraph folder group
  folderA---folderB---folderC
end
programA-->|look up by programA|folderA
programA-->|keep statement|stackArea
programA-->|call programA:loop|programA
programA-->|look up by programA|folderB
programA-->|look up by programA|folderC

```