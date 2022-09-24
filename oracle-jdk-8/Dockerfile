FROM oraclelinux:7-slim as builder

MAINTAINER Aurelio Garcia-Ribeyro <aurelio.garciaribeyro@oracle.com>

RUN set -eux; \
	yum install -y \
		gzip \
		tar \
	; \
	rm -rf /var/cache/yum

ENV LANG en_US.UTF-8

ENV JAVA_PKG=jdk-8u341-linux-x64.tar.gz \
	JAVA_SHA256=c98e57cfc6ac9947d9aa9a31c5878d52e2bf764f8d90f20eec08f3c3fcaee0e7 \
	JAVA_HOME=/usr/java/jdk-8

COPY $JAVA_PKG /tmp/jdk.tgz
RUN set -eux; \
	echo "$JAVA_SHA256 */tmp/jdk.tgz" | sha256sum -c -; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file /tmp/jdk.tgz --directory "$JAVA_HOME" --strip-components 1;

FROM oraclelinux:7-slim

ENV LANG en_US.UTF-8

ENV JAVA_VERSION=1.8.0_341 \
	JAVA_HOME=/usr/java/jdk-8 
	
ENV	PATH $JAVA_HOME/bin:$PATH

COPY --from=builder $JAVA_HOME $JAVA_HOME

RUN	ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone; \
    yum -y update; \
	rm -rf /var/cache/yum; \
	ln -sfT "$JAVA_HOME" /usr/java/default; \
	ln -sfT "$JAVA_HOME" /usr/java/latest; \
	for bin in "$JAVA_HOME/bin/"*; do \
		base="$(basename "$bin")"; \
		[ ! -e "/usr/bin/$base" ]; \
		alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
	done; \
	java -Xshare:dump;