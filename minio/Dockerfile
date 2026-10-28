# 第一阶段：构建阶段
FROM golang:1.24-alpine AS builder

# 设置 Go 代理为国内源
ENV GOPROXY=https://goproxy.cn,direct

# 安装构建工具
RUN apk add --no-cache git make build-base

# 设置工作目录
WORKDIR /workspace

# 克隆最新版 minio 源码
#RUN git clone https://github.com/minio/minio.git .
COPY . /workspace/minio

WORKDIR /workspace/minio

# 直接运行 make（不指定 target）
RUN make

# 第二阶段：运行阶段
FROM alpine:latest

# 安装运行环境依赖
RUN apk add --no-cache ca-certificates tzdata

# 创建工作目录
WORKDIR /opt/minio

# 从构建阶段复制编译好的二进制文件
COPY --from=builder /workspace/minio /opt/minio/minio

# 暴露端口
EXPOSE 9000 9001

# 设置启动命令
ENTRYPOINT ["/opt/minio/minio"]
CMD ["server", "/data", "--console-address", ":9001"]