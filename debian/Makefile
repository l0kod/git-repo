DEBFILES = control
ROOTDIR = root
OUTDIR = deb
DEBDIR = $(ROOTDIR)/DEBIAN
PREFIX = /usr
GIT_MINVER := $(shell sed -n -r 's/^MIN_GIT_VERSION\s*=\s*\(([0-9]+), ([0-9]+), ([0-9]+)\).*/\1.\2.\3/p' ../repo)
VERSION := $(shell git describe | sed -e 's/^v//')
AUTHOR := $(shell git config user.name) <$(shell git config user.email)>
DEB = git-repo_$(VERSION)_all.deb

.PHONY: all clean mrproper

$(OUTDIR)/$(DEB): $(DEBFILES)
	make -C .. DESTDIR=debian/$(ROOTDIR) PREFIX=$(PREFIX) install
	mkdir -p $(DEBDIR)
	cp $^ $(DEBDIR)
	sed -i -r 's/^(Version: )/\1$(VERSION)/' $(DEBDIR)/control
	sed -i -r 's/^(Maintainer: )/\1$(AUTHOR)/' $(DEBDIR)/control
	sed -i -r 's/@GIT_MINVER@/$(GIT_MINVER)/' $(DEBDIR)/control
	chmod a+rX -R $(ROOTDIR)
	mkdir -p $(OUTDIR)
	fakeroot -- dpkg-deb --build $(ROOTDIR) $(OUTDIR)

all: $(OUTDIR)/$(DEB)

clean:
	rm -rf $(ROOTDIR)

mrproper: clean
	rm -rf $(OUTDIR)
