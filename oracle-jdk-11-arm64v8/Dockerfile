FROM oraclelinux:7@sha256:c28cbdb929c7ad5cbd4f0c6ba0e43572ba2b8eab891b8309d464e82cd0ccdc4c

MAINTAINER Du Kui <dk900912@gmail.com>

RUN /bin/sh -c set -eux; \
    yum install gzip tar binutils freetype fontconfig; \
    yum clean all

ENV LANG en_US.UTF-8
         
ENV JAVA_PKG=jdk-11.0.18_linux-aarch64_bin.tar.gz \
        JAVA_SHA256=9888c78f8e50c2c83d2aaa941b610e1263b91a96214fa3840d33e3e20c709256 \
        JAVA_HOME=/usr/java/jdk-11

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
