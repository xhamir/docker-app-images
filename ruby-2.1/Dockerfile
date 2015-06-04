FROM xhamir/debian-base-app:0.01
MAINTAINER Omar Jair Montalvo Aquines <xhamir.one@gmail.com>

#ENV NODE_VERSION='0.12.2' \ 
#	NPM_VERSION='2.9.0' \
#	RUBY_MAJOR='2.1' \
#	RUBY_VERSION='2.1.6' \
#	RUBY_DOWNLOAD_SHA256='1e1362ae7427c91fa53dc9c05aee4ee200e2d7d8970a891c5bd76bee28d28be4' 

# verify gpg and sha256: http://nodejs.org/dist/v0.10.30/SHASUMS256.txt.asc
# gpg: aka "Timothy J Fontaine (Work) <tj.fontaine@joyent.com>"
# gpg: aka "Julien Gilli <jgilli@fastmail.fm>"

RUN gpg --keyserver pool.sks-keyservers.net \
    --recv-keys 7937DFD2AB06298B2293C3187D33FF9D0246406D \
    114F43EE0176B71C7BC219DD50A3051F888C628D

RUN curl -SLO "http://nodejs.org/dist/v0.12.2/node-v0.12.2-linux-x64.tar.gz" \
	&& curl -SLO "http://nodejs.org/dist/v0.12.2/SHASUMS256.txt.asc" \
	&& gpg --verify SHASUMS256.txt.asc \
	&& grep " node-v0.12.2-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
	&& tar -xzf "node-v0.12.2-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v0.12.2-linux-x64.tar.gz" SHASUMS256.txt.asc \
	&& npm install -g npm@"2.9.0" \
	&& npm cache clear \ 
	&& apt-get update \
	&& apt-get install -y bison libgdbm-dev ruby systemtap-sdt-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir -p /usr/src/ruby \
	&& curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.6.tar.gz" \
	&& echo "1e1362ae7427c91fa53dc9c05aee4ee200e2d7d8970a891c5bd76bee28d28be4 *ruby.tar.gz" | sha256sum -c - \
	&& tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
	&& rm ruby.tar.gz \
	&& cd /usr/src/ruby \
	&& autoconf \
	&& ./configure --disable-install-doc --enable-dtrace \
	&& make -j"$(nproc)" \
	&& make install \
	&& apt-get purge -y --auto-remove bison libgdbm-dev ruby systemtap-sdt-dev \
	&& rm -r /usr/src/ruby 

RUN gem install bundler --no-rdoc --no-ri

# Set the '/app/vendor/bundle' folder as the GEM_HOME - It will preserve
# the original gem paths - check it with 'gem env':
ENV GEM_HOME /app/vendor/bundle

# Set the image user as the unprivileged 'app' user:
USER app

# Set the image's working directory to the '/app' folder, which is the 'app'
# user's home:
WORKDIR /app

# Set BUNDLE_APP_CONFIG to GEM_PATH and set PATH to include app binaries:
ENV BUNDLE_APP_CONFIG=$GEM_HOME PATH=/app/bin:$GEM_HOME/bin:$PATH

CMD ["irb"]