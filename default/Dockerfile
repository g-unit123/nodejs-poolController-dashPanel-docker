FROM node:lts-alpine AS build
ARG TAG=master
RUN apk add --no-cache git python3 build-base linux-headers udev
WORKDIR /app
RUN npx degit rstrouse/nodejs-poolController-dashPanel#${TAG}
RUN npm ci
RUN npm run build
RUN npm ci --production

FROM node:lts-alpine
RUN apk add git
RUN mkdir /app && chown node:node /app && mkdir /app/data && chown node:node /app/data
WORKDIR /app
COPY --chown=node:node --from=build /app .
EXPOSE 5150
VOLUME [ "/app/data" ]
USER node
ENV NODE_ENV=production
ENV POOL_HTTP_IP=192.168.0.92
ENTRYPOINT ["node", "dist/app.js"]
