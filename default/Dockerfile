FROM node:lts-alpine AS build
ARG TAG=master
RUN apk add --no-cache make gcc g++ python linux-headers udev tzdata git
WORKDIR /app
RUN npx degit rstrouse/nodejs-poolController-dashPanel#${TAG}
#RUN npm install
#RUN npm run build && npm prune --production
RUN npm ci
RUN npm run build
RUN npm ci --production

FROM node:lts-alpine as prod
#RUN mkdir /app && chown node:node /app && mkdir /app/data && chown node:node /app/data
RUN mkdir /app && chown node:node /app
WORKDIR /app
COPY --chown=node:node --from=build /app /app
#RUN mkdir /app/config_file
#RUN cp /app/defaultConfig.json /app/config_file/config.json
#RUN ln -s /app/config_file/config.json /app/config.json
USER node
EXPOSE 5150
ENV NODE_ENV=production
ENTRYPOINT ["node", "dist/app.js"]
