FROM --platform=linux/amd64 opensuse/tumbleweed:20260723@sha256:cfee62057a1ab443ef9542135394bd1a2d90d97c6720504294c8d9883b4e4840

LABEL org.opencontainers.image.authors="Ankit Pati <contact@ankitpati.in>"
LABEL org.opencontainers.image.description="Ultimate command-line development environment for cloud, infrastructure, backend, and frontend applications."
LABEL org.opencontainers.image.licenses="GPL-3.0-or-later"
LABEL org.opencontainers.image.source="https://gitlab.com/ankitpati/dockerfiles/-/raw/main/src/openSUSE/Dockerfile"
LABEL org.opencontainers.image.title="Ultimate Command-Line Development Container"
LABEL org.opencontainers.image.url="https://quay.io/ankitpati/tigress"
LABEL org.opencontainers.image.vendor="Ankit Pati"
LABEL org.opencontainers.image.version="latest"

SHELL ["bash", "--login", "-o", "errexit", "-o", "nounset", "-o", "pipefail", "-c", "--"]

RUN \
    set -o errexit -o nounset -o pipefail; \
    cp /usr/etc/zypp/zypp.conf.d/excludedocs.conf /etc/zypp/zypp.conf.d/excludedocs.conf; \
    sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' --in-place /etc/zypp/zypp.conf.d/excludedocs.conf; \
    ( cd /etc/zypp/repos.d/; ls | while read -r repofile; do sed --in-place --regexp-extended 's,\bhttp://,https://,g' "$repofile"; done ); \
    zypper modifyrepo --enable repo-debug; \
    zypper addrepo --refresh --priority=90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman; \
    zypper addrepo 'https://packages.microsoft.com/yumrepos/vscode/' vscode; \
    zypper addrepo 'https://packages.microsoft.com/opensuse/15/prod/' microsoft-opensuse; \
    rpm --import 'https://packages.microsoft.com/keys/microsoft.asc'; \
    zypper \
        --gpg-auto-import-keys \
        --non-interactive \
        dist-upgrade \
            --allow-name-change \
            --allow-vendor-change \
            --auto-agree-with-licenses \
            --force-resolution \
   ; \
    function get_latest_packages { \
        for package_regex in "$@"; \
        do \
            zypper --no-refresh --quiet search "/^$package_regex\$/" | \
            cut --delimiter='|' --fields=2 | \
            tr --delete ' ' | \
            grep --invert-match --extended-regexp '^([+-]+|Name|)$' | \
            sort --reverse --version-sort | \
            head --lines=1; \
        done; \
    }; \
    \
    mapfile -t versioned_packages < <(get_latest_packages \
        'java-[0-9]+-openjdk-devel' \
        'java-[0-9]+-openjdk-jmods' \
        'libxml[0-9]+-tools' \
        'lua[0-9]+' \
        'python[0-9]+' \
        'python[0-9]+-Django' \
        'python[0-9]+-black' \
        'python[0-9]+-curses' \
        'python[0-9]+-devel' \
        'python[0-9]+-flake8' \
        'python[0-9]+-mypy' \
        'python[0-9]+-pip' \
        'python[0-9]+-pre-commit' \
        'python[0-9]+-psycopg[0-9]+' \
        'python[0-9]+-pylint' \
        'python[0-9]+-ty' \
        'python[0-9]+-uv' \
        'python[0-9]+-zxcvbn' \
        'ruby[0-9.]+-rubygem-haml' \
        'xdelta[0-9]+' \
    ); \
    \
    zypper \
        --no-refresh \
        --non-interactive \
        install \
            --allow-name-change \
            --allow-vendor-change \
            --auto-agree-with-licenses \
            --force-resolution \
            "${versioned_packages[@]}" \
            7zip \
            ImageMagick \
            R-base \
            ShellCheck \
            ack \
            ant \
            apache-ivy \
            aria2 \
            arkade \
            astyle \
            bash-completion \
            bat \
            bind-utils \
            binwalk \
            bison \
            brotli \
            buildah \
            buildpacks-cli \
            cargo \
            checksec \
            clang \
            clang-devel \
            cnf-bash \
            code \
            coreutils-doc \
            cpio \
            csvprintf \
            curl \
            diff-pdf \
            dive \
            dos2unix \
            dwarves \
            exiftool \
            fd \
            findutils \
            flex \
            fortio \
            gcc \
            gcc-c++ \
            gdb \
            ghostscript \
            git \
            git-delta \
            git-filter-repo \
            glibc-debuginfo \
            gnu-netcat \
            gnu_parallel \
            go \
            go-doc \
            gron \
            gzip \
            helm \
            hexyl \
            highlight \
            hostname \
            hurl \
            indent \
            iproute2 \
            iputils \
            jq \
            just \
            kind \
            kubectl-explore \
            kubernetes-client \
            kustomize \
            libcap-progs \
            libopenssl-devel \
            lsof \
            make \
            man \
            man-pages \
            maven \
            mergiraf \
            mesheryctl \
            mesheryctl-bash-completion \
            moreutils \
            nelm \
            neovim \
            nmap \
            npm-default \
            openssh \
            openssl \
            opentofu \
            patchutils \
            pdftk \
            perl-App-cpanminus \
            perl-File-Rename \
            perl-LWP-Protocol-https \
            perl-Modern-Perl \
            perl-Module-Signature \
            perl-Perl-Critic \
            perl-Perl-Tidy \
            perl-Term-ReadLine-Gnu \
            perl-Unicode-LineBreak \
            perl-doc \
            pnpm \
            podman \
            poppler-tools \
            postgresql \
            psmisc \
            pv \
            quilt \
            readline-devel \
            ripgrep \
            ruby \
            ruby-devel \
            rust \
            shadow \
            shfmt \
            skopeo \
            socat \
            steampipe \
            stern \
            strace \
            susepaste \
            susepaste-screenshot \
            tar \
            telnet \
            terminfo \
            tidy \
            tmux \
            tree \
            unzip \
            util-linux-tty-tools \
            valkey \
            vegeta \
            vim \
            vim-data \
            werf \
            xorg-x11-server-Xvfb \
            yasm \
            yt-dlp \
            zig \
            zip \
   ; \
    update-alternatives --get-selections | \
        cut --delimiter=' ' --fields=1 | \
        grep --invert-match '^ld$' | \
        while read -r invocable; \
        do \
            update-alternatives --list "$invocable" | \
            sort --reverse --version-sort | \
            head --lines=1 | \
            xargs update-alternatives --set "$invocable"; \
        done \
   ; \
    mkdir /run/tmux; \
    chmod 1777 /run/tmux; \
    useradd --no-log-init -- "tigress"; \
    brew_prefix=/home/linuxbrew/.linuxbrew; \
    mkdir --parents -- "$brew_prefix"; \
    chmod 0700 -- "$brew_prefix"; \
    chown -- tigress:tigress "$brew_prefix"; \
:

USER tigress

WORKDIR /home/tigress

RUN \
    set -o errexit -o nounset -o pipefail; \
    git clone --depth=1 'https://gitlab.com/ankitpati/dotfiles.git'; \
    make --directory=dotfiles/ install; \
    rm --force --recursive dotfiles/; \
    \
    git clone --depth=1 'https://gitlab.com/ankitpati/scripts.git'; \
    make --directory=scripts/ install; \
    mv scripts/src/Brewfile "$HOME/Brewfile"; \
    rm --force --recursive scripts/; \
    \
    mkdir --parents -- "$HOME/.postgresql/"; \
    cat <(curl --fail --show-error --silent 'https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem') \
        <(curl --fail --show-error --silent 'https://truststore.pki.us-gov-west-1.rds.amazonaws.com/global/global-bundle.pem') \
        > "$HOME/.postgresql/root.crt"; \
    \
    curl --fail --show-error --silent 'https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz' \
        | tar --extract --gzip; \
    google-cloud-sdk/install.sh --quiet --command-completion=true --path-update=true; \
    bash --login -c 'gcloud components update --quiet'; \
    \
    bash --login <(curl --fail --show-error --silent 'https://get.sdkman.io'); \
    \
    bash --login \
        <(curl --fail --show-error --silent 'https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh' \
          | perl -0pE 's/^have_sudo_access\(\) {.*?^}/have_sudo_access() { HAVE_SUDO_ACCESS=0; return 0; }/sm' \
          | perl -0pE 's/^execute_sudo\(\) {.*?^}/execute_sudo() { ohai "\$\@"; execute "\$\@"; }/sm'); \
    \
    bash --login -c '\
        set -o errexit -o nounset -o pipefail; \
        brew analytics off; \
        brew developer on; \
        brew bundle; \
    '; \
    \
    bash --login -c '\
        set -o errexit -o nounset -o pipefail; \
        cpanm \
            Digest::SHA \
        ; \
        gem install --user-install --no-format-executable \
            csvlint \
        ; \
        npm install --global npm; \
        hash -r; \
        npm install --global \
            eslint \
            prettier \
        ; \
        sdk install \
            gradle \
        ; \
        bazel version; \
        rm \
            Brewfile \
        ; \
        clean-cache; \
    '; \
:

ENTRYPOINT ["bash", "--login"]
