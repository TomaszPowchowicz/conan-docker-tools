FROM ubuntu:xenial

LABEL maintainer="Luis Martinez de Bartolome <luism@jfrog.com>"

ENV PYENV_ROOT=/opt/pyenv \
    PATH=/opt/pyenv/shims:${PATH} \
    CXX=/usr/bin/g++ \
    CC=/usr/bin/gcc

RUN dpkg --add-architecture i386 \
    && apt-get -qq update \
    && apt-get -qq install -y --no-install-recommends \
       sudo=1.8.* \
       build-essential=12.* \
       wget=1.17.* \
       git=1:2.7.* \
       libc6-dev-i386=2.23-* \
       g++-multilib=4:5.3.* \
       libgmp-dev=2:6.1.* \
       libmpfr-dev=3.1.* \
       libmpc-dev=1.0.* \
       libc6-dev=2.23-* \
       nasm=2.11.* \
       dh-autoreconf=11 \
    && apt-get -qq install -y --allow-downgrades --no-install-recommends \
       gcc-5=5.3.1-14ubuntu2 \
       gcc-5-base=5.3.1-14ubuntu2 \
       cpp-5=5.3.1-14ubuntu2 \
       libcc1-0=5.3.1-14ubuntu2 \
       libgcc-5-dev=5.3.1-14ubuntu2 \
       libstdc++6=5.3.1-14ubuntu2 \
       libgomp1=5.3.1-14ubuntu2 \
       libitm1=5.3.1-14ubuntu2 \
       libatomic1=5.3.1-14ubuntu2 \
       libasan2=5.3.1-14ubuntu2 \
       liblsan0=5.3.1-14ubuntu2 \
       libtsan0=5.3.1-14ubuntu2 \
       libubsan0=5.3.1-14ubuntu2 \
       libcilkrts5=5.3.1-14ubuntu2 \
       libmpx0=5.3.1-14ubuntu2 \
       libquadmath0=5.3.1-14ubuntu2 \
       gcc-5-multilib=5.3.1-14ubuntu2 \
       lib32gcc-5-dev=5.3.1-14ubuntu2 \
       libx32gcc-5-dev=5.3.1-14ubuntu2 \
       lib32gomp1=5.3.1-14ubuntu2 \
       libx32gomp1=5.3.1-14ubuntu2 \
       lib32itm1=5.3.1-14ubuntu2 \
       libx32itm1=5.3.1-14ubuntu2 \
       lib32atomic1=5.3.1-14ubuntu2 \
       libx32atomic1=5.3.1-14ubuntu2 \
       lib32asan2=5.3.1-14ubuntu2 \
       libx32asan2=5.3.1-14ubuntu2 \
       lib32ubsan0=5.3.1-14ubuntu2 \
       libx32ubsan0=5.3.1-14ubuntu2 \
       lib32cilkrts5=5.3.1-14ubuntu2 \
       libx32cilkrts5=5.3.1-14ubuntu2 \
       lib32mpx0=5.3.1-14ubuntu2 \
       lib32quadmath0=5.3.1-14ubuntu2 \
       libx32quadmath0=5.3.1-14ubuntu2 \
       lib32stdc++6=5.3.1-14ubuntu2 \
       libx32stdc++6=5.3.1-14ubuntu2 \
       linux-libc-dev:i386 \
       libstdc++-5-dev=5.3.1-14ubuntu2 \
    && apt-get -qq install -y --allow-downgrades --no-install-recommends \
       g++-5=5.3.1-14ubuntu2 \
       libstdc++-5-dev=5.3.1-14ubuntu2 \
       g++-5-multilib=5.3.1-14ubuntu2 \
       lib32stdc++-5-dev=5.3.1-14ubuntu2 \
       libx32stdc++-5-dev=5.3.1-14ubuntu2 \
       ninja-build=1.5.* \
       libffi-dev=3.2.* \
       libssl-dev=1.0.2* \
       pkg-config=0.29.1-0ubuntu1 \
       subversion=1.9.3-2ubuntu1.1 \
       zlib1g-dev=1:1.* \
       libbz2-dev=1.* \
       libsqlite3-dev=3.* \
       libreadline-dev=6.* \
       xz-utils=5.* \
       curl=7.* \
       libncurses5-dev=6.* \
       libncursesw5-dev=6.* \
       liblzma-dev=5.* \
       ca-certificates \
       autoconf-archive \
    && ln -s /usr/bin/g++-5 /usr/bin/g++ \
    && rm -rf /var/lib/apt/lists/* \
    && wget -q --no-check-certificate https://cmake.org/files/v3.12/cmake-3.12.1-Linux-x86_64.tar.gz \
    && tar -xzf cmake-3.12.1-Linux-x86_64.tar.gz \
       --exclude=bin/cmake-gui \
       --exclude=doc/cmake \
       --exclude=share/cmake-3.12/Help \
    && cp -fR cmake-3.12.1-Linux-x86_64/* /usr \
    && rm -rf cmake-3.12.1-Linux-x86_64 \
    && rm cmake-3.12.1-Linux-x86_64.tar.gz \
    && groupadd 1001 -g 1001 \
    && groupadd 1000 -g 1000 \
    && groupadd 2000 -g 2000 \
    && groupadd 999 -g 999 \
    && useradd -ms /bin/bash conan -g 1001 -G 1000,2000,999 \
    && printf "conan:conan" | chpasswd \
    && adduser conan sudo \
    && printf "conan ALL= NOPASSWD: ALL\\n" >> /etc/sudoers \
    && wget --no-check-certificate --quiet -O /tmp/pyenv-installer https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer \
    && chmod +x /tmp/pyenv-installer \
    && /tmp/pyenv-installer \
    && rm /tmp/pyenv-installer \
    && update-alternatives --install /usr/bin/pyenv pyenv /opt/pyenv/bin/pyenv 100 \
    && PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install 3.7.1 \
    && pyenv global 3.7.1 \
    # remove all __pycache__ directories created by pyenv
    && find /opt/pyenv -iname __pycache__ -print0 | xargs -0 rm -rf \
    && pip install -q --upgrade --no-cache-dir pip \
    && pip install -q --no-cache-dir conan conan-package-tools \
    && chown -R conan:1001 /opt/pyenv \
    && update-alternatives --install /usr/bin/python python /opt/pyenv/shims/python 100 \
    && update-alternatives --install /usr/bin/python3 python3 /opt/pyenv/shims/python3 100 \
    && update-alternatives --install /usr/bin/pip pip /opt/pyenv/shims/pip 100 \
    && update-alternatives --install /usr/bin/pip3 pip3 /opt/pyenv/shims/pip3 100

USER conan
WORKDIR /home/conan

RUN mkdir -p /home/conan/.conan \
    && printf 'eval "$(pyenv init -)"\n' >> ~/.bashrc \
    && printf 'eval "$(pyenv virtualenv-init -)"\n' >> ~/.bashrc
