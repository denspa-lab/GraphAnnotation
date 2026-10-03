[Denspa] A Smart Analysis Environment for LTspice: Automatically Add Annotations and Guide Lines to Waveform Plots

Related Explanatory Video
https://youtu.be/uhqiTxVLzPk

Do you ever struggle to locate specific `.meas` measurement points on your LTspice waveform graphs?
Is it a hassle to manually hunt for them using cursors every time?
In this video, we introduce "GraphAnnotation," a Python tool that automatically adds measurement point markers and lists of calculation results to your plot files (`.plt`)!
It eliminates the need for manual position checking and dramatically improves the readability of TRAN and DC analysis results.
It also features tools for drawing Smith chart guide circles and includes reliable automatic backup and restore capabilities (`--rollback`).

# GraphAnnotation User Manual

Target version: V0.1.35 (2026-10-03)

## Features

GraphAnnotation adds measurement-point labels, vertical and horizontal lines, calculation result lists, and auxiliary circles for Smith charts to saved LTspice PLT files.

Circuit simulation, saving ASC/PLT files, and reloading the updated files are performed manually in LTspice.

Currently, the program updates the `.plt` file located in the same folder as the ASC file and having the same base name. Updating `.log.plt` files is not yet supported.

## Files and Setup

| File | Purpose |
| --- | --- |
| `GraphAnnotation.py` | Main program to be launched |
| `LTsimplefunctions.py` | Copy of the common library that provides character-encoding detection |
| `denspa.py` | Copy of the common library that provides the ASC file selection dialog and creation of `Autopy.ini` |
| `Autopy.ini` | Settings for the working folder at startup, including the LTspice executable and library paths |
| Schematic `.asc`, corresponding `.log`, and corresponding `.plt` | Input files for normal processing. Only the PLT file is modified |
| `plt_backup` | Backup-only subfolder created in the same folder as the PLT file |
| `EvaluationCircuit` | Evaluation circuits, reference PLT files, and documentation |

Python 3.14 and spicelib 1.5.1 are used. `chardet` and `tkinter` are also required because of dependencies in the common libraries.

Use the following batch file to activate the development environment:

```text
～python3.14\py314\Scripts\activate.bat
```

If `Autopy.ini` does not exist, the configuration creation process in `denspa` is started. Settings in other folders are not searched.

If `exec.Prog` in an existing configuration is invalid, the program does not automatically switch to another LTspice installation and instead reports an error.

## How to Launch

```text
py GraphAnnotation.py
py GraphAnnotation.py "EvaluationCircuit\TonToff\6_NPN_Switch.asc"
py GraphAnnotation.py --help
```

When launched without arguments, the schematic file selection dialog is displayed.

Relative paths are interpreted relative to the working folder from which the program was launched.

Paths containing spaces must be enclosed in double quotation marks.

The schematic filename may be omitted with any processing option. If omitted, the file selection dialog is displayed. If the dialog is canceled, the program exits normally without modifying any files.

Options may be specified either before or after the filename.

Only one processing mode may be selected at a time. The `--detail` option may be used together with a chart option, but `--detail` cannot be used by itself.

| Option | Operation |
| --- | --- |
| None | Replace existing measurement-point markers and update calculation results |
| `--append` | Add new measurement-point markers while keeping the existing markers for comparison |
| `--rollback` | Restore backups one at a time for confirmation |
| `--impedance` | Standard impedance auxiliary circles |
| `--impedance --detail` | Detailed impedance auxiliary circles |
| `--admittance` | Standard admittance auxiliary circles |
| `--admittance --detail` | Detailed admittance auxiliary circles |
| `--immittance` | Overlay impedance and admittance auxiliary circles |
| `--immittance --detail` | Overlay detailed auxiliary circles for both impedance and admittance |
| `--help` / `-h` | Display usage information and exit normally |

## Normal Operation

The program can be launched without specifying a filename, as shown below. The legacy `--smith` options work in the same manner.

```text
py GraphAnnotation.py --rollback
py GraphAnnotation.py --append
py GraphAnnotation.py --impedance
py GraphAnnotation.py --admittance --detail
py GraphAnnotation.py --immittance
```
---
# GraphAnnotation 操作説明書

対象：V0.1.35（2026-10-03）

## できること

LTspiceの保存済みPLTへ測定点の文字・縦横線、計算結果一覧、スミスチャートの補助円を追加します。
回路のシミュレーション、ASC/PLTの保存、更新後の再読み込みはLTspice側で手動操作します。
現在はASCと同じ場所・同じベース名の.pltを更新します。.log.pltの更新は未対応です。

## 構成と準備

| ファイル | 役割 |
| --- | --- |
| GraphAnnotation.py | 起動するプログラム |
| LTsimplefunctions.py | 文字コード判定を提供する共通ライブラリのコピー |
| denspa.py | ASC選択ダイアログとAutopy.ini作成を提供するコピー |
| Autopy.ini | 起動時の作業フォルダーの設定。LTspice実行ファイルとライブラリパス |
| 回路図.asc・同名.log・同名.plt | 通常処理の入力。変更するのはPLTのみ |
| plt_backup | PLTと同じフォルダーに作成するバックアップ専用サブフォルダー |
| EvaluationCircuit | 評価回路と参考PLT・資料 |

Python 3.14、spicelib 1.5.1を使用します。共通ライブラリの依存でchardetとtkinterも必要です。
開発環境の有効化は次のバッチです。

```text
～python3.14\py314\Scripts\activate.bat
```

Autopy.iniがなければdenspaの設定作成処理を起動します。他のフォルダーの設定は探しません。
既存設定のexec.Progが不正な場合は自動で別のLTspiceに切り替えず、エラーになります。

## 起動方法

```text
py GraphAnnotation.py
py GraphAnnotation.py "EvaluationCircuit\TonToff\6_NPN_Switch.asc"
py GraphAnnotation.py --help
```

引数なしは回路図選択ダイアログです。相対パスは起動時の作業フォルダー基準です。
空白を含むパスは二重引用符で囲みます。すべての処理オプションで回路図名を省略でき、
省略時は選択ダイアログを開きます。キャンセルするとファイルを変更せず正常終了します。
オプションはファイル名の前後どちらでも指定できます。処理は一つだけ選び、
チャート指定には--detailを併用できます。--detail単独は使用できません。

| オプション | 操作 |
| --- | --- |
| なし | 旧測定点マーカーを置換し、計算結果を更新 |
| --append | 比較用に旧測定点マーカーを残して追加 |
| --rollback | バックアップを一つずつ復元して確認 |
| --impedance | 標準インピーダンス補助円 |
| --impedance --detail | 詳細インピーダンス補助円 |
| --admittance | 標準アドミタンス補助円 |
| --admittance --detail | 詳細アドミタンス補助円 |
| --immittance | インピーダンス・アドミタンスの補助円を重ねて表示 |
| --immittance --detail | 両方の詳細補助円を重ねて表示 |
| --help / -h | 使用法を表示して正常終了 |

## 通常の操作手順

ファイル名を入力せず、次のように起動できます。旧--smith系も同様です。

```text
py GraphAnnotation.py --rollback
py GraphAnnotation.py --append
py GraphAnnotation.py --impedance
py GraphAnnotation.py --admittance --detail
py GraphAnnotation.py --immittance
```

