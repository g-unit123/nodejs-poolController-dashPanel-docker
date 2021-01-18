FROM node:lts-alpine as builder
ARG TAG=master
RUN apk add --no-cache git python build-base linux-headers udev
WORKDIR /work
RUN npx degit rstrouse/nodejs-poolController-dashPanel#${TAG}
RUN npm ci
RUN npm run build

FROM node:lts-alpine
WORKDIR /usr/local/nodejs-poolController-dashPanel
COPY --from=builder /work/node_modules .
# Consider moving these files to dist folder
COPY --from=builder /work/LICENSE /work/defaultConfig.json /work/package.json ./
COPY --from=builder /work .
EXPOSE 80
VOLUME [ "/usr/local/nodejs-poolController-dashPanel/data" ]
ENTRYPOINT ["node", "app.js"]
