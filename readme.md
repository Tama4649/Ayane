﻿# Ayaneとは？

Ayaneとは、python Adaptor to YaneuraOuの略で、やねうら王をpythonから呼び出して便利に使えてしまうアダプターです。
- ※　AdaptorのAと、YaneuraOuのYaneの部分でAyaneです。また、YaneuraOuのYane-r--uの部分を取って、Ayaneru(あやねる)の愛称で呼ぶことがあります。

python側で少しコードを書くだけで棋譜解析や並列自己対局などができます。

USIプロトコルのエンジンであれば他のエンジンでもpythonから呼び出して使えますが、USIプロトコルでは規定されていない拡張を使いたいので、やねうら王以外のサポートはしません。

動作を確認しているpythonは3.7系です。それ以前のpythonの動作確認はしていません。

# 使い方

```python
import shogi.Ayane

# 通常探索用の思考エンジンの接続テスト
# 同期的に思考させる。
def test_ayane():

    # エンジンとやりとりするクラス
    usi = ayane.UsiEngine()

    # デバッグ用にエンジンとのやりとり内容を標準出力に出力する。
    # usi.debug_print = True

    # エンジンオプション自体は、基本的には"engine_options.txt"で設定する。(やねうら王のdocs/を読むべし)
    # 特定のエンジンオプションをさらに上書きで設定できる
    usi.set_options({"Hash":"128","Threads":"4","NetworkDelay":"0","NetworkDelay2":"0"})

    # エンジンに接続
    # 通常の思考エンジンであるものとする。
    usi.connect("exe/YaneuraOu.exe")

    # 開始局面から76歩の局面
    # ※　"position"コマンド、"go"コマンドなどについては、USIプロトコルの説明を参考にしてください。
    # cf.「USIプロトコルとは」: http://shogidokoro.starfree.jp/usi.html
    usi.usi_position("startpos moves 7g7f")

    # multipv 4で探索させてみる
    # 2秒思考して待機させる。
    usi.send_command("multipv 4")
    usi.usi_go_and_wait_bestmove("btime 0 wtime 0 byoyomi 2000")

    # 思考内容を表示させてみる。
    print("=== UsiThinkResult ===\n" + usi.think_result.to_string())

    # エンジンを切断
    usi.disconnect()

if __name__ == "__main__":
    test_ayane()
```

ね、簡単でしょう？

- Ayaneを使えば、エンジン同士の対局をさせるのも、わずかなコードで実現できます。
  - [unit-test.py](source/unit_test1.py)

- sfen文字列の取扱いなどは、下記のライブラリを使うと便利だと思います。
  - [python-shogi](https://github.com/gunyarakun/python-shogi)


# ニュース記事

- 2019/06/28 : [pythonからやねうら王を駆動できるアダプターAyane、公開しました](http://yaneuraou.yaneu.com/2019/06/28/python%e3%81%8b%e3%82%89%e3%82%84%e3%81%ad%e3%81%86%e3%82%89%e7%8e%8b%e3%82%92%e9%a7%86%e5%8b%95%e3%81%a7%e3%81%8d%e3%82%8b%e3%82%a2%e3%83%80%e3%83%97%e3%82%bf%e3%83%bcayane%e3%80%81%e5%85%ac%e9%96%8b/)


# Ayaneru-server(あやねるサーバー)

自己対局のための補助クラス。
鋭意製作中。


# Ayaneru-gate(あやねるゲート)

Ayaneを使った複数ソフト間の対局を自動化するスクリプト。
近日公開予定。


# License

Apache License Version 2.0とします。
