FROM oraclelinux:7-slim as builder

MAINTAINER Aurelio Garcia-Ribeyro <aurelio.garciaribeyro@oracle.com>

RUN set -eux; \
	yum install -y \
		gzip \
		tar;
	
ENV LANG en_US.UTF-8

ENV JAVA_HOME=/usr/java/jdk-11

COPY *.tar.gz /tmp/
RUN set -eux; \
	ARCH=$(uname -m) && \
    if [ "$ARCH" == "x86_64" ]; \
    then \
		mv $(ls /tmp/jdk-11*_linux-x64_bin.tar.gz) /tmp/jdk.tar.gz ; \
        JAVA_SHA256=4edc62ebb4359b276ea33abfd9a54bacb218b8f972603c20be9e415b8a59012c ; \
    else \
	    mv $(ls /tmp/jdk-11*_linux-aarch64_bin.tar.gz) /tmp/jdk.tar.gz ; \
    	JAVA_SHA256=546dca8e140abbcddbbb36f691817d81f7116b76e00e25ce2e0586bfee567f57 ; \
    fi && \
	echo "$JAVA_SHA256 */tmp/jdk.tar.gz" | sha256sum -c -; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file /tmp/jdk.tar.gz --directory "$JAVA_HOME" --strip-components 1
	
FROM oraclelinux:7-slim

ENV LANG en_US.UTF-8

ENV JAVA_VERSION=11.0.16.1 \
	JAVA_HOME=/usr/java/jdk-11

ENV	PATH $JAVA_HOME/bin:$PATH	

COPY --from=builder $JAVA_HOME $JAVA_HOME

RUN set -eux; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone; \
	yum -y update; \
	yum install -y \
		freetype fontconfig \
	; \
	rm -rf /var/cache/yum; \
	ln -sfT "$JAVA_HOME" /usr/java/default; \
	ln -sfT "$JAVA_HOME" /usr/java/latest; \
	for bin in "$JAVA_HOME/bin/"*; do \
		base="$(basename "$bin")"; \
		[ ! -e "/usr/bin/$base" ]; \
		alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
	done; \
	java -Xshare:dump;

CMD ["jshell"]
