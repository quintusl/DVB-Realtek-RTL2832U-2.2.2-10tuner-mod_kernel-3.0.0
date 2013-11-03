# License: GPL
# RTL2832U Driver

# `uname -r` can be 3.7-Comment or 3.7.3-comment ...
DETECT_KERNEL_MACRO := case `uname -r` in \
      3.15*) echo "ERROR";; \
      3.14*) echo "ERROR";; \
      3.13*) echo "ERROR";; \
      3.12*) echo "3110";; \
      3.11*) echo "3110";; \
      3.10*) echo "390";; \
      3.9*)  echo "390";; \
      3.8*)  echo "360";; \
      3.7*)  echo "360";; \
      3.6*)  echo "360";; \
      3.5*)  echo "350";; \
      3.4*)  echo "340";; \
      3.3*)  echo "330";; \
      3.2*)  echo "320";; \
      3.1*)  echo "300";; \
      3.0*)  echo "300";; \
      *)     echo "ERROR";; \
      esac

# Choose here which include file to use: from autodetect or manually
INCLUDE_EXTRA_DVB := include-$(shell $(DETECT_KERNEL_MACRO))

# ----------------------------------------

# current dir
SOURCEDIR := $(PWD)
KERNVER=`uname -r`
DISTRO:=$(shell test -f /etc/redhat-release && echo 'redhat' || echo 'other')

dvb-usb-rtl2832u-objs := demod_rtl2832.o dvbt_demod_base.o dvbt_nim_base.o \
	foundation.o math_mpi.o nim_rtl2832_mxl5007t.o nim_rtl2832_fc2580.o \
	nim_rtl2832_mt2266.o rtl2832u.o rtl2832u_fe.o rtl2832u_io.o \
	tuner_mxl5007t.o tuner_fc2580.o tuner_mt2266.o tuner_tua9001.o \
	nim_rtl2832_tua9001.o tuner_fc0012.o nim_rtl2832_fc0012.o demod_rtl2836.o \
	dtmb_demod_base.o dtmb_nim_base.o nim_rtl2836_fc2580.o 	nim_rtl2836_mxl5007t.o \
	tuner_e4000.o nim_rtl2832_e4000.o tuner_mt2063.o demod_rtl2840.o \
	tuner_max3543.o nim_rtl2832_mt2063.o nim_rtl2832_max3543.o \
	nim_rtl2840_mt2063.o nim_rtl2840_max3543.o qam_demod_base.o qam_nim_base.o \
	tuner_tda18272.o nim_rtl2832_tda18272.o rtl2832u_ioctl.o nim_rtl2832_fc0013.o \
	tuner_fc0013.o

obj-m += dvb-usb-rtl2832u.o

EXTRA_CFLAGS += -I$(KBUILD_SRC)/drivers/media/dvb/dvb-usb/ \
		-I$(KBUILD_SRC)/drivers/media/dvb/dvb-core/ \
		-I$(KBUILD_SRC)/drivers/media/dvb/frontends/ \
		-I$(KBUILD_SRC)/drivers/media/common/tuners/ \
		-I$(SUBDIRS)/$(INCLUDE_EXTRA_DVB) \
		-D__DVB_CORE__

KINS = /lib/modules
ifeq ($(DISTRO),redhat)
KDIR = /usr/src/kernels/$(KERNVER)
else
KDIR = /usr/src/linux-headers-$(KERNVER)
endif

default:
	make -C $(KDIR) SUBDIRS=$(PWD) modules

install:
	mkdir -p $(KINS)/$(KERNVER)/kernel/drivers/media/dvb/dvb-usb/
	cp dvb-usb-rtl2832u.ko $(KINS)/$(KERNVER)/kernel/drivers/media/dvb/dvb-usb/
	depmod -a

clean:
	-rm -f  *.o  *.ko *.mod.c .*.o.cmd  .*.o.d  .*.ko.cmd Module.symvers Module.markers modules.order
	-rm -rf .tmp_versions
