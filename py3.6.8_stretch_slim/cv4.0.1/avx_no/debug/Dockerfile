#from https://github.com/denismakogon/opencv-slim/blob/master/Dockerfile
FROM python:3.6.8-slim-stretch
#FROM python:3.6.8-slim-jessie

#NOTE: usare env per i parametri e' figo ma ti sembra di aver capito che invalida la cache!
#ogni RUN, ADD, COPY etc... genera un layer! 
#per ridurre il numero di layer, aggrega ovunque puoi con \ e &&
#COPY: docker controlla la checksum dei file: se non e' cambiata, usa la cache
#RUN: docker controlla la stringa: se non e' cambiata, usa la cache
#quando la cache su un layer viene invalidata (perche' e' cambiata la chksum di un file o la stringa
#di un RUN, tutti i layer successivi vengono ri-buildati)

#ATTENZIONE: le dimensioni dell'immagine crescono con il numero di layer!

#INTERESSANTE: https://learnk8s.io/blog/smaller-docker-images

#layer
RUN apt-get update \
    && apt-get install --no-install-recommends -qy build-essential \
                                                cmake \
                                                gcc \
                                                git \
                                                wget \
                                                unzip \
                                                yasm \
                                                pkg-config \
    && apt-get install --no-install-recommends -qy libtbb2 \
                                                libswscale-dev \
                                                libtbb-dev \
                                                libjpeg-dev \
                                                libpng-dev \
                                                libtiff-dev \
                                                libavformat-dev \
                                                libpq-dev \
    && wget -O /tmp/libjpeg-turbo8_1.4.2-0ubuntu3_amd64.deb "http://cz.archive.ubuntu.com/ubuntu/pool/main/libj/libjpeg-turbo/libjpeg-turbo8_1.4.2-0ubuntu3_amd64.deb" \ 
    && wget -O /tmp/libjpeg8_8c-2ubuntu8_amd64.deb "http://cz.archive.ubuntu.com/ubuntu/pool/main/libj/libjpeg8-empty/libjpeg8_8c-2ubuntu8_amd64.deb" \
    && wget -O /tmp/libjasper1_1.900.1-debian1-2.4ubuntu1.1_amd64.deb "http://security.ubuntu.com/ubuntu/pool/main/j/jasper/libjasper1_1.900.1-debian1-2.4ubuntu1.2_amd64.deb" \
    && wget -O /tmp/libjasper-dev_1.900.1-debian1-2.4ubuntu1.1_amd64.deb "http://security.ubuntu.com/ubuntu/pool/main/j/jasper/libjasper-dev_1.900.1-debian1-2.4ubuntu1.2_amd64.deb" \
    && dpkg -i /tmp/libjpeg-turbo8_1.4.2-0ubuntu3_amd64.deb \
    && dpkg -i /tmp/libjpeg8_8c-2ubuntu8_amd64.deb \
    && dpkg -i /tmp/libjasper1_1.900.1-debian1-2.4ubuntu1.1_amd64.deb \
    && dpkg -i /tmp/libjasper-dev_1.900.1-debian1-2.4ubuntu1.1_amd64.deb \
    && rm -fr /tmp/lib* \
    && pip install --upgrade pip \
    && pip install numpy \
    && apt-get install --no-install-recommends -qy openssh-server

 RUN apt-get remove --purge --auto-remove -y build-essential \
                                                cmake \
                                                gcc \
                                                git \
                                                wget \
                                                unzip \
                                                yasm \
                                                pkg-config \
      && apt-get remove --purge --auto-remove -y \
                                                #libswscale-dev \ #this is needed
                                                libtbb-dev \
                                                libjpeg-dev \
                                                libpng-dev \
                                                libtiff-dev \
                                                # libavformat-dev \ #this is needed
                                                libpq-dev \
      && apt-get clean \
      && apt-get autoclean \
      && apt-get autoremove \
      && rm -rf /tmp/* /var/tmp/* \
      && rm -rf /var/lib/apt/lists/* \
      && rm -f /var/cache/apt/archives/*.deb \
      && rm -f /var/cache/apt/archives/partial/*.deb \
      && rm -f /var/cache/apt/*.bin \
      && rm -rf /root/.[acpw]*

ENTRYPOINT "bash"