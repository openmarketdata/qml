include ../mk/pkg.mk

k.h.url = $(Q_DIR)/c/c/k.h
k.h.sha256 = beb5d3c83f46a80c49e2b42bc79c33dc24f1d6dd9665665d6ea32e73c12bb596

$(KXARCH)/q.lib.url = $(Q_DIR)/$(KXARCH)/q.lib
w32/q.lib.sha256 = 05520dab4079d379f18b0f0a8d2c27383975b050dc316ccee1f6c9c67892de81
w64/q.lib.sha256 = 98907c494b982fef150bd0c42775c38bad3ffd435b8001bd30cff0c22280df7d

Q_LIB=$(if $(WINDOWS),../download/$(KXARCH)/q.lib,)

do_extract: ../download/k.h $(Q_LIB)
	mkdir -p work
	cp $< work/
ifneq ($(Q_LIB),)
	cp $(Q_LIB) work/
endif

do_patch:
# Shouldn't need the WIN32 alternatives with gcc, and they conflict with math.h
	$(SEDI) 's/#if.*WIN32.*/#if 0/' work/k.h

ifneq ($(Q_LIB),)
# Using q.lib as -lq appears to work, but crashes q on load.
# Instead, list the symbols in q.lib and construct a new import library.
work/libq.def:
	echo "NAME q.exe"              >$@.tmp
	echo "EXPORTS"                >>$@.tmp
	$(call nm_exports,work/q.lib) >>$@.tmp
	mv $@.tmp $@

work/libq.a: work/libq.def
	$(DLLTOOL) -d $< -l $@

do_build: work/libq.a
endif

do_install:
	cp work/k.h ../include/
ifneq ($(Q_LIB),)
	cp work/libq.a ../lib/
endif

do_clean:
	rm -rf work
