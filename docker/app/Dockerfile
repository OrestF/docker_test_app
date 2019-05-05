FROM ruby:latest

# Use virtual build-dependencies tag so we can remove these packages after bundle install
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs ruby-dev git yarn

# Set an environment variable where the Rails app is installed to inside of Docker image
ENV RAILS_ROOT /var/www/app_name

# Set an environment variable where the Rails app is installed to inside of Docker image
ENV RAILS_ROOT /var/www/app_name

# make a new directory where our project will be copied
RUN mkdir -p $RAILS_ROOT

# Set working directory within container
WORKDIR $RAILS_ROOT

# Setting env up
ARG RAILS_ENV
ENV RAILS_ENV=$RAILS_ENV
ENV RAKE_ENV=$RAILS_ENV
ENV RACK_ENV=$RAILS_ENV

# Adding gems
COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock

# development/production differs in bundle install
RUN gem install bundler
RUN if [[ "$RAILS_ENV" == "production" ]]; then\
  bundle install --jobs 20 --retry 5 --without development test;\
  else bundle install --jobs 20 --retry 5; fi

# Remove build dependencies and install runtime dependencies
# RUN apt install postgresql-client postgresql-libs nodejs tzdata

# Adding project files
COPY . .

RUN bundle exec rake assets:precompile

# EXPOSE 3000

# CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
ENTRYPOINT ["sh", "/var/www/app_name/docker/app/docker-entrypoint.sh"]