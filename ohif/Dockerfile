######################################################

FROM node:20.13.1-alpine3.19 as builder

RUN apt-get update && apt-get install -y --no-install-recommends git=2.45.1 && rm -rf /var/lib/apt/lists/*

WORKDIR /sources
RUN git clone https://github.com/OHIF/Viewers.git
WORKDIR /sources/Viewers
RUN git checkout v3.8.0

WORKDIR /sources/Viewers

RUN yarn install --frozen-lockfile --verbose

ENV QUICK_BUILD true
# ENV GENERATE_SOURCEMAP=false
# ENV REACT_APP_CONFIG=config/default.js

RUN yarn install && QUICK_BUILD=true PUBLIC_URL=/ohif/ yarn run build

######################################################

FROM nginx:1.25.5-alpine3.19

RUN mkdir /etc/nginx/enabled-sites && mkdir /scripts

COPY ohif-static.conf /etc/nginx/enabled-sites/

COPY --from=builder /sources/Viewers/platform/app/dist /usr/share/nginx/html/
COPY default-app-config.js /usr/share/nginx/html/app-config.js