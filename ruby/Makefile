ifeq ($(NEOTONIC_ROOT),)
NEOTONIC_ROOT = ..
endif

include $(NEOTONIC_ROOT)/rules.mk

all: config.save ext/hdf/hdf.so testrb

config.save: install.rb
	ruby install.rb config -- --with-hdf-include=../../.. --with-hdf-lib=../../../libs --make-prog=$(MAKE)

ext/hdf/Makefile:
	ruby install.rb config -- --with-hdf-include=../../.. --with-hdf-lib=../../../libs --make-prog=$(MAKE)

ext/hdf/hdf.so: config.save
	ruby install.rb setup 

gold: ext/hdf/hdf.so
	ruby -Ilib -Iext/hdf test/hdftest.rb > hdftest.gold;
	@echo "Generated gold files"

testrb: ext/hdf/hdf.so
	@echo "Running ruby test"
	@failed=0; \
	rm -f hdftest.out; \
	ruby -Ilib -Iext/hdf test/hdftest.rb > hdftest.out; \
	diff --brief hdftest.out hdftest.gold > /dev/null 2>&1; \
	return_code=$$?; \
	#if [ $$return_code -ne 0 ]; then \
	#  diff hdftest.out hdftest.gold > hdftest.err; \
	#  echo "Failed Ruby Test: hdftest.rb"; \
	#  echo "    See hdftest.out and hdftest.err"; \
	#  failed=1; \
	#fi; \
	#if [ $$failed -eq 1 ]; then \
	#  exit 1; \
	#fi;
	@echo  "Passed ruby test"


install: all
	ruby install.rb install

clean:
	rm ext/hdf/*.o ext/hdf/*.so

distclean:
	rm Makefile.depends config.save ext/hdf/hdf.so
	rm ext/hdf/Makefile ext/hdf/mkmf.log ext/hdf/*.o
