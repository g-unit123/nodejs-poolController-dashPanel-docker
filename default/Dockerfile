FROM node:lts-alpine AS build
ARG TAG=master
RUN apk add --no-cache git python3 build-base linux-headers udev
WORKDIR /work
RUN npx degit rstrouse/nodejs-poolController-dashPanel#${TAG}
RUN npm ci
RUN npm run build
RUN npm ci --production

FROM node:lts-alpine
RUN apk add git
RUN mkdir /app && chown node:node /app && mkdir /app/data && chown node:node /app/data
WORKDIR /app
COPY --chown=node:node --from=build /app /app
EXPOSE 5150
VOLUME [ "/usr/local/nodejs-poolController-dashPanel/data" ]
USER node
#FROM node:lts-alpine as prod
#RUN apk add git
#RUN mkdir /app && chown node:node /app
#WORKDIR /app
#COPY --chown=node:node --from=build /app /app
#USER node
#EXPOSE 5150
ENV NODE_ENV=production
ENTRYPOINT ["node", "dist/app.js"]
