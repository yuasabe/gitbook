---
description: 32 bit Operating System based on the book「30日でできる! OS自作入門」
---

# OS自作入門

低レイヤーの勉強も兼ねて、最近色々なところで人気の川合秀実さんの「30日でできる！OS自作入門」をやってみようと思いました。このページ自体は自分向けのメモ代わりに使おうと思っているのであまり参考にもならないかもしれませんが、作業の過程や詰まった箇所を記していきます。また初心者なので色々な方々のコードやHPを参考に作ってみているので、どこを参考にしたかも書いておきたいです。また、勉強中の段階なので色々間違ったことや不十分な説明もあるかもしれませんが、そのときは教えてもらえると本当に喜びます。

### 作業環境

* MacOS Catalina 10.15.3
* QEMU 4.2.0
* GNU Make 3.81

開発環境を作るにおいて、@noanoa07さんの[『30日でできる！OS自作入門』 for macOS Catalina](https://github.com/noanoa07/myHariboteOS) を参考にさせていただきました。書籍付属の著者作成の独自ツール（tolsetツール）は元々Windows用であり、MacOSでは動きません。そのためMacOS向けの開発環境を用意してくださっている@sandaiさんの[https://github.com/sandai/30nichideosjisaku](https://github.com/sandai/30nichideosjisaku) などを参考にさせていただきましたが、ここにあるMac向けツール「TolsetOSX」をCatalinaで動かすことができなかったので、結局@noanoa07さんが行っていた方法で自分もやってみようかと思いました。ご丁寧に情報をまとめて下さりありがとうございます。ツールの使い方や代替方法は[『30日でできる！OS自作入門』を macOS Catalina で実行する](https://qiita.com/noanoa07/items/8828c37c2e286522c7ee) に沿ってやってみました。

作業中のレポは[yuasabe](https://github.com/yuasabe)/[**myos**](https://github.com/yuasabe/myos) ****にあります。

### Day 3 - 9 : ついにC言語導入へ

bootpack.cをコンパイルするために[https://vanya.jp.net/os/haribote.html\#hrb](https://vanya.jp.net/os/haribote.html#hrb) にある「OS用リンクスクリプト」を使わせて頂きました。このファイルをhrd.ldとして作成し、これをリンクしてコンパイルしました。コンパイルするには、リンクスクリプトを-Tで指定できるよう[i386-elf-gcc](https://github.com/nativeos/homebrew-i386-elf-toolchain)を使いました。

```text
# i386-elf-gccをhomebrewでインストール
brew tap nativeos/i386-elf-toolchain
brew install i386-elf-binutils i386-elf-gcc
i386-elf-gcc --version
```

この時点でのMakefileの中身を解説しておきます。

{% code title="Makefile" %}
```bash
default:
	make img

# Initial Program Loader (IPL) をアセンブル
# FDの10シリンダ分だけを読み込んである
ipl10.bin : ipl10.nas Makefile
	nasm ipl10.nas -o ipl10.bin -l ipl10.lst

# OSの最初の共通部分のみを切り出したものをアセンブル
asmhead.bin : asmhead.nas Makefile
	nasm asmhead.nas -o asmhead.bin -l asmhead.lst

# リンクスクリプト（hrb.ld）をもとにbootpack.cをコンパイル
bootpack.hrb : bootpack.c hrb.ld Makefile
	# Compile C file using linker script
	i386-elf-gcc -march=i486 -m32 -nostdlib -T hrb.ld bootpack.c -o bootpack.hrb

# 2つのバイナリファイルを合わせてharibote.sysを生成
haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin bootpack.hrb > haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	mformat -f 1440 -C -B ipl10.bin -i haribote.img ::
	mcopy -i haribote.img haribote.sys ::
	# -f : specifies the size of the DOS file system
	# -C : creates the disk image file to install the MS-DOS file system on it.
	# -B : Use the boot sector stored in the given file or device, instead of using its own. 
	#      Only the geometry fields are updated to match the target disks parameters.

img :
	make -r haribote.img

run :
	make -r img
	qemu-system-i386 -drive file=haribote.img,format=raw,if=floppy -boot a

clean:
	rm *.bin
	rm *.lst
	rm *.sys
	rm *.hrb

```
{% endcode %}

これでmake run すると下図のような黒い画面が表示されます。

![](.gitbook/assets/screen-shot-2020-03-04-at-13.34.06.png)

### Day 3-10 : とにかくHLTしたい

naskfunc.nasを作成し、io\_hlt関数を新しく定義する。アセンブリの中身はHLTとRETをするだけです。bootpack.cでio\_hlt関数を使えるように以下のように更新する。

{% code title="bootpack.c" %}
```c

extern void io_hlt(void);

void HariMain(void) {

	fin:
		io_hlt();
		goto fin;

}
```
{% endcode %}

Makefileでnaskfunc.oを新しくelf形式でアセンブルし、bootpack.hrbをコンパイルする際にリンクしました。make runして同じく黒い画面が表示されれば成功です。本の通りにやるとエラーでmakeできなかった箇所があったので[30日OS自作入門-3日目（C言語導入）-](https://motojiroxx.hatenablog.com/entry/2018/06/11/004414) などを参考にさせて頂きました。



### 参考文献

多くの方々のQiita記事やブログ記事、Githubレポを参考にさせて頂きました。自分も早く成長して他の人のためになれるような発信をしていきたいです。

* [「30日でできる！ OS自作入門」をMac向けに環境構築する](https://qiita.com/tatsumack/items/491e47c1a7f0d48fc762)
* \*\*\*\*[sandai](https://github.com/sandai)/[30nichideosjisaku](https://github.com/sandai/30nichideosjisaku)
* \*\*\*\*[noanoa07](https://github.com/noanoa07)/[myHariboteOS](https://github.com/noanoa07/myHariboteOS)
* [『30日でできる！OS自作入門』のメモ](https://vanya.jp.net/os/haribote.html#hrb)
* [30日OS自作入門-3日目（C言語導入）-](https://motojiroxx.hatenablog.com/entry/2018/06/11/004414)
* [https://wiki.osdev.org/GCC\_Cross-Compiler](https://wiki.osdev.org/GCC_Cross-Compiler)
* [NASM - The Netwide Assembler](https://www.nasm.us/doc/nasmdoc0.html)

