FROM pgvector/pgvector:pg18

USER root

RUN set -eux; \
    . /etc/os-release; \
    export DEBIAN_FRONTEND=noninteractive; \
    # 1. 安装构建期所需的临时工具
    apt-get update; \
    apt-get install -y --no-install-recommends wget ca-certificates; \
    # 2. 添加 Groonga 官方 APT 源（使用其预构建 apt-source 包）
    wget -q -O /tmp/groonga.deb \
        "https://packages.groonga.org/debian/groonga-apt-source-latest-${VERSION_CODENAME}.deb"; \
    apt-get install -y /tmp/groonga.deb; \
    rm -f /tmp/groonga.deb; \
    # 3. 手动添加 PGDG 源（跳过 postgresql-common）
    wget -q -O /etc/apt/trusted.gpg.d/postgresql.asc \
        https://www.postgresql.org/media/keys/ACCC4CF8.asc; \
    echo "deb https://apt.postgresql.org/pub/repos/apt/ ${VERSION_CODENAME}-pgdg main" \
        > /etc/apt/sources.list.d/pgdg.list; \
    # 4. 刷新源并安装 PGroonga
    apt-get update; \
    apt-get install -y --no-install-recommends postgresql-18-pgdg-pgroonga; \
    # 5. 清理
    apt-get purge -y --auto-remove wget; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/*

USER postgres
