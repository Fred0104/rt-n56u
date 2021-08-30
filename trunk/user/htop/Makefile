SRC_NAME = htop-3.0.2
SRC_URL = https://github.com/htop-dev/htop/archive/3.0.2.tar.gz
THISDIR = $(shell pwd)

all: download extract config
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download:
	( if [ ! -f $(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.gz $(SRC_URL); \
	fi )

extract:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.gz; \
	fi )

config:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		cd $(SRC_NAME) ; \
		#mkdir -p m4 ; \
		#aclocal; \
		#autoconf; \
		#autoheader; \
		#automake --add-missing; \
		#make configure && touch config_done; \
		sh ./autogen.sh && configure && make; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
		--prefix=/usr \
		--enable-unicode \
		HTOP_NCURSESW_CONFIG_SCRIPT="$(STAGEDIR)/bin/ncursesw6-config" \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)
	
clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/htop /usr/bin/htop
