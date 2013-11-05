LIST=vagrant-note.html vagrant-base.html virtualbox.html vmware.html
all: $(LIST)

%.html:%.md
	maruku $<

clean:
	rm -f $(LIST)
