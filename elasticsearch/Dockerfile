FROM docker.elastic.co/elasticsearch/elasticsearch:5.4.0
MAINTAINER Chuan-Yen Chiang, cychiang0823@gmail.com
# switch into root user to install plugin
USER root
# install repository-s3 plugin to enable functionality of snapshot/restore
RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install repository-s3
# switch back to elasticsearch
USER elasticsearch
