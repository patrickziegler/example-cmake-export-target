## How to run the container

```sh
docker build \
    --build-arg USER_NAME=$(whoami) \
    --build-arg USER_ID=$(id -u) \
    --build-arg USER_GID=$(id -g) \
    -t example-cmake-export-target-build .
docker run -it --rm -v ${PWD}:/usr/src/ example-cmake-export-target-build
```

## How to execute the example

```sh
cd /usr/src/

# build and install the library
mkdir build_lib && cd build_lib
cmake ../lib/ && make -j10 && sudo make install

# build and test the client
mkdir ../build_cli && cd ../build_cli
cmake ../cli/ && make -j10
./add
```

## References:

* https://cmake.org/cmake/help/git-stage/guide/importing-exporting/index.html
* https://gitlab.kitware.com/cmake/community/-/wikis/doc/tutorials/Exporting-and-Importing-Targets
