FROM oraclelinux@sha256:c28cbdb929c7ad5cbd4f0c6ba0e43572ba2b8eab891b8309d464e82cd0ccdc4c

MAINTAINER Du Kui <dk900912@gmail.com>

RUN /bin/sh -c set -eux; \
    yum install gzip tar binutils freetype fontconfig; \
    yum clean all

ENV LANG en_US.UTF-8

ENV JAVA_PKG=jdk-8u361-linux-aarch64.tar.gz \
        JFX_PKG=jfxrt.jar \
	JAVA_SHA256=581b6251f4ce31aa9f9e3e1756e1ae4d9ad366cd7cb233c5f9a1292173b44ee9 \
	JAVA_HOME=/usr/java/jdk-8

ENV PATH $JAVA_HOME/bin:$PATH

COPY $JAVA_PKG /tmp/jdk.tgz
COPY $JFX_PKG /tmp/jfxrt.jar

RUN /bin/sh -c set -eux; \
    echo "$JAVA_SHA256 */tmp/jdk.tgz" | sha256sum --strict --check -; \
    mkdir -p "$JAVA_HOME"; \
    tar --extract --file /tmp/jdk.tgz --directory "$JAVA_HOME" --strip-components 1; \
    cp /tmp/jfxrt.jar "$JAVA_HOME/jre/lib/ext/"; \
    rm /tmp/jdk.tgz*; \
    rm /tmp/jfxrt.jar*; \
    ln -sfT "$JAVA_HOME" /usr/java/default; \
    ln -sfT "$JAVA_HOME" /usr/java/latest; \
    for bin in "$JAVA_HOME/bin/"*; do \
      base="$(basename "$bin")"; \
      [ ! -e "/usr/bin/$base" ]; \
      alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
    done; \
    java -Xshare:dump;
