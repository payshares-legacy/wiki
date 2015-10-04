# Building boost 1.55 and using multiple versions with scons for Payshares

```
wget -O boost_1_55_0.tar.gz http://sourceforge.net/projects/boost/files/boost/1.55.0/boost_1_55_0.tar.gz/download
tar xzvf boost_1_55_0.tar.gz
cd boost_1_55_0/
./bootstrap.sh
./b2 stage
cd ..
```

Edit SConstruct and specify the correct location of the boost install:

```
BOOST_HOME = '/opt/boost_1_55_0'
```

Add --static flag around line 362

Voila!
