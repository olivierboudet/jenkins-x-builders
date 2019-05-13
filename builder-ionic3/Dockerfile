FROM gcr.io/jenkinsxio/builder-base:0.0.32

CMD ["gradle"]

ENV GRADLE_HOME /opt/gradle
ENV GRADLE_VERSION 4.1

RUN set -o errexit -o nounset \
    && echo "Set URLS" \
    && GRADLE_URL="https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
    && SHA256_URL="${GRADLE_URL}.sha256" \
    \
    && echo "Determine sha256sum" \
    && GRADLE_DOWNLOAD_SHA256=$(curl -sLf "${SHA256_URL}") \
    \
	&& echo "Downloading Gradle" \
	&& wget -q -O gradle.zip "${GRADLE_URL}" \
	\
	&& echo "Checking download hash" \
	&& echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum -c - \
	\
	&& echo "Installing Gradle" \
	&& unzip -q gradle.zip \
	&& rm gradle.zip \
	&& mkdir -p /opt \
	&& mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
	&& ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle


ENV ANDROID_VERSION 4333796
ENV ANDROID_HOME /opt/android-sdk-linux
RUN wget https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_VERSION}.zip && \
  unzip sdk-tools-linux-${ANDROID_VERSION}.zip -d android-sdk-linux && mv android-sdk-linux /opt/
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools
RUN yes | sdkmanager --licenses
RUN sdkmanager "platform-tools"
RUN yes | sdkmanager \
    "platforms;android-27" \
    "build-tools;28.0.0"

# node
RUN curl -f --silent --location https://rpm.nodesource.com/setup_11.x | bash -  && \
	yum install -y nodejs  && \
	yum clean all

# npm & yarn & cordova & ionic
ENV NPM_VERSION=6.9.0
ENV CORDOVA_VERSION=7.1.0
ENV IONIC_VERSION=3.20.1
ENV YARN_VERSION 1.3.2

RUN curl -f -L -o /tmp/yarn.tgz https://github.com/yarnpkg/yarn/releases/download/v${YARN_VERSION}/yarn-v${YARN_VERSION}.tar.gz && \	
	tar xf /tmp/yarn.tgz && \
	mv yarn-v${YARN_VERSION} /opt/yarn && \
	ln -s /opt/yarn/bin/yarn /usr/local/bin/yarn && \
	mkdir -p /.config /.yarn /.cordova

RUN yarn global add npm@"$NPM_VERSION" cordova@"$CORDOVA_VERSION" ionic@"$IONIC_VERSION" karma && \
    yarn cache clean --force

# jx
ENV JX_VERSION 2.0.117
RUN curl -f -L https://github.com/jenkins-x/jx/releases/download/v${JX_VERSION}/jx-linux-amd64.tar.gz | tar xzv && \
  mv jx /usr/bin/

RUN gradle --version
RUN yarn -version
RUN npm -version
RUN ionic -version
# run cordova to create config directories
RUN cordova -version --no-telemetry



