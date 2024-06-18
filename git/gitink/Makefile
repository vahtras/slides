.SUFFIXES: .txt .svg .svgh

default: c1.svg c2.svg

.txt.svg:
	gitink --in-file=$< --time-direction=0 > $*.svg
.txt.svgh:
	gitink --in-file=$< --time-direction=90 > $*.svgh

clean:
	rm -v c1.svg
