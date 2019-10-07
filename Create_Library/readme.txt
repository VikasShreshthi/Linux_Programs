refer 
https://renenyffenegger.ch/notes/development/languages/C-C-plus-plus/GCC/create-libraries/index

commands used:
Create the object files
gcc -c       src/main.c        -o bin/main.o

#
# Create the object files for the static library (without -fPIC)
#
gcc -c       src/tq84/add.c    -o bin/static/add.o
gcc -c       src/tq84/answer.c -o bin/static/answer.o

#
# object files for shared libraries need to be compiled as position independent
# code (-fPIC) because they are mapped to any position in the address space.
#
gcc -c -fPIC src/tq84/add.c    -o bin/shared/add.o
gcc -c -fPIC src/tq84/answer.c -o bin/shared/answer.o

Create static library
ar rcs bin/static/libtq84.a bin/static/add.o bin/static/answer.o

Link statically
gcc   bin/main.o  -Lbin/static -ltq84 -o bin/statically-linked
$ ./bin/statically-linked

Create the shared library
gcc -shared bin/shared/add.o bin/shared/answer.o -o bin/shared/libtq84.so
Link dynamically with the shared library
gcc  bin/main.o -Lbin/shared -ltq84 -o bin/use-shared-library

Use the shared library with LD_LIBRARY_PATH
LD_LIBRARY_PATH=$(pwd)/bin/shared bin/use-shared-library
sudo mv bin/shared/libtq84.so /usr/lib
sudo chmod 755 /usr/lib/libtq84.so

bin/use-shared-library

#
# Using dl functions
#
gcc src/dynamic-library-loader.c -ldl -o bin/dynamic-library-loader

#
#  As long as /usr/lib/libtq84.so exists, LD_LIBRARY_PATH
#  needs not be set
#
bin/dynamic-library-loader