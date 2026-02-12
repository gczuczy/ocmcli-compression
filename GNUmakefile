CTFDIR=ctf
PAYLOAD?=prayer.txt
CTOR=component-descriptor.yaml
BASEDIR=$(shell dirname "$(abspath $(lastword $(MAKEFILE_LIST)))")
BINDIR=$(BASEDIR)/bin
OCMVER?=0.35.0
CTFINDEX=$(CTFDIR)/artifact-index.json
OCMCLI=$(BINDIR)/ocm
CV_DESCRIPTOR=$(BASEDIR)/component-descriptor.yaml
OCMCLI_IMAGE?=ghcr.io/open-component-model/cli:main
CROOT=/work
CCTFDIR=$(CROOT)/ctf
CCV_DESCRIPTOR=$(CROOT)/component-descriptor.yaml
OCMCLI=docker run -u $(shell id -u):$(shell id -g) --mount type=bind,source=.,destination=$(CROOT) $(OCMCLI_IMAGE)

COMPNAME=foo.bar/foobar
PROVIDER=foo.bar
RSCNAME=prayer
RSCVERSION=0.0.1
DLFILE=prayer-dl.txt
CDLFILE=$(CROOT)/prayer-dl.txt

$(PAYLOAD).gz: $(PAYLOAD)
	gzip -c $< > $@

.PHONY: pack-ocm
pack-ocm: $(CTFINDEX)

$(CTFDIR):
	mkdir -p $(CTFDIR)

$(CTFINDEX): $(PAYLOAD).gz $(CV_DESCRIPTOR) | $(CTFDIR)
	$(OCMCLI) add componentversions --repository $(CCTFDIR) --constructor $(CCV_DESCRIPTOR)
#ocm add componentversions --create --file $(CCTFDIR) $(CCV_DESCRIPTOR)

$(BINDIR):
	mkdir $@

.PHONY: dl
dl: $(DLFILE)

$(DLFILE): $(CTFINDEX)
	$(OCMCLI) download resource $(CCTFDIR)//$(COMPNAME):$(RSCVERSION) \
	--output $(CDLFILE) --identity name=$(RSCNAME)
	file $@
