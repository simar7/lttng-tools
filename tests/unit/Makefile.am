AM_CFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src -I$(top_srcdir)/tests/utils/ -I$(srcdir)
AM_LDFLAGS =

if LTTNG_TOOLS_BUILD_WITH_LIBDL
AM_LDFLAGS += -ldl
endif
if LTTNG_TOOLS_BUILD_WITH_LIBC_DL
AM_LDFLAGS += -lc
endif

LIBTAP=$(top_builddir)/tests/utils/tap/libtap.la

LIBCOMMON=$(top_builddir)/src/common/libcommon.la
LIBSESSIOND_COMM=$(top_builddir)/src/common/sessiond-comm/libsessiond-comm.la
LIBHASHTABLE=$(top_builddir)/src/common/hashtable/libhashtable.la

if HAVE_LIBLTTNG_UST_CTL
noinst_PROGRAMS = test_uri test_session test_ust_data test_kernel_data
else
noinst_PROGRAMS = test_uri test_session	test_kernel_data
endif

# URI unit tests
test_uri_SOURCES = test_uri.c
test_uri_LDADD = $(LIBTAP) $(LIBCOMMON)

# Session unit test
SESSIONS=$(top_srcdir)/src/bin/lttng-sessiond/session.c	\
	 $(top_srcdir)/src/bin/lttng-sessiond/consumer.c \
	 $(top_srcdir)/src/bin/lttng-sessiond/health.c \
	 $(top_srcdir)/src/common/uri.c \
	 $(top_srcdir)/src/common/utils.c \
	 $(top_srcdir)/src/common/error.c

test_session_SOURCES = test_session.c $(SESSIONS)
test_session_LDADD = $(LIBTAP) $(LIBCOMMON) $(LIBSESSIOND_COMM) $(LIBHASHTABLE) \
					 -lrt

# UST data structures unit test
if HAVE_LIBLTTNG_UST_CTL
UST_DATA_TRACE=$(top_srcdir)/src/bin/lttng-sessiond/trace-ust.c \
	       $(top_srcdir)/src/bin/lttng-sessiond/consumer.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/buffer-registry.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/ust-registry.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/ust-metadata.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/ust-app.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/ust-consumer.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/fd-limit.c \
		   $(top_srcdir)/src/bin/lttng-sessiond/health.c \
	       $(top_srcdir)/src/common/uri.c \
	       $(top_srcdir)/src/common/utils.c

test_ust_data_SOURCES = test_ust_data.c $(UST_DATA_TRACE)
test_ust_data_LDADD = $(LIBTAP) $(LIBCOMMON) $(LIBSESSIOND_COMM) $(LIBHASHTABLE) \
					  -lrt -llttng-ust-ctl
endif

# Kernel data structures unit test
KERN_DATA_TRACE=$(top_srcdir)/src/bin/lttng-sessiond/trace-kernel.c	\
		$(top_srcdir)/src/bin/lttng-sessiond/consumer.c	\
		$(top_srcdir)/src/bin/lttng-sessiond/health.c \
		$(top_srcdir)/src/common/uri.c \
		$(top_srcdir)/src/common/utils.c

test_kernel_data_SOURCES = test_kernel_data.c $(KERN_DATA_TRACE)
test_kernel_data_LDADD = $(LIBTAP) $(LIBCOMMON) $(LIBSESSIOND_COMM) $(LIBHASHTABLE) \
						 -lrt
