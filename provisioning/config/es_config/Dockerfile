FROM elasticsearch:5

COPY ./elasticsearch.yml /usr/share/elasticsearch/config/

EXPOSE 9200
EXPOSE 9300

ENV ES_JAVA_OPTS -Xms512M -Xmx512M
