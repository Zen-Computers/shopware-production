#syntax=docker/dockerfile:1.4

# pin versions
FROM ghcr.io/friendsofshopware/production-docker-base:8.2 as base-image
FROM ghcr.io/friendsofshopware/shopware-cli:0.1.71-php-8.2 as shopware-cli

# build shopware

FROM shopware-cli as build

ADD . /src
WORKDIR /src

RUN --mount=type=secret,id=composer_auth,dst=/src/auth.json \
    --mount=type=cache,target=/root/.composer \
    --mount=type=cache,target=/root/.npm \
    /usr/local/bin/entrypoint.sh shopware-cli project ci /src

# final image

FROM base-image

COPY --from=build --chown=82 --link /src /var/www/html
