include ../../../Makefile.defs

BUILDDIR := "$(CURDIR)/cilium-$(VERSION)"

build: clean
	mkdir -p $(BUILDDIR)
	@$(CURDIR)/../cp-dirs.sh "$(BUILDDIR)"
	find "$(BUILDDIR)" -name ".*" -exec rm -rf {} \;
	cp "$(CURDIR)/Dockerfile" "$(BUILDDIR)"
	cp "$(CURDIR)/control" "$(BUILDDIR)"
	$(MAKE) -C "$(BUILDDIR)" clean
	docker build -t cilium:cilium-binary-$(VERSION) "$(BUILDDIR)"
	docker run --rm cilium:cilium-binary-$(VERSION) bash -c 'cd /tmp && tar -c cilium_$(VERSION).deb' | tar -xvC .

clean:
	ls -d ./* | grep -vE Makefile\|control\|Dockerfile | xargs rm -rf

.PHONY: force build clean
force :;

