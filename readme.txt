looks like i did not make any notes back then when i built zyn-fusion
for arm, so looks like it works as described here:

  https://github.com/zynaddsubfx/zyn-fusion-build/wiki/Building-on-Linux

but most probably the gcc flags will have to be adjusted:
* armv7l: either "-march=armv7-a -mfpu=vfpv3-d16 -mno-unaligned-access" or "-march=armv7-a -mfpu=neon -mno-unaligned-access" - not sure anymore
* aarch64: "-march=native -mcpu=native"

required packages to build:
apt-get install build-essential git ruby libtool libmxml-dev automake cmake libfftw3-dev libjack-jackd2-dev liblo-dev libz-dev libasound2-dev mesa-common-dev libgl1-mesa-dev libglu1-mesa-dev libcairo2-dev libfontconfig1-dev bison python

and here are some old notes from the raspbian build which i did for trying to use zyn-fusion headless on a pi-zero, which does not really work - but maybe this build is useable on a bigger raspberry pi? (untested)

old raspbian notes:
result: zyn engine works, gui does somehow not work, but would be too slow anyway
        the pi zero is simply too slow to do anything serious with zyn and overclocking is no option for it

https://github.com/zynaddsubfx/zyn-fusion-build/wiki/Building-on-Linux
some notes for building on the rpi: https://lucidbeaming.com/blog/setting-up-a-raspberry-pi-3-to-run-zynaddsubfx-in-a-headless-configuration/

getting ready for jack on the rpi:
/etc/rc.local:
echo "performance" > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
echo "10" > /proc/sys/vm/swappiness

sudo usermod -a -G audio pi

swap to 512 in /etc/dphys-swapfile

$ sudo apt-get install build-essential git ruby libtool libmxml-dev automake cmake libfftw3-dev libjack-jackd2-dev liblo-dev libz-dev libasound2-dev mesa-common-dev libgl1-mesa-dev libglu1-mesa-dev libcairo2-dev libfontconfig1-dev bison
$ git clone https://github.com/zynaddsubfx/zyn-fusion-build
$ cd zyn-fusion-build

# comment out the demo build from build-linux.rb
# in the next step go to zynaddsubfx dir and do "patch -p1 < rpi.patch" as soon as zynaddsubfx was cloned

$ ruby build-linux.rb
$ tar -jxvf zyn-fusion-linux-64bit-3.0.3-patch1-release.tar.bz2
$ cd zyn-fusion
$ sudo bash ./install-linux.sh

proper gcc flags: -march=armv6zk -mcpu=arm1176jzf-s -mfloat-abi=hard -mfpu=vfp
from: https://stackoverflow.com/questions/32446519/correct-architecture-for-the-raspberry-pi-1
to get them: gcc -mcpu=native -march=native -Q --help=target
