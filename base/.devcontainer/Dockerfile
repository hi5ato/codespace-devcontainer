ARG VARIANT="noble"
FROM buildpack-deps:${VARIANT}-curl

LABEL dev.containers.features="common"

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y unminimize && yes | unminimize; apt-get clean

ENV TZ=Asia/Tokyo

RUN apt-get install -y locales
RUN /bin/echo -e "ja_JP.UTF-8 UTF-8\nen_US.UTF-8 UTF-8\nC.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen && \
    /usr/sbin/update-locale LANG=ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8

RUN apt-get install -y --no-install-recommends \
    aptitude less ugrep fzf bat fd-find vim fish moreutils htop lsof lv lftp zip unzip ncat socat ssh-import-id \
    && apt-get clean

RUN if [ "$VARIANT" = "noble" ] || [ "$VARIANT" = "oracular" ]; then \
        if id "ubuntu" &>/dev/null; then \
            echo "Deleting user 'ubuntu'  for $VARIANT" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user for $VARIANT"; \  
        else \
            echo "User 'ubuntu' does not exist for $VARIANT"; \ 
        fi; \
    fi

RUN chsh -s /usr/bin/fish root

RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash -   && \
    apt-get install -y nodejs && \
    apt-get clean