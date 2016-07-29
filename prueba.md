#Hexapod-robot

This repository contains the components, to control the robot phantom-x.

First, you should clone the robocomp repository and install in your pc. As indicated in the link:

	https://github.com/robocomp/robocomp/blob/master/README.md

Once already installed robocomp, you should clone this repository in your pc, with the next line:

	https://github.com/ibarbech/hexapod-robot.git

To run the components, you should have a phantom-x with a odroid, then,  I explain the steps are set to build the phantom-x

##Make hexapod with phantom-x

###Initial Setup Odroid

In this chapter we are going to explain how setup the environment that we need to run our components.

Materials:

* Odroid c1+

* sd memory card class A

* hdmi cable

* rj45 cable

The first step is downloading the necessary software to prepare the memory card. We used SD Formatter 4.0, available on https://www.sdcard.org/.

![SD Formatter](img/SD_formatter.jpg)

Only select the memory card path and select format.

Now, we are going to burn a odroid image on our Odroid C1. We can find the image on http://odroid.com/ and then we’ll burn it on the memory card with win32DiskImager.

![Win 32 Disk Imager](img/Win_32_Disk_Imager.jpg)

When the process is finished, we insert the memory in the Odroid C1 and we’ll connect all.

####First setup

This O.S. is similar to another Linux distribution that you could install in your personal computer. 

Firstly, we are going to install robocomp, an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces.

#####Installing Robocomp

Requeriments:

	$ sudo apt-get update

	$ sudo apt-get install git git-annex cmake g++ libgsl0-dev libopenscenegraph-dev cmake-qt-gui zeroc-ice35 freeglut3-dev libboost-system-dev libboost-thread-dev qt4-dev-tools yakuake openjdk-7-jre python-pip  python-pyparsing python-numpy python-pyside pyside-tools libxt-dev pyqt4-dev-tools qt4-designer libboost-test-dev libboost-filesystem-dev

#####Installation of Robocomp

We followed the indications that provide from robocomp Github. 

	https://github.com/robocomp/robocomp


#####Installing Dynamixel SDK

Steps:
	
	$ sudo apt-get update && sudo apt-get upgrade
	$ sudo apt-get install libusb-dev
	$ mkdir DinamixelSDK && cd DinamixelSDK
	$ wget http://support.robotis.com/en/baggage_files/dynamixel_sdk/dxl_sdk_linux_v1_01.zip
	$ unzip dxl_sdk_linux_v1_01.zip
	$ cd src
	$ make
	$ cp libdxl.a /lib/libdxl.a

#####Installing PyDinamixel

Steps

	$ sudo pip install pyserial
	$ cd
	$ mkdir software
	$ cd software
	$ git clone https://github.com/richard-clark/pydynamixel
	$ cd pydynamixel/
	$ sudo python setup.py install

You can check that PyDynamixel is installed writting this commands:

	$ python
	>>> from pydynamixel import dynamixel

We’ll know if all it’s ok if we don’t see any kind of error.

#####Creating Component

Once we get installed pydynamixel we are going to create the component that we will use to implement JointMotor interface.

We need to have installed Robocomp to make the component in an easy way. 

Steps:

- In the terminal we write:

	$ robocompcdsl DynamixelComponent

- We open the file with an editor and we have to include the JointMotor interface, located in robocomp/files/interfaces/ We write the name of our component which implements, no requires, JointMotor. We have installed pydynamixel, a python library, so our component will be code in python. To do this, we only have to modify the language tag. If we don’t need an UI, we could delete the line implements Qt.

- Once we have modify the file, we have to write in the terminal:

	$ robocompcdsl DynamixelComponent .

This process will be take some time.

- The component will be over, when we’ll gain access to the terminal .

#####How should you use this component?

Our component implements JointMotor so it has all methods and functions described in the interface. This interfaces is located in /robocomp/files/interfaces and you could see how works if you see our component.

The first thing you should do if you want to use it will be requires it when you are creating your component. In the part where the component is requiring or implementing other components, you have to requires JointMotor, and import the correspondent idsl.

Once you are writing you code, you only have to call the proxy and use its methods, like setSyncPosition or GetAllMotorState.

The component’s timer is set in 100 milliseconds and in every iteration will read the state of each motor in hexapod.
