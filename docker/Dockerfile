FROM node:16-alpine

RUN apk add --update --no-cache \
                            git \
                            libzmq \
                            zeromq-dev \
                            python3 \
                            make \
                            g++

WORKDIR /insight

# Copy thoughtcore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/thoughtnetwork/thoughtcore-node.git .

# Copy config file
COPY thoughtcore-node.json .

ARG VERSION

# Install npm packages
RUN npm ci

# Install Insight API module
RUN bin/thoughtcore-node install @thoughtnetwork/insight-api@${VERSION}

FROM node:16-alpine

LABEL maintainer="Thought Developers <dev@thought.org>"
LABEL description="Dockerised Insight API"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/thoughtcore-node", "start"]
