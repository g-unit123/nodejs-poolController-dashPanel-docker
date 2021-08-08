FROM node:lts-alpine AS build
ARG TAG=master
RUN apk add --no-cache make gcc g++ python linux-headers udev tzdata git
WORKDIR /app
RUN npx degit rstrouse/nodejs-poolController-dashPanel#${TAG}
RUN npm ci
RUN npm run build
RUN npm ci --production

FROM node:lts-alpine as prod
RUN apk add git
RUN mkdir /app && chown node:node /app
WORKDIR /app
COPY --chown=node:node --from=build /app /app
USER node
EXPOSE 5150
ENV NODE_ENV=production
ENTRYPOINT ["node", "dist/app.js"]
