---
published: true

title : clear linux 한글 설정
layout: single
tag: [clear linux]
toc: true
katex: True

---

clear linux 에서 한글 키보드 입력방법을 설명합니다.

아래 커맨드를 terminal창에서 실행하면됩니다.

-

```bash
sudo swupd bundle-add  devpkg-expat
sudo swupd bundle-add  desktop-dev
export PKG_CONFIG_PATH=/usr/lib/pkgconfig
sudo ldconfig
git clone https://github.com/libhangul/libhangul
cd libhangul
./autogen.sh
sudo ./configure -prefix=/usr
sudo make
sudo  make install
cd ..
git clone https://github.com/libhangul/ibus-hangul
cd ibus-hangul
./autogen.sh
./configure -prefix=/usr
sudo make
sudo make install
sudo ldconfig
cd /usr/bin
./ibus-setup-hangul
```
