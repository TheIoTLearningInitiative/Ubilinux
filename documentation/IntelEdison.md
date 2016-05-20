# Intel Edison


### Installation

> ubilinux™ is an embedded Linux distribution from Emutex and is based on Debian "Wheezy". It is targeted at embedded devices that have limited memory and storage capabilities.

- [Emutex Labs ubilinux™ Homepage](http://www.emutexlabs.com/ubilinux)
- [Loading Debian (Ubilinux) on the Edison ](https://learn.sparkfun.com/tutorials/loading-debian-ubilinux-on-the-edison)

### Boot Up

```sh
    Debian GNU/Linux 7 ubilinux ttyMFD2

    ubilinux login: edison
    Password: edison
    ...
    edison@ubilinux:~$ su
    Password: edison
    root@ubilinux:/home/edison# date -s "10/22/2015 16:01:00"
    Thu Oct 22 16:01:00 UTC 2015
```

### WiFi

```sh
    edison@ubilinux:~$ su
    Password: edison
    root@ubilinux:/home/edison# cd 
    root@ubilinux:~# nano /etc/network/interfaces
    # interfaces(5) file used by ifup(8) and ifdown(8)
    auto lo
    iface lo inet loopback

    #auto usb0
    #iface usb0 inet static
    #    address 192.168.2.15
    #    netmask 255.255.255.0
    
    auto wlan0
    iface wlan0 inet dhcp
        # For WPA
        wpa-ssid INFINITUMxxxx
        wpa-psk yyyy
        # For WEP
        #wireless-essid itesm
        #wireless-mode Managed
        #wireless-key s:""
    
    root@ubilinux:~# ifup wlan0
    root@ubilinux:~# reboot

    <reboot your board, sign in and become root>
```

## Packages Apt-Get Installation

```sh
    edison@ubilinux:~$ su
    Password: edison
    root@ubilinux:/home/edison# cd 
    root@ubilinux:~# apt-get update
    root@ubilinux:~# apt-get install python-pip
    root@ubilinux:~# apt-get install git build-essential python-dev swig
    root@ubilinux:~# apt-get install python-smbus lm-sensors
    root@ubilinux:~# apt-get install gpsd gpsd-clients python-gps libxml2-dev libxslt1-dev
```

## Packages Python Pip Installation

```sh
    root@ubilinux:~# pip install numpy psutil XBee pyserial pykml LoremIpsum randomwords
    root@ubilinux:~# exit
    edison@ubilinux:~$ 
```

## Packages Manual Installation

### Adafruit BMP180 Python Library

```sh
    edison@ubilinux:~$ cd
    edison@ubilinux:~$ git clone https://github.com/adafruit/Adafruit_Python_BMP.git
    edison@ubilinux:~$ cd Adafruit_Python_BMP
    edison@ubilinux:~/Adafruit_Python_BMP$ su
    Password: edison
    root@ubilinux:/home/edison/Adafruit_Python_BMP# python setup.py install
    root@ubilinux:/home/edison/Adafruit_Python_BMP# exit
    edison@ubilinux:~/Adafruit_Python_BMP$ cd
    edison@ubilinux:~$
```

[Why not using UPM Library?](https://github.com/xe1gyq/eekmex/issues/1)

### Mraa

> Low Level Skeleton Library for IO Communication on GNU/Linux platforms
> C/C++ library with bindings to JavaScript and Python to interface with the I/O on the Intel® Galileo board, Intel® Edison board, and other platforms. With board detection done at runtime, you can create portable code that works across multiple platforms.

- [MRAA Github](https://github.com/intel-iot-devkit/mraa)

```sh
    edison@ubilinux:~$ su
    Password: 
    root@ubilinux:/home/edison# cd
    root@ubilinux:~# apt-get update
    root@ubilinux:~# apt-cache search pcre
    root@ubilinux:~# apt-get install libpcre3-dev git cmake python-dev swig
    root@ubilinux:~# exit
    edison@ubilinux:~$ cd
    edison@ubilinux:~$ git clone https://github.com/intel-iot-devkit/mraa.git
    edison@ubilinux:~$ mkdir mraa/build && cd $_
    edison@ubilinux:~/mraa/build$ cmake .. -DBUILDSWIGNODE=OFF
    edison@ubilinux:~/mraa/build$ make -j3
    edison@ubilinux:~/mraa/build$ su
    Password: edison
    root@ubilinux:/home/edison/mraa/build# make install
    root@ubilinux:/home/edison/mraa/build# nano /etc/ld.so.conf
    include /etc/ld.so.config.d/*.conf
    /usr/local/lib/i386-linux-gnu/
    root@ubilinux:/home/edison/mraa/build# ldconfig
    root@ubilinux:/home/edison/mraa/build# ldconfig -p | grep mraa
    root@ubilinux:/home/edison/mraa/build# nano ~/.bashrc
    ...
    export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    root@ubilinux:/home/edison/mraa/build# exit
    edison@ubilinux:~/mraa/build# nano ~/.bashrc
    export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    edison@ubilinux:~/mraa/build$ export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    edison@ubilinux:~/mraa/build$ cd
    edison@ubilinux:~$ 
```

### Cmake

> Welcome to CMake, the cross-platform, open-source build system. CMake is a family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files. CMake generates native makefiles and workspaces that can be used in the compiler environment of your choice.

```sh
    edison@ubilinux:~$ cd
    edison@ubilinux:~$ wget http://www.cmake.org/files/v3.2/cmake-3.2.2.tar.gz
    edison@ubilinux:~$ tar xvf cmake-3.2.2.tar.gz
    edison@ubilinux:~$ cd cmake-3.2.2
    edison@ubilinux:~/cmake-3.2.2$ ./bootstrap
    edison@ubilinux:~/cmake-3.2.2$ make -j3
    edison@ubilinux:~/cmake-3.2.2$ su
    root@ubilinux:/home/edison/cmake-3.2.2# make install
    root@ubilinux:/home/edison/cmake-3.2.2# cp /usr/local/bin/cmake /usr/bin/cmake
    root@ubilinux:/home/edison/cmake-3.2.2# exit 
    edison@ubilinux:~/cmake-3.2.2$ cd
    edison@ubilinux:~$ 
```

### Upm

> UPM - Sensor/Actuator repository for libmraa. High-level repository for sensors and actuators that use libmraa. In other words, UPM gives you easy function calls to use your sensors, such as reading temperature values or writing data to an LCD screen. With over a hundred sensors and more being added, this library speeds up your development time.

- [UPM Documentation](http://iotdk.intel.com/docs/master/upm/index.html)
- [UPM Sensor Categories](http://iotdk.intel.com/docs/master/upm/modules.html)
- [UPM Github](https://github.com/intel-iot-devkit/upm)
- [UPM Python Exampes](https://github.com/intel-iot-devkit/upm/tree/master/examples/python)

```sh
    edison@ubilinux:~$ cd
    edison@ubilinux:~$ git clone https://github.com/intel-iot-devkit/upm.git
    edison@ubilinux:~$ cd upm
    edison@ubilinux:~/upm$ mkdir build
    edison@ubilinux:~/upm/build$ cd build
    edison@ubilinux:~/upm/build$ export CMAKE_ROOT=/usr/local/share/cmake-3.2
    edison@ubilinux:~/upm/build$ cmake .. -DBUILDSWIGNODE=OFF
    edison@ubilinux:~/upm/build$ make -j3
    edison@ubilinux:~/upm/build$ su
    Password: 
    root@ubilinux:/home/edison/upm/build# make install
    root@ubilinux:/home/edison/upm/build# nano ~/.bashrc
    ...
    export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    root@ubilinux:/home/edison/upm/build# export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    root@ubilinux:/home/edison/upm/build# exit
    edison@ubilinux:~/upm/build$ nano ~/.bashrc
    export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    edison@ubilinux:~/upm/build$ export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages/
    edison@ubilinux:~/upm/build$ cd
    edison@ubilinux:~$ 
```

### RTIMULib

> RTIMULib is the simplest way to connect a 9-dof, 10-dof or 11-dof IMU to an embedded Linux system and obtain quaternion or Euler angle pose data. Basically, two simple function calls (IMUInit() and IMURead()) are pretty much all that's needed to integrate RTIMULib.

```sh
    edison@ubilinux:~$ cd
    edison@ubilinux:~$ git clone https://github.com/richards-tech/RTIMULib.git
    edison@ubilinux:~$ mkdir -p RTIMULib/RTIMULib/build
    edison@ubilinux:~$ cd RTIMULib/RTIMULib/build
    edison@ubilinux:~/RTIMULib/RTIMULib/build$ cmake ..
    edison@ubilinux:~/RTIMULib/RTIMULib/build$ make -j3
    edison@ubilinux:~/RTIMULib/RTIMULib/build$ su
    Password: edison
    root@ubilinux:/home/edison/RTIMULib/RTIMULib/build# make install
    root@ubilinux:/home/edison/RTIMULib/RTIMULib/build# ldconfig
    root@ubilinux:/home/edison/RTIMULib/RTIMULib/build# nano /etc/ld.so.conf
    ...
    /usr/local/lib/
    root@ubilinux:/home/edison/RTIMULib/RTIMULib/build# exit
    edison@ubilinux:~/RTIMULib/RTIMULib/build$ cd ../../Linux/python/
    edison@ubilinux:~/RTIMULib/Linux/python$ python setup.py build
    edison@ubilinux:~/RTIMULib/Linux/python$ su
    Password: edison
    root@ubilinux:/home/edison/RTIMULib/Linux/python# python setup.py install
    root@ubilinux:/home/edison/RTIMULib/Linux/python# exit
    edison@ubilinux:~/RTIMULib/Linux/python$ cd
    edison@ubilinux:~$
```

## Storage

```sh
    edison@ubilinux:~$ su
    Password: 
    root@ubilinux:/home/edison# cd
    root@ubilinux:~# mkdir /media/sdcard/
    root@ubilinux:~# mount /dev/mmcblk1 /media/sdcard/
    root@ubilinux:~# vi /etc/fstab
    /dev/mmcblk1p1 /media/sdcard
```


