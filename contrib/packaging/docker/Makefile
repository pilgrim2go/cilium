include ../../../../Makefile.defs

BUILDDIR := "$(CURDIR)/cilium-$(VERSION)"

build: clean
	@$(CURDIR)/../cp-dirs.sh "$(BUILDDIR)"
	find "$(BUILDDIR)" -name ".*" -exec rm -rf {} \;
	docker build -t "cilium:cilium-ubuntu-16-04" "$(BUILDDIR)"

clean:
	ls -d ./* | grep -vE Makefile | xargs rm -rf

.PHONY: clean build force
force :;
