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
$ sudo apt install -y libsgx-launch
```
Try ctest. You can find the result of [ctest](https://github.com/developer-onizuka/Intel_SGX-1/blob/main/result_ctest.txt).
```
$ source /usr/local/share/openenclave/openenclaverc
$ ctest
Test project /mnt/sgx/openenclave/build
        Start   1: samples
        ... snip ...
	100% tests passed, 0 tests failed out of 760

Total Test time (real) = 675.24 sec
```

# 5. Test HelloWorld
```
$ cd ../samples/helloworld/
$ make
$ make run
host/helloworldhost ./enclave/helloworldenc.signed
Hello world from the enclave
Enclave called into host to print: Hello World!
```

