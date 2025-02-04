FROM elasticsearch:8.15.5

# Install analysis-ik
RUN curl --retry 10 -S -L --output /tmp/elasticsearch-analysis-ik-8.15.5.zip https://release.infinilabs.com/analysis-ik/stable/elasticsearch-analysis-ik-8.15.5.zip \
    && unzip -o /tmp/elasticsearch-analysis-ik-8.15.5.zip -d /usr/share/elasticsearch/plugins/analysis-ik/ \
    && rm -f /tmp/elasticsearch-analysis-ik-8.15.5.zip
