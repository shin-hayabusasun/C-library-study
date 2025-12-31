# C言語ライブラリの使い方とリンカについて

## ライブラリ作成の手順

1. オブジェクトファイルを作成（コンパイルのみ、リンクなし）  
```
gcc -c mylib.c
```
mylib.c をコンパイルして mylib.o を作成
まだ実行可能ファイルは作られない

2. 静的ライブラリを作成  
```
ar rcs libmylib.a mylib.o
```
mylib.o をまとめて libmylib.a という静的ライブラリファイルにする
libmylib.a がライブラリ本体

---

## ライブラリを使う C ファイルのコンパイル
```
gcc main.c -L. -lmylib

```
-L. : ライブラリのあるパス（ここではカレントディレクトリ）  
-lmylib : libmylib.a をリンクする  
リンカが .o ファイル同士やライブラリを紐づけて、実行可能バイナリを作る

---

## 使用する際のポイント

1. ヘッダファイル (.h) を用意  
```
/* mylib.h */
int add(int a, int b);

```
これによって、libmylib.aのバイナリの中身をmainから見れるように

2. C ファイルでヘッダをインクルード  
```
#include "mylib.h"

int main() {
int x = add(2, 3);
return 0;
}

```
mainからでも見れる.hファイルをここでインポートすることで、リンカ時にlibmylib.aとmylib.hを使用できる。
libmylib.aとmylib.hは同じディレクトリに

3. コンパイル＆リンク  
上記の gcc main.c -L. -lmylib でリンク時にリンカが libmylib.a と main.o を結びつける  
その結果、main がライブラリ関数を呼べるようになる
gcc -c mylib.c(リンカーなしでoファイルを作成) 
ar rcs libmylib.a mylib.o(作ったファイルをライブラリファイルにする)
gcc main.c -L. -lmylib(gcc main.c -Lどこにライブラリがあるかのパス -lライブラリ名)

---

### まとめ

.o ファイルは「コンパイル済みの部品」  
ar で .a にまとめるとライブラリになる  
リンカ（gcc のリンク時処理）が .o や .a を結びつけて実行可能バイナリを作る  
ヘッダファイルはコンパイル時に関数の宣言を知らせるために必要
oファイル同士のリンカ使用もある。例えば、2つのファイルでCを作ったときやinit.sとmain.cの組み込みの時