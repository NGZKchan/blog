##### ビルド環境 #####
FROM node:12.14.0 as build-stage
WORKDIR /app
# vue.jsのProjectをコピーする
COPY ./vue ./   
RUN npm install
# npm run build で distファイルが生成されるscriptがpackage.jsonにある前提
RUN npm run build

##### 本番環境 #####
FROM nginx:1.17.6 as production-stage

ENV LANG C.UTF-8
ENV TZ Asia/Tokyo


# contentsを配置するディレクトリを作成する
COPY nginx/work/ /home/work
RUN mkdir -p /var/log/nginx/log\
    && mkdir /home/www\
    && mkdir /home/www/contents

# ビルド環境で構築されたdistディレクトリをnignxの該当のディレクトリに配置する
COPY --from=build-stage /app/dist /home/www/contents

# nginx.confファイルを配置する
WORKDIR /home/work
RUN rm -f /etc/nginx/conf.d/*.conf\
    && rm -f /etc/nginx/nginx.conf\
    && cp -i *.conf /etc/nginx

EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;","-c","/etc/nginx/nginx.conf"]