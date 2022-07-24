TUP = tup

ifeq (, $(shell which tup))
TUP = ./build.sh
endif

all: .tup
	$(TUP) -j$(shell nproc)

build: .tup
	$(TUP) -j$(shell nproc) hello 

FILES=$(shell sqlite3 .tup/db " \
	WITH RECURSIVE path(id, level, name, parent) AS ( \
		SELECT id, 0, name, dir FROM node WHERE type=4 \
		UNION ALL \
		SELECT path.id, path.level + 1, node.name, node.dir FROM node \
		JOIN path on node.id = path.parent), \
	path_from_root AS ( \
		SELECT id, name FROM path ORDER BY id, level DESC) \
	SELECT group_concat(name, '/') FROM path_from_root GROUP BY id;" | xargs echo)

clean:
	rm -f $(FILES)

.PHONY: all clean build

.tup: 
	tup init

build.sh: clean
	tup generate build.sh
