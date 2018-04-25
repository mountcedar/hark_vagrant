## Getting Started

このドキュメントは、HARK HACKATHONのためにvagrantでHARKの簡易環境を作るためのガイダンスである。
対象となるユーザは、Mac OSXを使っているユーザである。Ubuntu, Windowsのユーザはそれぞれインストール
可能であるため、直接、ホストOSにソフトウェアをインストールすることをオススメする。

### 準備

Vagrantとは、開発環境の簡易構築のためのツールで、Virtualbox, AWSなどどこの仮想環境でも同じ環境を
構築するためのツールである。ここでは、UbutuベースのHARK環境を構築するために使う。

Vagrantのインストール方法は無数にあるが、コマンドラインからインストールするには、
[Homebrew](https://brew.sh/index_ja.html)を使う。

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew tap caskroom/cask
```

HomebrewはLinuxレガシーなソフトウェアのインストールに特化しているが、Homebrew Caskを導入することで、
Mac用のソフトウェアのインストールにも対応する。

```
$ brew cask install virtualbox
$ brew cask install vagrant
$ brew cask install xquartz
```

上記では、仮想環境実行ソフトのVirtualbox, vagrant, そして、仮想環境で実行したソフトウェアをMac上で操作するための
ソフトであるXQuartzをインストールしている。

## Vagrant環境の構築

vagrantの環境を構築する。
まず、HARK環境の構築用のディレクトリを作成し、そのディレクトリに移動する。

```
$ mkdir harkenv
$ cd harkenv
```

次に、Vagrantfileを作成する。

```Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "harkhackathon/ubuntu"
  config.vm.box_version = "0.0.1"  
  config.vm.network "public_network"
  config.ssh.forward_x11 = true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'ESP', '--vendorid', '0x23a7', '--productid', '0x0010']
  end
end
```

準備が整ったら、

```
$ vagrant up
```

を実行すると、HARK pre-install済みの環境がダウンロードされ、Virtualbox上に展開される。
仮想環境へのアクセスは、

```
$ vagrant ssh
```

でログインし、

```
$ audacity
```

などと実行すると、XServer経由で、Xアプリケーションが立ち上がる。
