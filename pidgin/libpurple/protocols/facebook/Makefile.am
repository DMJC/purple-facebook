EXTRA_DIST = \
	Makefile.mingw \
	marshaller.list

pkgdir = @PURPLE_PLUGINDIR@

FACEBOOKSOURCES = \
	marshal.c \
	marshal.h \
	api.c \
	api.h \
	data.c \
	data.h \
	facebook.h \
	facebook.c \
	http.c \
	http.h \
	id.h \
	json.c \
	json.h \
	mqtt.c \
	mqtt.h \
	thrift.c \
	thrift.h \
	util.c \
	util.h \
	../../glibcompat.h \
	../../http.c \
	../../http.h \
	../../purple-socket.h \
	../../purple-socket.c

CLEANFILES = \
	marshal.c \
	marshal.h

marshal.c: $(srcdir)/marshaller.list marshal.h
	$(AM_V_GEN)echo "#include \"marshal.h\"" > $@
	$(AM_V_at)$(GLIB_GENMARSHAL) --prefix=fb_marshal --body $(srcdir)/marshaller.list >> $@

marshal.h: $(srcdir)/marshaller.list
	$(AM_V_GEN)$(GLIB_GENMARSHAL) --prefix=fb_marshal --header $(srcdir)/marshaller.list > $@

AM_CFLAGS = $(st)

libfacebook_la_LDFLAGS = -module @PLUGIN_LDFLAGS@

if STATIC_FACEBOOK

st = -DPURPLE_STATIC_PRPL
noinst_LTLIBRARIES     = libfacebook.la
libfacebook_la_SOURCES = $(FACEBOOKSOURCES)
libfacebook_la_CFLAGS  = $(AM_CFLAGS)

else

st =
pkg_LTLIBRARIES        = libfacebook.la
libfacebook_la_SOURCES = $(FACEBOOKSOURCES)
libfacebook_la_LIBADD  = @PURPLE_LIBS@ $(JSON_LIBS)

endif

AM_CPPFLAGS = \
	$(GLIB_CFLAGS) \
	$(JSON_CFLAGS) \
	$(PURPLE_CFLAGS) \
	$(ZLIB_CFLAGS) \
	$(PLUGIN_CFLAGS) \
	$(DEBUG_CFLAGS)
