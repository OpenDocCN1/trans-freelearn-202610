As you can see in the following code, the `Dockerfile` used by the Compose `release` service does the bulk of the work needed for the release. It starts by fetching the dependencies, compiling the application and the Phoenix assets, and then building the release. In the end, it copies `elixir_drip.tar.gz` to the `/tmp/elixir_drip` folder.

```
$ cat Dockerfile
FROM elixir:1.6.4-alpine

# Setting the needed environment variables
ENV APP_PATH /opt/app
ENV APP_NAME elixir_drip
ENV APP_VERSION 0.0.8
ENV HTTP_PORT 4000
ENV HTTPS_PORT 4040
ENV MIX_ENV prod

# Installing the dependencies
RUN apk add --no-cache build-base git nodejs nodejs-npm
RUN mix archive.install https://github.com/phoenixframework/archives/raw/master/phx_new.ez --force
RUN mix local.hex --force && mix local.rebar --force

# Copying the host current folder to the container
COPY . $APP_PATH/$APP_NAME
WORKDIR $APP_PATH/$APP_NAME

# Compiling the project and the Phoenix static assets
RUN MIX_ENV=$MIX_ENV mix do clean, deps.get, compile
RUN cd apps/elixir_drip_web/assets \
    && npm install \
    && ./node_modules/brunch/bin/brunch b -p

# Building the release
RUN cd $APP_PATH/$APP_NAME \
    && MIX_ENV=$MIX_ENV mix phx.digest \
    && MIX_ENV=$MIX_ENV mix release --env=$MIX_ENV

# Setting up the release to be copied
RUN mkdir -p /tmp/$APP_NAME
RUN cp $APP_PATH/$APP_NAME/_build/$MIX_ENV/rel/$APP_NAME/releases/$APP_VERSION/$APP_NAME.tar.gz /tmp/$APP_NAME

WORKDIR /tmp/$APP_NAME
RUN tar -xzf $APP_NAME.tar.gz
RUN rm -rf $APP_NAME.tar.gz
```