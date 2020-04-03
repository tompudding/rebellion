ARMCC=arm-none-eabi-gcc
EMUL_DIR=../armv2_emulator
EMUL_SRC_DIR=${EMUL_DIR}/src
LIBC_DIR=${EMUL_SRC_DIR}/libc

ARMCFLAGS  = -Os -std=gnu99 -nostdlib -march=armv2a -Wa,-mapcs-26 -mno-thumb-interwork -marm -Wl,--omagic -I${EMUL_SRC_DIR} -I${LIBC_DIR}
TAPE_NAMES = guessing trivia werewolf adventure
LOADING_SCREENS = trivia werewolf adventure
STATIC_TARGETS = $(addprefix build/, ${TAPE_NAMES})
SHARED_TARGETS = $(addsuffix .so, ${STATIC_TARGETS})
_LOADING_TARGETS = ${addprefix build/, ${LOADING_SCREENS}}
LOADING_TARGETS = ${addsuffix _loading.so, ${_LOADING_TARGETS}}
TAPE_TARGETS = ${addsuffix .tape, ${addprefix tapes/, ${TAPE_NAMES}}}
LIBDIR := ../../build

all: ${SHARED_TARGETS} ${LOADING_TARGETS} ${TAPE_TARGETS}

build/%_loading.so: %_loading.c loading_common.c | build
	${ARMCC} -shared ${ARMCFLAGS} -T linking_loading.txt -o $@ $^

build/%.so: %.c | build
	${ARMCC} -shared ${ARMCFLAGS} -T linking.txt -o $@ $<

#build/adventure.so: build/adventure
#	cp adventure/adventure.so $@

#build/adventure: | build
#	make -C adventure
#	cp adventure/adventure $@

tapes/%.tape: build/%.so | tapes
	python ${EMUL_DIR}/create.py -o $@ dummy_header build/$*.so

tapes:
	mkdir -p $@

build:
	mkdir -p $@

clean:
	make -C adventure clean
	rm -f build/* tapes/*
	rm -df build
	rm -df tapes
