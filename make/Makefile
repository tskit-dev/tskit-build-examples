
TSK_OBJECTS=kastore.o tskit_tables.o tskit_core.o tskit_trees.o \
	tskit_stats.o tskit_genotypes.o tskit_convert.o
CFLAGS += -Ikastore/c -I tskit/c

# The example program.
example: example.c libtskit.a
	${CC} ${CFLAGS} $^ -o $@

libtskit.a: ${TSK_OBJECTS}
	${AR} rcs $@ ${TSK_OBJECTS} 

kastore.o: kastore
	${CC} -c ${CFLAGS} kastore/c/kastore.c -o kastore.o

tskit_%.o: tskit
	${CC} -c ${CFLAGS} tskit/c/tskit/$*.c -o $@

kastore:
	git clone https://github.com/tskit-dev/kastore.git

tskit: 
	git clone https://github.com/tskit-dev/tskit.git

clean:
	rm -f *.a *.o

mrproper: clean
	rm -fr tskit kastore example example.trees

