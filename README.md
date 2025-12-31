#使い方とリンカについて
gcc -c mylib.c(リンカーなしでoファイルを作成)
ar rcs libmylib.a mylib.o(作ったファイルをライブラリファイルにする)
gcc main.c -L. -lmylib(gcc main.c -Lどこにライブラリがあるかのパス -lライブラリ名)
↑
リンカはoファイル同士を紐づけたり、ライブラリを紐づけたり