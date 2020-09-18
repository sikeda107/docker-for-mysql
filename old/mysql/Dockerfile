#使うDockerイメージ
FROM mysql:5.6

#ポートを開ける
EXPOSE 3306

#MySQL設定ファイルをイメージ内にコピー
COPY ./my.cnf /etc/mysql/conf.d/my.cnf
# DB初期化用のファイルをイメージ内にコピー
COPY ./init/* /docker-entrypoint-initdb.d/

#docker runに実行される
CMD ["mysqld"]