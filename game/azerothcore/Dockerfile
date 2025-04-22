FROM ubuntu:22.04

ENV AC_CODE_DIR=/data/azerothcore

RUN export AC_CODE_DIR=/data/azerothcore
WORKDIR $AC_CODE_DIR

RUN apt-get update \
    && apt-get install -y git cmake make gcc g++ clang libmysqlclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev mysql-server libboost-all-dev supervisor screen

RUN git clone https://gitclone.com/github.com/azerothcore/azerothcore-wotlk.git --branch master --depth=1 --single-branch $AC_CODE_DIR
RUN cd $AC_CODE_DIR && mkdir build && cd build \
    && cmake ../ -DCMAKE_INSTALL_PREFIX=$AC_CODE_DIR/env/dist/ -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DWITH_WARNINGS=1 -DTOOLS_BUILD=all -DSCRIPTS=static -DMODULES=static \
    && make -j8 && make install

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

VOLUME $AC_CODE_DIR
EXPOSE 8085

ENTRYPOINT ["/usr/bin/supervisord"]