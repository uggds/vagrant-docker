FROM fluent/fluentd:latest-onbuild

USER fluent

WORKDIR /home/fluent
ENV PATH /home/fluent/.gem/ruby/2.3.0/bin:$PATH
RUN gem install fluent-plugin-secure-forward # セキリュティ対策
RUN gem install fluent-plugin-elasticsearch # Elasticsearch連携

EXPOSE 24284

CMD fluentd -c /fluentd/etc/fluent.conf -p /fluentd/plugins

