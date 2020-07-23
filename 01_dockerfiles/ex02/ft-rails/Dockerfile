FROM ruby
RUN apt-get update -yq && apt-get install -yq nodejs \
&& gem install rails -v 5.1.6 && gem install bundler
ONBUILD WORKDIR /opt
ONBUILD RUN rails new app
ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle install
ONBUILD RUN rails generate migration CreateProducts same:string
ONBUILD RUN rake db:migrate

#docker build -t ft-rails .
