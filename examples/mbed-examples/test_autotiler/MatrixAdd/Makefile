# Copyright (C) 2017 GreenWaves Technologies
# All rights reserved.

# This software may be modified and distributed under the terms
# of the BSD license.  See the LICENSE file for details.

TILER_PATH = $(GAP_SDK_HOME)/tools/autotiler
TEST_C = main.c ModelKernels.c ModelKernelsInit.c BasicKernel.c
MODEL_SRC = MatAddModel.c
RM=rm -f

MBED_FLAGS += -g3
MBED_FLAGS += -mno-memcpy -fno-tree-loop-distribute-patterns
# The generated code outputs a maybe-uninitialized error which is rather difficult to suppress
# in a clean way.
MBED_FLAGS += -Wall -Wno-maybe-uninitialized -Wno-unused-but-set-variable
INC_PATH += -I$(TILER_PATH)/StdTypes -I$(TILER_GENERATOR_PATH)/FFT_Lib

# The double colon allows us to force this to occur before the imported all target
# Link model generation to all step
all:: model

# Build the code generator
GenTile:
	gcc -o GenTile -I$(TILER_PATH)/GenTiling -I$(TILER_PATH)/StdTypes/ $(MODEL_SRC) $(TILER_PATH)/GenTiling/LibTile.a

# Run the code generator
ModelKernels.c ModelKernelsInit.c: GenTile
	./GenTile -m

model: ModelKernels.c ModelKernelsInit.c

clean::
	$(RM) GenTile ModelKernels.* ModelKernelsInit.*

.PHONY: model clean

include $(GAP_SDK_HOME)/tools/rules/mbed_rules.mk


