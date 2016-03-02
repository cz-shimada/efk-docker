FROM alpine:latest

# Do not split this into multiple RUN!
# Docker creates a layer for every RUN-Statement
# therefore an 'apk delete build*' has no effect
RUN apk --no-cache --update add \
                            build-base \
                            ca-certificates \
                            ruby \
                            ruby-irb \
                            ruby-dev \
                            bash gawk sed grep bc coreutils && \
    echo 'gem: --no-document' >> /etc/gemrc && \
    gem install fluentd -v 0.12.20 && \
    apk del build-base ruby-dev && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/*

RUN adduser -D -g '' -u 1000 -h /home/fluent fluent
RUN chown -R fluent:fluent /home/fluent

# for log storage (maybe shared with host)
RUN mkdir -p /fluentd/conf && \
    mkdir -p /fluentd/etc && \
    mkdir -p /fluentd/log && \
    mkdir -p /fluentd/buffer && \
    mkdir -p /fluentd/dest && \
    mkdir -p /fluentd/plugins

RUN chown -R fluent:fluent /fluentd

USER fluent
WORKDIR /home/fluent

# Tell ruby to install packages as user
RUN echo "gem: --user-install --no-document" >> ~/.gemrc
ENV PATH /home/fluent/.gem/ruby/2.2.0/bin:$PATH
ENV GEM_PATH /home/fluent/.gem/ruby/2.2.0:$GEM_PATH
ENV LANG ja_JP.UTF-8

ENV FLUENTD_OPT=""
ENV FLUENTD_CONF=""
ENV RUBY_GC_MALLOC_LIMIT=2000000
ENV RUBY_GC_MALLOC_LIMIT_MAX=4000000
ENV EXPOSE=24224

EXPOSE $EXPOSE

RUN gem install fluentd -v 0.12.16 && \
    gem install fluent-plugin-s3 && \
    gem install fluent-plugin-forest && \
    gem install fluent-plugin-elasticsearch -v 1.3.0 && \
    gem install fluent-plugin-grep && \
    gem install fluent-plugin-record-reformer && \
    gem install fluent-plugin-mail && \
    gem install fluent-plugin-rewrite && \
    gem install fluent-plugin-record-reformer && \
    gem install fluent-plugin-record-modifier && \
    gem install fluent-plugin-filter && \
    gem install fluent-plugin-suppress && \
    gem install fluent-plugin-rewrite-tag-filter && \
    gem install fluent-plugin-string-scrub && \
    gem install fluent-plugin-slack

COPY conf/* /fluentd/conf/

CMD exec fluentd -c $FLUENTD_CONF -p /fluentd/plugins $FLUENTD_OPT
