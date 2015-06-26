Friends of Friends cluster finder.

1. About

2. Installation Instructions

3. Example Usage

4. Troubleshooting



1. About

Friends of Friends galaxy cluster finding code

2. Authors

Samuel Farrens, Stefano Sartor, Luca Tornatore


3. Installation Instructions
For installation instructions see ./docs/readme.md
and ./docs/html/index.html


4. Example Usage




5. Troubleshooting

See also ./docs/html/index.html

5.1 Trouble shooting on a macOSX Yosemite 10.10.3 

5.1.1 Trouble with cmake

Upon getting an error with the command

%>./cmake CMakeList.txt

Try adding your openmp gcc executables directly. e.g. on a macOS X (10.10.3) with macports install MPI found here:
/opt/local/bin/gcc-mp-4.9

type:

%>CC=gcc-mp-4.9 CXX=g++-mp-4.9 cmake CMakeLists.txt

At least one user was required to pass in the following CMAKE_CXX_FLAGS flags:

%>CC=gcc-mp-4.9 CXX=g++-mp-4.9 CMAKE_CXX_FLAGS="-stdlib=libstdc++.6.dylib -L/opt/local/lib -I/opt/local/include -L/opt/local/lib -lboost_program_options-mt" cmake CMakeLists.txt

5.1.2 Trouble with make

If cmake passes and make fails, in particular with "boost" linking errors you will need to install boost by hand. Be sure to remove other versions of boot first.
E.g. on a Mac
%>sudo port uninstall boost

Then download the Boost source: http://sourceforge.net/projects/boost/files/boost/1.58.0/boost_1_58_0.tar.gz/download

and run the bootstrap script from within boost_X_XX_X/

>> ./bootstrap.sh --prefix=<PATH> --with-toolset=gcc


If gcc points to the openmp version of gcc, E.g. on a macOS
%>which gcc
/opt/local/bin/gcc
%>ls -alh /opt/local/bin/gcc 
/opt/local/bin/gcc -> /opt/local/bin/gcc-mp-4.9

You are good to go.

If gcc doesn't point to openmp version, then follow these commands:

a)Edit line 13 of the project-config.jam file that is generated:

b) e.g. Change “ using gcc ; ” to something like “ using gcc : 4.9.0 : g++-4.9.0 : <linker-type>darwin ; “

b.1) Note that where I have "g++-4.9.0” can be the full path to the executable (e.g. /usr/local/bin/g++-4.9.0).

c) Run the b2 script:
%> ./b2 install


Finally run with boost paths set

%>CC=gcc-mp-4.9 CXX=g++-mp-4.9 CMAKE_CXX_FLAGS="-stdlib=libstdc++.6.dylib -L/opt/local/lib -I/opt/local/include -L/opt/local/lib -lboost_program_options-mt" cmake CMakeLists.txt -DBOOST_ROOT= <PATH>

