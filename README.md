# Intel_SGX-1

# 1. Install Ubuntu18.04
```
$ sudo dmidecode |grep GHz
	Version: Intel(R) Core(TM) i5-7500 CPU @ 3.40GHz
$ sudo dmesg |grep -i sgx
(No result)
```

# 2. apt Update and Upgrade
```
$ sudo apt update && sudo apt upgrade -y
$ reboot
$ uname -r
5.4.0-104-generic
```

# 3. Install git
```
$ sudo apt install -y git
```

# 4. Install OpenEnclave
```
$ git clone https://github.com/openenclave/openenclave
$ cd openenclave/
$ sudo scripts/ansible/install-ansible.sh
$ sudo ansible-playbook scripts/ansible/oe-contributors-setup-sgx1.yml
```
You can find isgx driver in /dev.
```
$ ls /dev |grep sgx
isgx
```
```
$ mkdir build
$ cd build 
$ git submodule update --recursive --init
$ cmake -DHAS_QUOTE_PROVIDER=OFF ..
$ make -j4
$ cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/usr/local ..
$ sudo make install
$ ctest
Test project /mnt/sgx/openenclave/build
        Start   1: samples
        ...
        
```

# 5. Test it
```


```

