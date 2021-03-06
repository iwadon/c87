# POODLE

**POODLE**は、2014年最後の(11月現在)大きな攻撃である。
SSLv3の仕様に致命的な脆弱性があり、ついにSSLが滅んだ。

## 概要

POODLEは *Padding Oracle On Downgraded Legacy Encryption* の略で、
パディングオラクル攻撃とダウングレード攻撃を組み合わせた攻撃である。
中間者攻撃の一種で、暗号文が解読される。

パディングオラクル攻撃とは、ブロック暗号の解読エラーを利用し、
暗号化されたデータを鍵無しで解読する攻撃方法である。
適当な文字列を暗号文として送りつけた時に、パディングが不正のときエラーを返すとヒントになってしまう。
何度も適当な文字列を送ることにより、パディングの正誤の情報を元に、鍵を知ることなく暗号文を解読できるらしい。
SSLv3を**CBC**というブロック暗号と組み合わせた時にこの攻撃が可能で、平均256回のリクエストで暗号文の1バイトが解読できるらしい。

通常ブラウザはSSLv3より新しい**TLS 1.0, 1.1, 1.2**などを使うが、
TLSでのリクエストに失敗した場合は自動的に古いバージョンのSSLv3を使おうとする。
(中間者である)攻撃者はTLSのリクエストを改竄するなどして、TLS通信を失敗させ、
無理やりSSLv3を使わせることが可能になる。
これをダウングレード攻撃という。

つまり中間者は

1. ダウングレード攻撃で無理やりSSLv3を使わせ
1. SSLv3で通信された暗号文を取得し
1. オラクルパディング攻撃でその暗号を解読する

ことが可能である。

## 対応

各ブラウザベンダの対応は素早かった。
脆弱性が発覚した直後、各ブラウザは**即座にSSLv3を殺した**バージョンをリリースした。
ユーザはすぐにバージョンアップを適応すれば、特に大きな問題はなかったようだ。

サーバ管理者は、SSLv3を無効化しなくてはならない。
多くのWebサーバはデフォルトではSSLv3以降が有効になっている。
脆弱性が発覚したあとも、**IE6**などの古いブラウザがTLSをサポートしていないなどの理由で、
WebサーバはSSLv3をデフォルトで有効にし続けるようだ。
**早くIE6滅んでくれマジで**

ただ、サーバでSSLv3を殺すということは**IE6の接続を拒否する**ということだ。
その他にも古いガラケーブラウザなども拒否される可能性がある。
*あーこまったなー どうしよっかなー こわいからなー 拒否しちゃってもしょうがないよなー(棒)*

やはりSSLv3は確実に殺すべきだ。
きちんと、確実に、容赦なく、殺せ。

## 総括

18年生き続けたSSLv3はついぞ死んだ。
もはやSSLv3をサポートする**まともな**ブラウザはない。
そしてSSLの最終バージョンであるSSLv3が死んだことで、**SSLは完全に終焉を迎えた**。
暗号通信の規格はSSLの後継である**TLS**に完全に移行したが、
セキュリティの代名詞としてSSLの名前は永遠に使われ続けるだろう。

そして!!! なにより!!! IE6が!!! 滅んだ!!!!!!!!!!!
