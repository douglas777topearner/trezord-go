PLATFORM  = linux
VOL_MOUNT = -v $(shell pwd):/release
IMAGETAG  = trezord-go-build-env-$(PLATFORM)

IMPORT_PATH = ../..

all: clean .package

clean:
	$(info Cleaning...)
	rm -rf build

.binary:
	$(info Building with xgo ...)
	mkdir -p build
	# xgo -targets=linux/386,linux/amd64,linux/arm-7,linux/arm64 $(IMPORT_PATH)
	xgo -targets=linux/386,linux/amd64 $(IMPORT_PATH)
	mv -f trezord-go-linux-386 build/trezord-linux-386
	mv -f trezord-go-linux-amd64 build/trezord-linux-amd64
	# mv -f trezord-go-linux-arm-7 build/trezord-linux-arm-7
	# mv -f trezord-go-linux-arm64 build/trezord-linux-arm64
	cp ../../VERSION build

.package: .binary .docker-image
	$(info Packaging ...)
	docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /release/release.sh linux-386
	docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /release/release.sh linux-amd64
	# docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /release/release.sh linux-arm-7
	# docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /release/release.sh linux-arm64

.docker-image:
	$(info Preparing docker image ...)
	docker build -t $(IMAGETAG) .

shell: .docker-image
	docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /bin/bash

privkey:
	gpg --armor --export-secret-key > privkey.asc
