
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

tskit: 
	mkdir tskit
	cd tskit && wget https://github.com/tskit-dev/tskit/releases/download/C_1.0.0/tskit-1.0.0.tar.xz
	cd tskit && tar xvf tskit-1.0.0.tar.xz --strip-components=1

clean:
	rm -f *.a *.o

mrproper: clean
	rm -fr tskit example example.trees
