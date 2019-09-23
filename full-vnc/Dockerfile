FROM gitpod/workspace-full-vnc:latest

USER root

# CUSTOM // Erlang/Elixir ######

# Important!  Update this no-op ENV variable when this Dockerfile
# is updated with the current date. It will force refresh of all
# of the base images and things like `apt-get update` won't be using
# old cached versions when the Dockerfile is built.
ENV REFRESHED_AT=2019-07-12 \
    LANG=en_US.UTF-8 \
    HOME=/opt/app/ \
    TERM=xterm \
    ERLANG_VERSION=22.1.0 \
    ELIXIR_VERSION=1.9.1-otp-22

# Install custom tools, runtime, etc.
RUN apt-get update \
    # window manager
    && apt-get install -y jwm \
    # electron
    && apt-get install -y libgtk-3-0 libnss3 libasound2 \
    # native-keymap
    && apt-get install -y libx11-dev libxkbfile-dev \

    # CUSTOM // build tools
    && apt-get install build-essential git wget libssl-dev libreadline-dev libncurses5-dev zlib1g-dev m4 curl wx-common libwxgtk3.0-dev autoconf \
    && apt-get install libxml2-utils xsltproc fop unixodbc unixodbc-bin unixodbc-dev \

    && apt-get clean && rm -rf /var/cache/apt/* && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*

USER gitpod

# Apply user-specific settings
RUN bash -c ". .nvm/nvm.sh \
    && nvm install 10 \
    && nvm use 10 \
    && npm install -g yarn"

# CUSTOM // asdf-vm install
RUN cd \
    && git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.7.4 \
    # For Ubuntu or other linux distros
    && echo '. $HOME/.asdf/asdf.sh' >> ~/.bashrc \
    && echo '. $HOME/.asdf/completions/asdf.bash' >> ~/.bashrc \

    # CUSTOM // install erlang/elixir
RUN bash \
    && asdf plugin-add erlang \
    && asdf install erlang $ERLANG_VERSION \
    && asdf global erlang $ERLANG_VERSION \
    && asdf plugin-add elixir \
    && asdf install elixir $ELIXIR_VERSION \
    && asdf global elixir $ELIXIR_VERSION

# Give back control
USER root
