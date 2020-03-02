---
description: Notes for nnn online courses.
---

# nnn

VagrantとVirtualboxを使う  
MyVagrant/ubuntu64\_18にbionic64をインストール

```bash
vagrant box add ubuntu/bionic64
vagrant init ubuntu/bionic64
vagrant up
vagrant ssh

sudo apt update
sudo timedatectl set-timezone Asia/Tokyo

sudo apt install bsdgames
tetris-bsd

exit
vagrant halt

```

```bash
# Show Hardware
sudo lshw -short

# Show storage and data usage
df
```

#### Vagrantの共有フォルダ

```bash
cd ~/MyVagrant/ubuntu64_18
mkdir workspace

vi VagrantFile
```

{% code title="VagrantFile" %}
```bash
config.vm.synced_folder "./workspace", "/home/vagrant/workspace"

config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
end
```
{% endcode %}

```bash
vagrant reload
vagrant ssh

cd ~
mkdir workspace/tmp # The same folder should appear on Mac.
```





