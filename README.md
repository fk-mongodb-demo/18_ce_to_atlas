# CE to Atlas

## Install CE 4.4.6

Download the installer from this page [Archives Download](https://www.mongodb.com/try/download/community-edition/releases/archive)

![image](https://github.com/user-attachments/assets/c013bfea-7a3e-4ac5-87d4-2887dc915b36)

Install libssl1.1

```
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```

Install MongoDB 4.4.6

```
dpkg -i mongodb-org-server_4.4.6_amd64.deb
```

Install Mongo Shell 4.4.6

```
dpkg -i mongodb-org-shell_4.4.6_amd64.deb
```
