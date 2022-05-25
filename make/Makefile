
TSK_VER=1.0.0
TARBALL=tskit-${TSK_VER}.tar.xz
DOWNLOAD=https://github.com/tskit-dev/tskit/releases/download/C_${TSK_VER}/${TARBALL}

TSK_OBJECTS=kastore.o tskit_tables.o tskit_core.o tskit_trees.o \
	tskit_stats.o tskit_genotypes.o tskit_convert.o
CFLAGS += -Itskit/subprojects/kastore -Itskit

# The example program.
example: example.c libtskit.a
	${CC} ${CFLAGS} $^ -o $@ -lm

libtskit.a: ${TSK_OBJECTS}
	${AR} rcs $@ ${TSK_OBJECTS}

kastore.o: tskit
	${CC} -c ${CFLAGS} tskit/subprojects/kastore/kastore.c -o kastore.o

tskit_%.o: tskit
	${CC} -c ${CFLAGS} tskit/tskit/$*.c -o $@

${TARBALL}:
	wget ${DOWNLOAD}

tskit: ${TARBALL}
	tar xf ${TARBALL}
	mv tskit-${TSK_VER} tskit

clean:
	rm -f *.a *.o

mrproper: clean
	rm -fr tskit example example.trees tskit-*.tar.xz
