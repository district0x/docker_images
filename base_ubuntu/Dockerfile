FROM ubuntu:22.04
SHELL ["/bin/bash", "-exo", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=dumb \
    PAGER=cat \
    YQ_VERSION="v4.45.4"
# Configure environment
# RUN echo 'APT::Get::Assume-Yes "true";' > /etc/apt/apt.conf.d/90circleci && \
# 	echo 'DPkg::Options "--force-confnew";' >> /etc/apt/apt.conf.d/90circleci && \
RUN mkdir ~/bin && mkdir -p ~/.local/bin \
    && apt-get update && apt-get install -y \
		curl \
		locales \
	&& locale-gen en_US.UTF-8 \
	&& rm -rf /var/lib/apt/lists/* \
    && dockerizeArch=arm64 \
    && if uname -p | grep "x86_64"; then dockerizeArch=x86_64; fi \
    && curl -sSL --fail --retry 3 --output /usr/local/bin/dockerize "https://github.com/powerman/dockerize/releases/download/v0.20.2/dockerize-linux-${dockerizeArch}" \
	&& chmod +x /usr/local/bin/dockerize&& dockerize --version

ENV PATH=~/ubuntu/.local/bin:~/bin:$PATH \
	LANG=en_US.UTF-8 \
	LANGUAGE=en_US:en \
	LC_ALL=en_US.UTF-8

RUN apt-get update && apt-get install -y --no-install-recommends \
		autoconf \
		build-essential \
		ca-certificates \
		cmake \
		curl \
		file \
		gettext-base \
		gnupg \
		gzip \
		jq \
		libcurl4-openssl-dev \
		libmagic-dev \
		# popular DB lib - MariaDB
		libmariadb-dev \
		# allows MySQL users to use MariaDB lib
		libmariadb-dev-compat \
		# popular DB lib - PostgreSQL
		libpq-dev \
		libssl-dev \
		libsqlite3-dev \
		lsof \
		make \
		# compiling tool
		pkg-config \
		python-is-python3 \
		retry \
		shellcheck \
		software-properties-common \
		# already installed but here for consistency
		sudo \
		tar \
		tzdata \
		unzip \
		# for ssh-enabled builds
		vim \
		wget \
		zip && \
	rm -rf /var/lib/apt/lists/*


RUN YQ_ARCH=$(dpkg --print-architecture) && \
	curl -sSL "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_$YQ_ARCH.tar.gz" | \
	tar -xz -C /usr/local/bin && \
	mv /usr/local/bin/yq{_linux_$YQ_ARCH,}
