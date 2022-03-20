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

$ sudo dmesg |grep -i sgx
[   56.723882] isgx: loading out-of-tree module taints kernel.
[   56.723902] isgx: module verification failed: signature and/or required key missing - tainting kernel
[   56.724097] intel_sgx: Intel SGX Driver v2.11.0
[   56.724112] intel_sgx INT0E0C:00: EPC bank 0xc0200000-0xc5f80000
[   56.737948] intel_sgx:  can not reset SGX LE public key hash MSRs
[   56.737996] intel_sgx: second initialization call skipped
[   59.042970] Modules linked in: snd_hda_codec_hdmi nls_iso8859_1 intel_rapl_msr intel_rapl_common snd_hda_codec_realtek snd_hda_codec_generic mei_hdcp x86_pkg_temp_thermal ledtrig_audio intel_powerclamp coretemp snd_hda_intel kvm_intel snd_intel_dspcfg kvm snd_hda_codec snd_hda_core crct10dif_pclmul snd_hwdep crc32_pclmul snd_pcm ghash_clmulni_intel aesni_intel crypto_simd cryptd glue_helper rapl intel_cstate snd_seq_midi snd_seq_midi_event dell_wmi dell_smbios dcdbas nouveau i915 snd_rawmidi dell_wmi_descriptor sparse_keymap wmi_bmof mxm_wmi snd_seq ttm drm_kms_helper input_leds snd_seq_device drm joydev snd_timer i2c_algo_bit snd fb_sys_fops syscopyarea soundcore mei_me sysfillrect mei sysimgblt mac_hid acpi_pad sch_fq_codel isgx(OE) parport_pc ppdev lp parport ip_tables x_tables autofs4 hid_generic usbhid hid mlx4_ib ib_uverbs mlx4_en ib_core nvme e1000e mlx4_core ahci nvme_core libahci wmi video
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

