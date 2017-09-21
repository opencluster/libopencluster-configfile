## make file for libopencluster-auth.

# Project specific variables.
PRODUCT=opencluster-configfile
LIBVER=0.1.0
SOVER=1
DESTDIR=
WORKDIR=.


# Additional compile args.
ARGS=

# Project derived variables.  Do not manually change these. These should be dependent on the variables already set.
LIBNAME=lib$(PRODUCT)
SRCFILE=$(LIBNAME).c
INCFILE=$(PRODUCT).h
MAINFILE=$(LIBNAME).so
LIBFILE=$(MAINFILE).$(LIBVER)
SONAME=$(MAINFILE).$(SOVER)
LIBDIR=$(DESTDIR)/usr/lib
INCDIR=$(DESTDIR)/usr/include


MANPATH=/usr/local/man

all: $(WORKDIR)/$(LIBFILE)
package: $(WORKDIR)/$(LIBNAME)-$(LIBVER).tar.gz
src: $(WORKDIR)/$(LIBNAME)-$(LIBVER)-src.tar.gz

static: $(WORKDIR)/$(LIBNAME)-$(LIBVER)-static.tar.gz


# Need to be able to make 'man-pages' as well.  Not sure where to get the source for those... 

$(WORKDIR)/$(LIBNAME).o: $(SRCFILE) $(INCFILE)
	@echo "Making: $@"
	@-[ -d $(WORKDIR) ] || mkdir $(WORKDIR)
	gcc -c -fPIC $(SRCFILE) -o $@ $(ARGS)

$(WORKDIR)/$(LIBNAME).a: $(WORKDIR)/$(LIBNAME).o
	@echo "Making: $@"
	@-[ ! -d $(WORKDIR) ] && mkdir $(WORKDIR)
	@>$@
	@rm $@
	ar -r $@
	ar -r $@ $^

$(WORKDIR)/$(LIBFILE): $(WORKDIR)/$(LIBNAME).o
	@echo "Making: $@"
	@-[ -d $(WORKDIR) ] || mkdir $(WORKDIR)
	gcc -shared -Wl,-soname,$(SONAME) -o $@ $^


$(WORKDIR)/$(LIBNAME)-$(LIBVER)-src.tar.gz: $(SRCFILE) $(INCFILE) Makefile
	@echo "Making: $@"
	@-test ! -d $(WORKDIR) || mkdir $(WORKDIR)
	@-test ! -e $@ || rm $@
	tar zcf $@ $^

$(WORKDIR)/$(LIBNAME)-$(LIBVER).tar.gz: $(WORKDIR)/$(LIBFILE) $(INCFILE) $(WORKDIR)/$(LIBNAME).o
	@echo "Making: $@"
	@-[ -d $(WORKDIR) ] || mkdir $(WORKDIR)
	@-test ! -e $@ || rm $@
	tar zcf $@ $^

$(WORKDIR)/$(LIBNAME)-$(LIBVER)-static.tar.gz: $(WORKDIR)/$(LIBNAME).o $(INCFILE)
	@echo "Making: $@"
	@-[ -d $(WORKDIR) ] || mkdir $(WORKDIR)
	@-test ! -e $@ || rm $@
	tar zcf $@ $^

install: $(WORKDIR)/$(LIBFILE)
	cp $(WORKDIR)/$(LIBFILE) $(LIBDIR)/
	@rm $(LIBDIR)/$(MAINFILE)
	ln -s $(LIBDIR)/$(LIBFILE) $(LIBDIR)/$(MAINFILE)
	ldconfig


install_dev: $(INCFILE) $(LIBNAME).pc
	cp $(INCFILE) $(INCDIR)/
	cp $(LIBNAME).pc /usr/lib/pkgconfig/


man-pages: manpages/libopencluster-auth.3 
	@mkdir tmp.install
	@cp manpages/* tmp.install/
	@gzip tmp.install/*.3
	cp tmp.install/*.3.gz $(MANPATH)/man3/
	@rm tmp.install/*
	@rmdir tmp.install
	@echo "Man-pages Install complete."


uninstall:
	@-rm $(LIBDIR)/$(LIBFILE)
	@-rm $(LIBDIR)/$(MAINFILE)

clean:
	@-rm $(WORKDIR)/$(LIBNAME).o
	@-rm $(WORKDIR)/$(LIBNAME).so*
