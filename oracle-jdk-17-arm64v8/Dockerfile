FROM oraclelinux:8@sha256:0544d02a4bc774447f4ec47fdf34a0763017adbbf9b6219be2cccc3509c786e7

MAINTAINER Du Kui <dk900912@gmail.com>

RUN /bin/sh -c set -eux; \
    dnf install -y gzip tar binutils freetype fontconfig

ENV LANG en_US.UTF-8
         
ENV JAVA_PKG=jdk-17_linux-aarch64_bin.tar.gz \
        JAVA_SHA256=21f185f730c7af1891626c305cdaf28b086c1c423d9d1490847eb6200a9dabf6 \
        JAVA_HOME=/usr/java/jdk-17

ENV  PATH $JAVA_HOME/bin:$PATH

COPY $JAVA_PKG /tmp/jdk.tgz
    
RUN /bin/sh -c set -eux; \
    echo "$JAVA_SHA256 */tmp/jdk.tgz" | sha256sum --strict --check -; \
    mkdir -p "$JAVA_HOME"; \
    tar --extract --file /tmp/jdk.tgz --directory "$JAVA_HOME" --strip-components 1; \
    rm /tmp/jdk.tgz*; \
    ln -sfT "$JAVA_HOME" /usr/java/default; \
    ln -sfT "$JAVA_HOME" /usr/java/latest; \
    for bin in "$JAVA_HOME/bin/"*; do \
      base="$(basename "$bin")"; \
      [ ! -e "/usr/bin/$base" ]; \
      alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
    done; \
    java -Xshare:dump;
    
CMD ["jshell"]
