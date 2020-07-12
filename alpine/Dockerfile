FROM alpine:3.12

RUN set -x \
    # Runtime dependencies.
 && apk add --no-cache \
        libcurl \
        libgcc \
        libstdc++ \
        openssl \
    # Build dependencies.
 && apk add --no-cache -t .build-deps \
        autoconf \
        automake \
        build-base \
        curl \
        curl-dev \
        git \
        openssl-dev \
    # Compile from source code.
 && git clone --recursive https://github.com/zcoinofficial/cpuminer.git /tmp/cpuminer \
 && cd /tmp/cpuminer \
 && chmod +x autogen.sh \
 && chmod +x configure \
 && ./autogen.sh \
 && ./configure CFLAGS="-march=bdver2" --with-crypto --with-curl \ 
 && make install \
    # Install dumb-init (avoid PID 1 issues).
    # https://github.com/Yelp/dumb-init
 && curl -Lo /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.2/dumb-init_1.2.2_amd64 \
 && chmod +x /usr/local/bin/dumb-init \
    # Clean-up
 && cd / \
 && apk del --purge .build-deps \
 && rm -rf /tmp/* \
    # Verify
 && cpuminer --cputest \
 && cpuminer --version

ENTRYPOINT ["dumb-init"]
CMD ["cpuminer", "--help"]
