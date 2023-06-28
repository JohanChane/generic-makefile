# # Generic Makefile for C

# ## Variables
AppName := app
CC := gcc

SRC_DIR := src
OUT_DIR := out

SRC_SUFFIX := .c
OBJ_SUFFIX := .o

# CFLAGS
CINCS = -I$(SRC_DIR)
#CFLAGS += -std=c11 -Wall -O2 -fno-strict-aliasing $(CINCS) 		# for release
CFLAGS += -std=c11 -Wall -g $(CINCS) 														# for debug

# LDFLAGS
LDLIBS = 
LDFLAGS = -L$(SRC_DIR) $(LDLIBS)

APP := $(OUT_DIR)/$(AppName)
SRC_FILES := $(shell find $(SRC_DIR) -type f -name '*$(SRC_SUFFIX)')
OBJ_FILES := $(patsubst $(SRC_DIR)/%,$(OUT_DIR)/%,$(subst $(SRC_SUFFIX),$(OBJ_SUFFIX),$(SRC_FILES)))
DEP_FILES := $(subst $(OBJ_SUFFIX),.d,$(OBJ_FILES))

# ## Rules
.PHONY: all build rebuild install clean cleantarget run debug debug_make

all: cleantarget build install run

run:
	./$(APP)

build: $(APP)

rebuild: clean
	$(MAKE) -f $(firstword $(MAKEFILE_LIST)) build

install: $(APP)
	# install --mode=a+x $(APP) $(APP)
	chmod a+x ./$(APP)

$(APP): $(OBJ_FILES)
	$(CC) $(LDFLAGS) $^ -o $@

cleantarget:
	rm -rf $(APP) 2> /dev/null

clean:
	rm -rf $(OUT_DIR) 2> /dev/null

debug: $(APP)
	gdb ./$(APP)

ifneq ($(MAKECMDGOALS),clean)
-include $(DEP_FILES)
endif

$(OUT_DIR)/%.d:
	@echo Generate $@ ...

	# create the DEP_FILE
	@$(CC) -MM $(CFLAGS) $(SRC_DIR)/$*$(SRC_SUFFIX) | install -D /dev/stdin $@.$$$$; \
	sed 's,.*:,$(subst .d,$(OBJ_SUFFIX),$@):,g' < $@.$$$$ > $@; \
	echo '	$(CC) -c $$(CFLAGS) $(SRC_DIR)/$*$(SRC_SUFFIX) -o $(subst .d,$(OBJ_SUFFIX),$@)' >> $@; \
	echo '$@: $(subst .d,$(SRC_SUFFIX),$@)' >> $@;\
	rm -f $@.$$$$

debug_make:
	@echo $(SRC_FILES)
	@echo $(OBJ_FILES)
	@echo $(DEP_FILES)
