#Building Guide to Bringup Rom 

# You need to have a linux distro with atleast 8 cores and atleast 12 gb ram and 130 gb of HDD.

#Setting up linux distro 

*For setting up linux distro we need to install some packages into our system for that , copy paste the preceeing code into your linux distro*

```sudo apt-get update && sudo apt-get upgrade```

/*This will install the latest files of your linux distro*/

**Then to add those packages necessary for building , copy paste the preceding code**

```sudo apt-get install bc bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline6-dev lib32z1-dev libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev openjdk-8-jdk```

#Now we need to install repo.

```sudo apt-get install repo```

#Now lets configure you github username and email globally.

```git config --global user.name "FIRST_NAME LAST_NAME"```
```git config --global user.email "YOUR EMAIL"```

#Lets make a directory where we will intialise our rom source
``` mkdir lineage``` 
#It will make a directory named lineage

#Now Lets sync a Source into the lineage directory we have made
```cd lineage```
```repo init -u "CLONE_URL" -b "BRANCH"```
#For example lets clone Lineage Source for Pie.
#So the Branch will be lineage-16.0 for oreo it will be lineage-15.1.
```repo init -u git://github.com/LineageOS/android.git -b lineage-16.0```

#Now To sync the Source.

```repo sync -j$(nproc --all)```

#Wait for the sync to finish ,it takes time.

#Now we have to Clone Device tree, Vendor blobs and kernel Source, Search on github for your own device.
#For example I am taking daisy (Mi A2 Lite)

#Lets clone device tree

```git clone https://github.com/HachimanH20/android_device_xiaomi_daisy.git -b lineage-16.0 device/xiaomi/daisy```
#Here the branch is lineage-16.0 and location is device/xiaomi/daisy where the device tree will bs cloned

#Lets clone vendor blobs

```git clone https://github.com/HachimanH20/vendor_xiaomi_daisy.git -b pie vendor/xiaomi/daisy```
#Here the branch is pie and clone path is vendor/xiaomi/daisy

#Lets clone Kernel Source

```git clone https://github.com/HachimanH20/Caf_Kernel_Daisy.git -b Caf-kernel kernel/xiaomi/daisy```
#Here the branch is Caf-kernel and the clone path is kernel/xiaomi/daisy

#Now after cloning device tree , vendor blobs and kernel source we need to edit some Make files in Our devicetree in order to build the Rom
#Make files which we need to edit are AndroidProducts.mk , lineage_daisy.mk(prefix differs from source to source)
#we also need to edit vendorsetup.sh and the dependencies.
#First lets go to the device tree directory where we have cloned it

```cd device/xiaomi/daisy```

#Now lets edit the AndroidProducts.mk 

```nano AndroidP*``` 
#you can see i didnt write the whole name of the file and also added a * at the suffix , well the main use of this is to reduce the writing, as it will automatically find the file you are targetting it also works to open directories

#Now we have to rename the 

```$(LOCAL_DIR)/mk_daisy.mk``` to ```$(LOCAL_DIR)/lineage_daisy.mk```
#save and exit i.e., ctrl + X then press y and then enter.

#Now we have to rename 

```mk_daisy.mk``` to ```lineage_daisy.mk```
#save and exit

#We need to edit the lineage_daisy.mk makefile

```nano lineage_daisy.mk```
#Here we need to edit some specific lines 
```$(call inherit-product, vendor/mk/config/common_full_phone.mk)``` to ```$(call inherit-product, vendor/lineage/config/common_full_phone.mk)```

```PRODUCT_NAME := mk_daisy``` to ```PRODUCT_NAME := lineage_daisy```
#save and exit 

#Now go Back to the Rom directory 
```cd ../../../``` or ``` cd ~ && cd lineage``` 

**And start the build**
 
``` . build/envsetup.sh``` 
```lunch lineage_daisy-userdebug```
``` brunch tulip``` 

/** your rom should be compiling , this is a simple guide for building a custom rom from source*/