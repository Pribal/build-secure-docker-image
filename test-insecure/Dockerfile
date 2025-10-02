# Bad practice : using latest
FROM ubuntu:latest  

# Bad practice : use root user
USER root

# Bad practice: Install package without cleaning cache
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    vim

# Add file
COPY hello.sh /usr/local/bin/hello.sh
RUN chmod +x /usr/local/bin/hello.sh

# Bad practice : no non-root user
CMD ["/usr/local/bin/hello.sh"]
