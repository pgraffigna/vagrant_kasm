FROM kasmweb/core-ubuntu-bionic:1.10.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########

RUN apt-get update \ 
    && apt-get install -y openvpn \
    && apt-get install -y unzip

# Cambiar el fondo de pantalla
COPY assets/ub-wall.png  /usr/share/extra/backgrounds/bg_default.png

# Starship
RUN wget -q https://starship.rs/install.sh \
    && chmod +x install.sh \
    && ./install.sh -y

# Starship autostart
RUN echo 'eval "$(starship init bash)"' >> .bashrc

# Starship config
COPY assets/starship.toml .config/starship.toml

# Hack Nerd Font
RUN wget -q https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Hack.zip \
    && unzip Hack.zip -d /usr/local/share/fonts

# XFCE Dark 
RUN apt-get install numix-gtk-theme

######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000

## docker build -t pgraffigna/test-dbox:v1 .
## docker run --rm -it --shm-size=512m -p 6901:6901 -e VNC_PW=pass123 pgraffigna/test-dbox:v1 ## user:kasm_user
## docker run --detach -p6901:6901 --shm-size=512m -e VNC_PW=pass123 pgraffigna/xfce-dbox:v1
