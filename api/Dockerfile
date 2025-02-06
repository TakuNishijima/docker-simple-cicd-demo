FROM gradle:7 AS base
CMD ["bash"]

# build
FROM base AS test-and-build
# baseイメージから/workspaceにコピー
COPY . /workspace
# gradleビルドのためにワーキングディレクトリを/workspaceに指定
WORKDIR /workspace
# gradleファイルのビルド
# gradlewをビルドするとき同時にテストもしてくれてる[
# gradlewの実行には実行権限の付与が必要
RUN chmod +x ./gradlew
RUN ./gradlew build

# production
FROM openjdk:17 AS production
# buildイメージのjarファイルをもってくる
COPY --from=test-and-build /workspace/build/libs/api-0.0.1-SNAPSHOT.jar ./api.jar
# jarファイルを実行
CMD [ "java", "-jar", "api.jar"]