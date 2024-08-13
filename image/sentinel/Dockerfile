# 使用适当的基础镜像进行安装
FROM amd64/buildpack-deps:buster-curl as installer

ARG VERSION=1.8.7
ARG PORT=8090

RUN echo "Based on commit: $VERSION"
RUN echo "Based on commit: $PORT"

RUN set -x \
    && curl -SL --output ./sentinel-dashboard.jar https://github.com/alibaba/Sentinel/releases/download/$VERSION/sentinel-dashboard-$VERSION.jar

# 使用适当的基础镜像运行应用程序
FROM openjdk:8-jre-slim

# 复制安装阶段的 Sentinel jar 文件
COPY --from=installer ["./sentinel-dashboard.jar", "/home/sentinel-dashboard.jar"]

# 定义环境变量
# ENV PORT=$PORT
# ENV JAVA_OPTS="-Dserver.port=${PORT} -Dcsp.sentinel.dashboard.server=localhost:${PORT}"

# RUN echo "Based on commit: $JAVA_OPTS"

RUN chmod +x /home/sentinel-dashboard.jar

# 暴露端口
EXPOSE $PORT

# 启动命令
CMD java -Dserver.port=$PORT -Dcsp.sentinel.dashboard.server=localhost:$PORT -jar /home/sentinel-dashboard.jar
