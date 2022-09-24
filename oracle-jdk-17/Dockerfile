FROM oraclelinux:8 as builder

MAINTAINER Aurelio Garcia-Ribeyro <aurelio.garciaribeyro@oracle.com>

RUN set -eux; \
	dnf install -y tar; 
	
ENV LANG en_US.UTF-8

ENV JAVA_URL=https://download.oracle.com/java/17/latest \
	JAVA_HOME=/usr/java/jdk-17

RUN set -eux; \
	ARCH=$(uname -m) && \
    if [ "$ARCH" == "x86_64" ]; \
        then ARCH="x64"; \
    fi && \
    JAVA_PKG="$JAVA_URL/jdk-17_linux-${ARCH}_bin.tar.gz" ; \
	JAVA_SHA256=$(curl "$JAVA_PKG".sha256) ; \
	curl --output /tmp/jdk.tgz "$JAVA_PKG" && \
	echo "$JAVA_SHA256 */tmp/jdk.tgz" | sha256sum -c; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file /tmp/jdk.tgz --directory "$JAVA_HOME" --strip-components 1
	
FROM oraclelinux:8

ENV LANG en_US.UTF-8
ENV	JAVA_HOME=/usr/java/jdk-17
ENV	PATH $JAVA_HOME/bin:$PATH	

COPY --from=builder $JAVA_HOME $JAVA_HOME

RUN set -eux; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone; \
	dnf -y update; \
	dnf install -y \
		freetype fontconfig \
	; \
	rm -rf /var/cache/dnf; \
	ln -sfT "$JAVA_HOME" /usr/java/default; \
	ln -sfT "$JAVA_HOME" /usr/java/latest; \
	for bin in "$JAVA_HOME/bin/"*; do \
		base="$(basename "$bin")"; \
		[ ! -e "/usr/bin/$base" ]; \
		alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
	done;
	
CMD ["jshell"]
