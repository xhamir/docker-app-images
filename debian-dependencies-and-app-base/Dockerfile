FROM debian:jessie
MAINTAINER Omar Jair Montalvo Aquines <xhamir.one@gmail.com>

#####################################################################################
RUN apt-get update && apt-get install -y --no-install-recommends \
		ca-certificates \
		curl \
		bzr \
		git \
		autoconf \
		build-essential \
		imagemagick \
		libbz2-dev \
		libcurl4-openssl-dev \
		libevent-dev \
		libffi-dev \
		libglib2.0-dev \
		libjpeg-dev \
		liblzma-dev \
		libmagickcore-dev \
		libmagickwand-dev \
		libncurses-dev \
		libpq-dev \
		libreadline-dev \
		libssl-dev \
		libxml2-dev \
		libxslt-dev \
		libyaml-dev \
		zlib1g-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& cp /etc/default/useradd /etc/default/useradd.bak \
  	&& echo "HOME=" >> /etc/default/useradd \
  	&& useradd --create-home --shell /usr/sbin/nologin app \
  	&& rm -rf /etc/default/useradd \
  	&& mv /etc/default/useradd.bak /etc/default/useradd \
  	&& rm /app/.bash_logout /app/.bashrc /app/.profile

# 3 > 29	| Build dependencies.
# 30		| Clean.
# 31 > 36 	| Setup the 'app' User & '/app' directory.
# 31 > 36 	| Add the "app" user and group first to make sure their IDs get assigned
# 31 > 36 	| consistently - we definitely like them to be both 1000, to match our
# 31 > 36 	| workstation's user/group ID's.
# 31 > 36 	| Also, we like the app user home directory to be '/app':
#####################################################################################

# Define the '/app' directory as a volume, to prevent persistance of files in
# this folder:
VOLUME /app

