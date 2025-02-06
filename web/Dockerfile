FROM node:18 AS base

CMD [ "bash" ]

# ビルド
FROM base AS test-and-build
# baseイメージの成果物をbuildイメージの/workdir配下にコピー
COPY . /workspace
# workdirの指定(Reactのコマンドは/workspaceで実行する必要があるため)
WORKDIR /workspace
# 変数の指定(github actionsで一時的な変数を入れてあげる)
ARG REACT_APP_API_SERVER
# React動かすためのコマンドインストール、Reactコードテストの実施、/workspace/build配下にビルド
RUN npm ci && npm run test && npm run build

# nginxにホスティング
FROM nginx:latest AS production
# buildイメージ/workspace/build配下の成果物(実行用ファイル)をproductionイメージ/var/www配下にコピー
COPY --from=test-and-build /workspace /var/www
# nginxの設定ファイルをコピー(apiコンテナにAPIリクエスト流すため)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# コマンドはbashのままでOK
# CMD [ "bash" ]