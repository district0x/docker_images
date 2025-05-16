FROM 0xbase

ENV JAVA_VERSION=21.0.6 \
	JAVA_HOME=/usr/local/jdk-${JAVA_VERSION} \
	CLOJURE_VERSION=1.11.1.1435 \
	BB_VERSION=1.3.188 \
	NVM_VERSION=0.39.7 \
	NVM_DIR=/usr/local/nvm

# Install Java
RUN [[ $(uname -m) == "x86_64" ]] && ARCH="x64" || ARCH="aarch64" && \
	JAVA_MAJOR_VERSION=$(echo "$JAVA_VERSION" | cut -d '.' -f 1) && \
	JAVA_BUILD=$(curl -s "https://api.github.com/repos/adoptium/temurin${JAVA_MAJOR_VERSION}-binaries/releases" | \
	jq "limit(1; .[] | select(.tag_name | startswith(\"jdk-${JAVA_VERSION}+\"))) | .tag_name | split(\"+\")[1]" | tr -d '"') && \
	URL="https://github.com/adoptium/temurin${JAVA_MAJOR_VERSION}-binaries/releases/download/jdk-${JAVA_VERSION}%2B${JAVA_BUILD%.*}/OpenJDK${JAVA_MAJOR_VERSION}U-jdk_${ARCH}_linux_hotspot_${JAVA_VERSION}_${JAVA_BUILD%.*}.tar.gz" && \
	curl -sSL -o java.tar.gz "${URL}" && \
	sudo mkdir -p ${JAVA_HOME} && \
	sudo tar -xzf java.tar.gz --strip-components=1 -C ${JAVA_HOME} && \
	rm java.tar.gz && \
	sudo ln -s ${JAVA_HOME}/bin/* /usr/bin/ && \
	java --version

# # Install Clojure and Babashka
# RUN curl -sSL https://download.clojure.org/install/linux-install-${CLOJURE_VERSION}.sh | sudo bash && \
# 	sudo bash -c "curl -sSL https://raw.githubusercontent.com/babashka/babashka/master/install | bash -s -- ${BB_VERSION}"

# # Install nvm
# RUN mkdir -p ${NVM_DIR} && \
# 	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v${NVM_VERSION}/install.sh | \
# 	PROFILE=/etc/bash.bashrc bash

# # Add NVM to PATH
# ENV PATH=${JAVA_HOME}/bin:/usr/local/bin:${PATH}
# SHELL ["/bin/bash", "-l", "-c"]

# # Verify installations
# RUN java --version && \
# 	clj --version && \
# 	bb --version && \
# 	source ${NVM_DIR}/nvm.sh && nvm --version
