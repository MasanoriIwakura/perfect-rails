ARG RUBY_VERSION=3.2.2-slim-buster
ARG NODE_VERSION=18.16.0-slim

FROM node:${NODE_VERSION} AS node_image

FROM ruby:${RUBY_VERSION}

WORKDIR /tmp

RUN apt-get update -qq && apt-get install -y build-essential git libmariadb-dev

COPY --from=node_image /usr/local/bin/node /usr/local/bin/node
COPY --from=node_image /usr/local/bin/yarn /usr/local/bin/yarn

RUN useradd -m -u 1000 rails
USER rails

RUN gem install bundler

WORKDIR /app

COPY --chown=rails Gemfile Gemfile.lock /app/
# COPY --chown=rails Gemfile Gemfile.lock package.json yarn.lock /app/

RUN bundle install
# RUN yarn install

COPY --chown=rails . /app

RUN bin/rails assets:precompile

CMD ["bin/rails", "s", "-b", "0.0.0.0"]
