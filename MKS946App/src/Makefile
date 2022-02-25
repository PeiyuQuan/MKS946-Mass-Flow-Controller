TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# Use typed rset structure (see 3.16.1 release notes)
USR_CPPFLAGS += -DUSE_TYPED_RSET

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += MKS946Support

# Compile and add code to the support library
MKS946Support_SRCS += xxxRecord.c
MKS946Support_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application, that
# causes problems on Windows DLL builds
MKS946Support_SRCS += dbSubExample.c
MKS946Support_SRCS += devMKS946Version.c
MKS946Support_SRCS += MKS946Hello.c
MKS946Support_SRCS += initTrace.c

MKS946Support_LIBS += $(EPICS_BASE_IOC_LIBS)

# Auto-generate a header file containing a version string.
# Version comes from the VCS if available, else date+time.
GENVERSION = MKS946Version.h
# Macro name
GENVERSIONMACRO = MKS946VERSION

# Build the IOC application
PROD_IOC = MKS946

# MKS946.dbd will be created and installed
DBD += MKS946.dbd

# MKS946.dbd will include these files:
MKS946_DBD += base.dbd
MKS946_DBD += xxxSupport.dbd
MKS946_DBD += dbSubExample.dbd
MKS946_DBD += devMKS946Version.dbd
MKS946_DBD += MKS946Hello.dbd
MKS946_DBD += initTrace.dbd
MKS946_DBD += asyn.dbd
MKS946_DBD += drvAsynIPPort.dbd
MKS946_DBD += drvAsynSerialPort.dbd
MKS946_DBD += calc.dbd
MKS946_DBD += stream-scalcout.dbd
MKS946_DBD += stream.dbd
MKS946_DBD += as.dbd
MKS946_DBD += asSupport.dbd

# MKS946_registerRecordDeviceDriver.cpp derives from MKS946.dbd
MKS946_SRCS += MKS946_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
MKS946_SRCS_DEFAULT += MKS946Main.cpp
MKS946_SRCS_vxWorks += -nil-

# Link in the code from our support library
MKS946_LIBS += MKS946Support
MKS946_LIBS += asyn
MKS946_LIBS += calc
MKS946_LIBS += autosave
MKS946_LIBS += stream

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into MKS946Support
    sncExample_SNCFLAGS += +r
    MKS946_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    MKS946Support_SRCS += sncExample.stt
    MKS946Support_LIBS += seq pv
    MKS946_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Link QSRV (pvAccess Server) if available
ifdef EPICS_QSRV_MAJOR_VERSION
    MKS946_LIBS += qsrv
    MKS946_LIBS += $(EPICS_BASE_PVA_CORE_LIBS)
    MKS946_DBD += PVAServerRegister.dbd
    MKS946_DBD += qsrv.dbd
endif

# Finally link IOC to the EPICS Base libraries
MKS946_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

# Explicit dependency needed for generated header file
devMKS946Version$(DEP): $(COMMON_DIR)/$(GENVERSION)
