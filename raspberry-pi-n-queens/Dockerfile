FROM resin/armv7hf-debian:stretch

# enable building ARM container on x86 machinery on the web (comment out next line if built on Raspberry)
RUN [ "cross-build-start" ]

# labeling
LABEL maintainer="lemariva@gmail.com" \
      version="V0.0.1.0" \
      description="Docker Application | Raspbian:Stretch"

# environment variables
ENV LEMARIVA_APPLICATION_VERSION 0.0.1.0

#install SSH & do users
RUN apt-get update \
	&& apt-get install -y openssh-server \
	&& echo 'root:root' | chpasswd \
    && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && mkdir /var/run/sshd

#install python & dependencies
RUN apt-get install -y screen python3 python3-dev python3-pip python3-setuptools\
	&& rm -rf /var/lib/apt/lists/*
    
#install python libraries
RUN python3 -m pip install pip --upgrade \
    && python3 -m pip install wheel --upgrade \ 
	&& python3 -m pip install tqdm --upgrade \ 
    && python3 -m pip install Flask --upgrade
    
#SSH Port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]

# set STOPSGINAL
STOPSIGNAL SIGTERM

# stop processing ARM emulation (comment out next line if built on Raspberry)
RUN [ "cross-build-end" ]