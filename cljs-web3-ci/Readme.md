# cljs-web3 Docker image for CI

This is a slightly tweaked circle-ci image featuring components as below:
- Clojure `1.12.6` (JDK 17)
- Node.js `24.x` (default; override with `--build-arg NODE_VERSION=...`)
- Chromium-headless `101`
