build/%.iso: build/%.cfg
	./build-kickstart-iso.sh build/$*.cfg build/$*.iso

build/%.cfg: /kicks/base.cfg
	ansible-playbook --limit $* ../template-kickstart.yml

.PHONY: clean
clean:
	rm -rf build/*
