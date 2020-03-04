---
description: 32 bit Operating System based on the book「30日でできる! OS自作入門」
---

# OS自作入門

低レイヤーの勉強も兼ねて、最近色々なところで人気の川合秀実さんの「30日でできる！OS自作入門」をやってみようと思いました。このページ自体は自分向けのメモ代わりに使おうと思っているのであまり参考にもならないかもしれませんが、作業の過程や詰まった箇所を記していきます。また初心者なので色々な方々のコードやHPを参考に作ってみているので、どこを参考にしたかも書いておきたいです。

### 作業環境

* MacOS Catalina 10.15.3
* QEMU 4.2.0
* GNU Make 3.81

開発環境を作るにおいて、@noanoa07さんの[『30日でできる！OS自作入門』 for macOS Catalina](https://github.com/noanoa07/myHariboteOS) を参考にさせていただきました。書籍付属の著者作成の独自ツール（tolsetツール）は元々Windows用であり、MacOSでは動きません。そのためMacOS向けの開発環境を用意してくださっている@sandaiさんの[https://github.com/sandai/30nichideosjisaku](https://github.com/sandai/30nichideosjisaku) などを参考にさせていただきましたが、ここにあるMac向けツール「TolsetOSX」をCatalinaで動かすことができなかったので、結局@noanoa07さんが行っていた方法で自分もやってみようかと思いました。ご丁寧に情報をまとめて下さりありがとうございます。ツールの使い方や代替方法は[『30日でできる！OS自作入門』を macOS Catalina で実行する](https://qiita.com/noanoa07/items/8828c37c2e286522c7ee) に沿ってやってみました。

### Day 3 - 9 : C言語導入



### 参考文献

多くの方々のQiita記事やブログ記事、Githubレポを参考にさせて頂きました。自分も早く成長して他の人のためになれるような発信をしていきたいです。

* [「30日でできる！ OS自作入門」をMac向けに環境構築する](https://qiita.com/tatsumack/items/491e47c1a7f0d48fc762)
* \*\*\*\*[sandai](https://github.com/sandai)/[30nichideosjisaku](https://github.com/sandai/30nichideosjisaku)
* \*\*\*\*[noanoa07](https://github.com/noanoa07)/[myHariboteOS](https://github.com/noanoa07/myHariboteOS)
* [『30日でできる！OS自作入門』のメモ](https://vanya.jp.net/os/haribote.html#hrb)
* 
