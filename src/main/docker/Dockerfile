FROM ubuntu:23.04
RUN apt-get update -y
RUN apt-get install -qq -y unzip zip wget curl gdbserver libc6-dev libc6-dbg zlib1g-dev gcc
RUN apt-get install -qq -y gdb
RUN curl -s "https://get.sdkman.io" | bash
RUN bash -c "source /root/.sdkman/bin/sdkman-init.sh; sdk install java 17.0.8-graal"
RUN bash -c "source /root/.sdkman/bin/sdkman-init.sh; sdk install java 20.0.2-graal"
RUN bash -c "source /root/.sdkman/bin/sdkman-init.sh; sdk install java 21-graal"
ENV GRAALVM_HOME=/root/.sdkman/candidates/java/21-graal

RUN ${GRAALVM_HOME}/bin/native-image --version

RUN update-alternatives --install /usr/bin/java java /root/.sdkman/candidates/java/17.0.8-graal/bin/java 0
RUN apt-get install -qq -y openjdk-17-jre maven
ENV M2_HOME=/usr/share/maven
