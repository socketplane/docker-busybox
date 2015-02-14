FROM debian:jessie
# This will make it easier to upgrade to the next version of buildroot.
# You might have to also update the busybox version at the end of this file!
ENV BUILDROOT_VERSION 2015.02-rc1
# This will get rid of a build warning related to 'tput'.
ENV TERM dumb
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
RUN apt-get update -q
RUN apt-get install -q -y wget \
	build-essential \
	libncurses5-dev \
	rsync \
	python \
	unzip \
	bc \
	locales \
	cpio
# Let's make wget output a bit less verbose.
RUN echo progress = dot:mega > /.wgetrc
RUN wget http://buildroot.uclibc.org/downloads/buildroot-$BUILDROOT_VERSION.tar.gz
# Check that we got the right tarball (update this when upgrading buildroot!)
RUN echo "a3113756938a9c8b49ea763d1aa37e23  buildroot-$BUILDROOT_VERSION.tar.gz" > buildroot.md5
RUN md5sum --check buildroot.md5
RUN tar -zxf buildroot-$BUILDROOT_VERSION.tar.gz
# This symlink is because WORKDIR doesn't expand $VARS.
RUN ln -s buildroot-$BUILDROOT_VERSION buildroot
WORKDIR /buildroot
ADD .config /buildroot/ 
ADD openssl-006-fix-library-version.patch /buildroot/package/openssl/
RUN sed -i "s/no-shared,shared/no-shared,shared -Wl,--version-script=openssl.ld/" package/openssl/openssl.mk  
RUN for CFGVAR in \
    CONFIG_FEATURE_SEAMLESS_GZ \
    ; do sed -i "s/# $CFGVAR is not set/$CFGVAR=y/" package/busybox/busybox.config ; done
RUN make
RUN ln -s /buildroot/output/images/rootfs.tar /rootfs.tar
