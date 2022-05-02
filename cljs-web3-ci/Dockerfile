FROM cimg/clojure:1.11.1
LABEL com.circleci.preserve-entrypoint=true

ENV PATH /home/circleci/.yarn/bin:$PATH
ENV NODE_VERSION 12.22.12
ENV YARN_VERSION 1.22.15
ENV CHROME_VERSION 101
ENV DISPLAY=":99"

USER root

RUN curl -L -o node.tar.xz "https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz" && \
	tar -xJf node.tar.xz -C /usr/local --strip-components=1 && \
	rm node.tar.xz && \
	ln -s /usr/local/bin/node /usr/local/bin/nodejs

RUN curl -L -o yarn.tar.gz "https://yarnpkg.com/downloads/${YARN_VERSION}/yarn-v${YARN_VERSION}.tar.gz" && \
	tar -xzf yarn.tar.gz -C /opt/ && \
	rm yarn.tar.gz && \
	ln -s /opt/yarn-v${YARN_VERSION}/bin/yarn /usr/local/bin/yarn && \
	ln -s /opt/yarn-v${YARN_VERSION}/bin/yarnpkg /usr/local/bin/yarnpkg

# Chromium headless (via chromium-browser)
RUN curl -sSL https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
	&& echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >  /etc/apt/sources.list.d/google-chrome.list \
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
        google-chrome-stable="${CHROME_VERSION}*" \
	&& rm -rf /var/lib/apt/lists/*

RUN	printf '#!/bin/sh\nXvfb :99 -screen 0 1280x1024x24 &\nexec "$@"\n' | tee /docker-entrypoint.sh && \
	chmod +x /docker-entrypoint.sh

USER circleci
