FROM mysql:8-debian

# RUN apt-get update && \
#     apt-get -y install \
#             dos2unix \
#         --no-install-recommends && \
#         apt-get clean && \
#         rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY ./config/mysql.cnf /etc/mysql/conf.d/local.cnf

COPY ./docker-entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod 777 /usr/local/bin/entrypoint.sh
# RUN chmod 777 /usr/local/bin/entrypoint.sh && \
#     dos2unix /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

EXPOSE 3306 33060
CMD ["mysqld"]
