FROM node:latest
LABEL description="A demo Dockerfile for build Docsify."
# 克隆仓库
WORKDIR /
RUN git clone https://github.com/sivanWu0222/AlgoBook.git
WORKDIR /AlgoBook/docs
RUN npm install -g docsify-cli@latest
EXPOSE 9001/tcp
ENTRYPOINT docsify serve . -p 9001
