# 利用するOSイメージ
FROM ubuntu:18.04

# bash実行にする
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# アップデート + 必要ソフトインストール
RUN apt update && apt install -y \
        openssh-server python3-pip \
        sudo curl wget unzip

# 変数定義
ENV ADD_USER user
ENV USER_PASS pass

# userの追加
RUN useradd -m --groups sudo ${ADD_USER}
RUN echo ${ADD_USER}":"${USER_PASS} | chpasswd

# sshkeyの保存ディレクトリの作成
RUN mkdir /home/${ADD_USER}/.ssh/

# sshkeyの作成
RUN ssh-keygen -t rsa -b 4096 -N '' -f /home/${ADD_USER}/id_rsa
# すでに作成済みのものを再利用する場合
# COPY ./key/id_rsa /home/${ADD_USER}/id_rsa
# COPY ./key/id_rsa.pub /home/${ADD_USER}/id_rsa.pub

# ssh接続用の公開鍵のファイル名変更
RUN mv /home/${ADD_USER}/id_rsa.pub /home/${ADD_USER}/.ssh/authorized_keys

# 権限設定
RUN chmod 700 /home/${ADD_USER}/.ssh/
RUN chmod 600 /home/${ADD_USER}/.ssh/authorized_keys

# 所有者の設定
RUN chown -R ${ADD_USER}:${ADD_USER} /home/${ADD_USER}/.ssh/

# ssh用のフォルダの作成
RUN mkdir /var/run/sshd

# python3コマンドをpythonで使用できるように変更
RUN ln -sf /usr/bin/python3.6 /usr/bin/python
RUN ln -sf /usr/bin/python3.6 /usr/bin/python-config
RUN ln -sf /usr/bin/pip3      /usr/bin/pip

# ssh設定ファイルの変更(公開鍵での接続を受け入れる)
RUN sed -i 's|#AuthorizedKeysFile	.ssh/authorized_keys .ssh/authorized_keys2|AuthorizedKeysFile	.ssh/authorized_keys|g' /etc/ssh/sshd_config
RUN sed -i 's|#PubkeyAuthentication yes|PubkeyAuthentication yes|g' /etc/ssh/sshd_config
RUN sed -i 's|#PermitRootLogin prohibit-password|PermitRootLogin no|g' /etc/ssh/sshd_config
RUN sed -i 's|#PasswordAuthentication yes|PasswordAuthentication no|g' /etc/ssh/sshd_config
RUN sed -i 's|#PermitEmptyPasswords no|PermitEmptyPasswords no|g' /etc/ssh/sshd_config

# 秘密鍵をコピーしてsshdを起動する
RUN echo "#!/bin/bash" > /root/start.sh
RUN echo "mv /home/${ADD_USER}/id_rsa /usr/local/key/id_rsa" >> /root/start.sh
RUN echo "cp /home/${ADD_USER}/.ssh/authorized_keys /usr/local/key/id_rsa.pub" >> /root/start.sh
RUN echo "chown -R ${ADD_USER}:${ADD_USER} /usr/local/key" >> /root/start.sh
RUN echo "chown -R ${ADD_USER}:${ADD_USER} /usr/local/src" >> /root/start.sh
RUN echo "/usr/sbin/sshd -D" >> /root/start.sh

RUN chmod 700 /root/start.sh

CMD ["/root/start.sh"]

EXPOSE 22


