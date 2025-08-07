FROM node:15.10.0-buster-slim

ENV DEBIAN_FRONTEND=noninteractive

# dash va foydalanuvchilarni sozlash
RUN dpkg-reconfigure -p critical dash
RUN for i in $(seq 1001 1500); do \
        groupadd -g $i runner$i && \
        useradd -M runner$i -g $i -u $i ; \
    done

# 👉 MUHIM: arxivdagi manbalarni o‘rnatish
RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list && \
    sed -i 's|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y libxml2 gnupg tar coreutils util-linux libc6-dev \
    binutils build-essential locales libpcre3-dev libevent-dev libgmp3-dev \
    libncurses6 libncurses5 libedit-dev libseccomp-dev rename procps python3 \
    libreadline-dev libblas-dev liblapack-dev libpcre3-dev libarpack2-dev \
    libfftw3-dev libglpk-dev libqhull-dev libqrupdate-dev libsuitesparse-dev \
    libsundials-dev libpcre2-dev && \
    rm -rf /var/lib/apt/lists/*

# Lokal sozlamalar
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

# Loyihani o‘rnatish
WORKDIR /piston_api
COPY ["package.json", "package-lock.json", "./"]
RUN npm install
COPY ./src ./src

# Kompilyatsiya qismi
RUN make -C ./src/nosocket/ all && make -C ./src/nosocket/ install

# Tarmoq va ishga tushirish
EXPOSE 2000/tcp
CMD ["node", "src"]
