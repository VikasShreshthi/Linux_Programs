In this we are checking how to create static libabry, dynamic library.
How to use both.

Commands:
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
#
#
gcc   bin/main.o  -Lbin/static -ltq84 -o bin/statically-linked
./bin/statically-linked

Create the shared library
gcc -shared bin/shared/add.o bin/shared/answer.o -o bin/shared/libtq84.so

#
#  In order to create a shared library, position independent code
#  must be generated. This can be achieved with `-fPIC` flag when
#  c-files are compiled.
#
#  If the object files are created without -fPIC (such as when the static object files are produces), then
#      gcc -shared bin/static/add.o bin/static/answer.o -o bin/shared/libtq84.so
#  produces this error:
#     /usr/bin/ld: bin/tq84.o: relocation R_X86_64_PC32 against symbol `gSummand' can not be used when making a shared object; recompile with -fPIC


Link dynamically with the shared library

gcc  bin/main.o -Lbin/shared -ltq84 -o bin/use-shared-library

#
#   Moving the shared object to a standard location
#
sudo mv bin/shared/libtq84.so /usr/lib
sudo chmod 755 /usr/lib/libtq84.so

#
#   Since the shared library was copied to a standard
#   location (/usr/lib), the environment variable LD_LIBRARY_PATH
#   does not need to be set to execute the executable:

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



refer 
https://renenyffenegger.ch/notes/development/languages/C-C-plus-plus/GCC/create-libraries/index

