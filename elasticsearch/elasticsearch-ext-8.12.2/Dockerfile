from elasticsearch:8.12.2

# Install analysis-ik
RUN curl --retry 10 -S -L --output /tmp/elasticsearch-analysis-ik-8.12.2.zip https://github.com/infinilabs/analysis-ik/releases/download/v8.12.2/elasticsearch-analysis-ik-8.12.2.zip \
    && unzip -o /tmp/elasticsearch-analysis-ik-8.12.2.zip -d /usr/share/elasticsearch/plugins/analysis-ik/ \
    && rm -f /tmp/elasticsearch-analysis-ik-8.12.2.zip
