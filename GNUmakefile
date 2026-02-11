CTFDIR=ctf
PAYLOAD?=prayer.txt
CTOR=component-descriptor.yaml
BASEDIR=$(shell dirname "$(abspath $(lastword $(MAKEFILE_LIST)))")
BINDIR=$(BASEDIR)/bin
OCMVER?=0.35.0
CTFINDEX=$(CTFDIR)/artifact-index.json
OCMCLI=$(BINDIR)/ocm
CV_DESCRIPTOR=$(BASEDIR)/component-descriptor.yaml
OS?=linux
ARCH?=amd64
OCMCLI_URL?=https://github.com/open-component-model/ocm/releases/download/v0.35.0/ocm-$(OCMVER)-$(OS)-$(ARCH).tar.gz

COMPNAME=foo.bar/foobar
RSCNAME=prayer
DLFILE=prayer-dl.txt

$(PAYLOAD).gz: $(PAYLOAD)
	gzip -c $< > $@

.PHONY: pack-ocm
pack-ocm: $(CTFINDEX)

$(CTFDIR):
	mkdir -p $(CTFDIR)

$(CTFINDEX): $(OCMCLI) $(PAYLOAD).gz $(CV_DESCRIPTOR) | $(CTFDIR)
	ocm add componentversions --create --file $(CTFDIR) $(CV_DESCRIPTOR)

$(BINDIR):
	mkdir $@

$(OCMCLI): | $(BINDIR)
	wget -qO - $(OCMCLI_URL) | tar -C $(BINDIR)/ -xzf - ocm

.PHONY: dl
dl: $(DLFILE)

$(DLFILE): $(CTFINDEX)
	ocm download resource -O $@ directory::$(CTFDIR)//${COMPNAME} ${RSCNAME}
	file $@
