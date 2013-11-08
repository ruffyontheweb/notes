HTMLIZER=maruku
LIST:=$(patsubst %.md,%.html,$(wildcard *.md))

all: $(LIST)

%.html:%.md
	$(HTMLIZER) $<

clean:
	rm -f $(LIST)
