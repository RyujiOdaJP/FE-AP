# OS

+ メモリ管理
+ ファイル管理
+ HW管理

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

アプリケーション側からOSの機能を呼び出すためのインターフェース

+ 開発効率アップ
+ 操作性の統一
+ 互換性の確保

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

+ 到着順
+ 優先順
+ ラウンドロビン...一定時間単位で使用権の付与、終わらないタスクは最後に回される

