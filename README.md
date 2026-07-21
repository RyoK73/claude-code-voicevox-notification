## VOICEVOX Notification

Claude Hooksで`./scripts/`のBashスクリプトを起動し、`./sounds/`に保存したVOICEVOX音声で作業終了などの通知を行う

## 動作確認環境

下記必要ソフトがインストールされていれば動作すると思います。

- Arch Linux

## 必要ソフト

| ソフト名    | 説明                                          |
| ----------- | --------------------------------------------- |
| paplay      | 再生ソフト。再生音量を指定可能                |
| aplay       | macOS標準の再生ソフト。再生音量を指定できない |
| Claude Code | コーディングエージェント                      |

## 使い方

1. `cd $HOME/.claude`
1. `git clone https://github.com/RyoK73/claude-code-voicevox-notification.git`
1. *setting.jsonへの追記例*を参考に`.claude/setting.json`に追加する。

## 通知音の一覧

| 音声                     | 通知音名          | Hooks        | 条件                         |
| ------------------------ | ----------------- | ------------ | ---------------------------- |
| 作業が完了したのだ       | done.wav          | Stop         | -                            |
| 権限確認が必要なのだ     | permission.wav    | Notification | Matcher: "permission_prompt" |
| 入力待ちなのだ           | waiting.wav       | Notification | Matcher: "idle_prompt"       |
| サブタスクが完了したのだ | subagent_done.wav | SubagentStop | -                            |

## paplay volume指定値

筆者のスピーカーだと77000(120%弱)あたりから音割れが発生します。

音量の出力は、_paplayの--volume値_ x *PCの音量値*で計算されます。
例. `paplay --volume=78643`(120%),PCの音量*50%*の場合、再生される音量はPCの音量基準で60%です。

| 音量 | 引数   |
| ---- | ------ |
| 0%   | 0      |
| 10%  | 6554   |
| 20%  | 13107  |
| 30%  | 19661  |
| 40%  | 26214  |
| 50%  | 32768  |
| 60%  | 39322  |
| 70%  | 45875  |
| 80%  | 52429  |
| 90%  | 58982  |
| 100% | 65536  |
| 110% | 72090  |
| 120% | 78643  |
| 130% | 85197  |
| 140% | 91750  |
| 150% | 98304  |
| 160% | 104858 |
| 170% | 111411 |
| 180% | 117965 |
| 190% | 124518 |
| 200% | 131072 |

## setting.jsonへの追記例

以下のコードを`$HOME/.claude/settings.json`へ追加してください。

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "paplay --volume=76000 $HOME/.claude/voicevox-notification/sounds/done.wav"
          }
        ]
      }
    ],
    "SubagentStop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "paplay --volume=76000 $HOME/.claude/voicevox-notification/sounds/subagent_done.wav"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "permission_prompt",
        "hooks": [
          {
            "type": "command",
            "command": "paplay --volume=76000 $HOME/.claude/voicevox-notification/sounds/permission.wav"
          }
        ]
      },
      {
        "matcher": "idle_prompt",
        "hooks": [
          {
            "type": "command",
            "command": "paplay --volume=76000 $HOME/.claude/voicevox-notification/sounds/notification.wav"
          }
        ]
      }
    ],
}
```

## 東北ずん子・ずんだもんの利用について

[公式ガイドライン](https://zunko.jp/guideline.html)に準拠しています。
