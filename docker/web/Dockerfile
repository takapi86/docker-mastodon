FROM rubylang/ruby:2.6.6-bionic

RUN apt update \
    && apt install -y autoconf \
                      bison \
                      build-essential \
                      curl \
                      ffmpeg \
                      file \
                      git-core \
                      g++ \
                      gcc \
                      imagemagick \
                      libffi-dev \
                      libgdbm-dev \
                      libicu-dev \
                      libidn11-dev \
                      libjemalloc-dev \
                      libncurses5-dev \
                      libpq-dev \
                      libprotobuf-dev \
                      libreadline6-dev \
                      libssl-dev \
                      libxml2-dev \
                      libxslt1-dev \
                      libyaml-dev \
                      pkg-config \
                      protobuf-compiler \
                      zlib1g-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt install -y nodejs

RUN npm install --global yarn

COPY ./src/mastodon /var/www/app

ENV RAILS_ENV=development
ENV BUNDLE_PATH=/usr/local/bundle

WORKDIR /var/www/app

RUN bundle install
RUN bundle exec rails assets:precompile
CMD bash -c "rm tmp/pids/server.pid 2>/dev/null; bundle exec rails s -p 3000 -b '0.0.0.0' "
