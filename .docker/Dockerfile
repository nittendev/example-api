FROM node:20-alpine as development

WORKDIR /usr/src/app

COPY ./package.json ./
COPY ./yarn.lock ./

RUN apk update && apk upgrade && apk --no-cache add --virtual native-deps \
    g++ gcc libgcc libstdc++ linux-headers make python3 && \
    yarn global add node-gyp -g 

RUN yarn install --production=false --ignore-optional --non-interactive && \
    yarn cache clean && \
    rm -rf dist/*

COPY . .

RUN yarn build

FROM node:20-alpine as production

LABEL DATE_CREATED=20220310144005

WORKDIR /usr/src/app

COPY ./package.json ./
COPY ./yarn.lock ./

RUN yarn install --production --ignore-optional --non-interactive && \
    yarn cache clean && \
    rm -rf dist/*

COPY --from=development /usr/src/app/dist ./dist

EXPOSE 3116

CMD ["node", "dist/main"]
