from elasticsearch:7.9.3

# Install analysis-ik
RUN curl --retry 10 -S -L --output /tmp/elasticsearch-analysis-ik-7.9.3.zip https://github.com/infinilabs/analysis-ik/releases/download/v7.9.3/elasticsearch-analysis-ik-7.9.3.zip \
    && unzip -o /tmp/elasticsearch-analysis-ik-7.9.3.zip -d /usr/share/elasticsearch/plugins/analysis-ik/ \
    && rm -f /tmp/elasticsearch-analysis-ik-7.9.3.zip
