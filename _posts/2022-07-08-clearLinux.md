---
published: true

title : clear linux 한글 설정
layout: single
tag: [clear linux]
toc: true
katex: True

---

clear linux는 기본적으로 한글을 제공하지않습니다.

아래 커맨드로 한글 입력 가능합니다. 

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
