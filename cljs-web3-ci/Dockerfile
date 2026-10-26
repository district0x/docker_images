FROM cimg/clojure:1.12.6-openjdk-17.0 AS pull_bb
WORKDIR /tmp/
ARG BB_VERSION=1.13.226
RUN wget "https://github.com/babashka/babashka/releases/download/v${BB_VERSION}/babashka-${BB_VERSION}-linux-amd64-static.tar.gz" -q -O /tmp/babashka-static.tar.gz \
    && tar -xf /tmp/babashka-static.tar.gz

RUN ./bb --version


FROM cimg/clojure:1.12.6-openjdk-17.0
WORKDIR /home/circleci/project
LABEL com.circleci.preserve-entrypoint=true

ENV PATH /home/circleci/.yarn/bin:$PATH
ARG NODE_VERSION=24.22.1
ENV YARN_VERSION=1.22.22
ENV DISPLAY=":99"

USER root

ENV NODE_VERSION="${NODE_VERSION}"

RUN wget --no-verbose -O node.tar.gz "https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz" && \
  tar -xzf node.tar.gz -C /usr/local --strip-components=1 && \
	rm node.tar.gz && \
  ln -s /usr/local/bin/node /usr/local/bin/nodejs

RUN wget --no-verbose -O yarn.tar.gz "https://yarnpkg.com/downloads/${YARN_VERSION}/yarn-v${YARN_VERSION}.tar.gz" && \
	tar -xzf yarn.tar.gz -C /opt/ && \
	rm yarn.tar.gz && \
	ln -s /opt/yarn-v${YARN_VERSION}/bin/yarn /usr/local/bin/yarn && \
	ln -s /opt/yarn-v${YARN_VERSION}/bin/yarnpkg /usr/local/bin/yarnpkg

# Chromium headless (via chromium-browser)
RUN curl -sSL https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/google-chrome.gpg \
	&& echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] https://dl.google.com/linux/chrome/deb/ stable main" >  /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update && apt-get upgrade -yqq \
  && apt-get install -y --no-install-recommends \
        rlwrap \
        fonts-liberation \
        fontconfig \
        fonts-ipafont-gothic \
        fonts-wqy-zenhei \
        fonts-thai-tlwg \
        fonts-kacst \
        fonts-symbola \
        fonts-noto \
        fonts-freefont-ttf \
        google-chrome-stable \
	&& rm -rf /var/lib/apt/lists/*

RUN	printf '#!/bin/sh\nXvfb :99 -screen 0 1280x1024x24 &\nexec "$@"\n' | tee /docker-entrypoint.sh && \
	chmod +x /docker-entrypoint.sh
COPY --from=pull_bb /tmp/bb /usr/local/bin/bb

USER circleci
