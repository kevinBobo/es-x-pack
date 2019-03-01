# 替换x-pack-core-6.6.0.jar的class文件
```
mkdir org/elasticsearch/license/

mv LicenseVerifier.class org/elasticsearch/license/LicenseVerifier.class

jar uvf x-pack-core-6.3.0.jar org/elasticsearch/license/LicenseVerifier.class


mkdir org/elasticsearch/xpack/core/

mv LicenseVerifier.class org/elasticsearch/xpack/core/XPackBuild.class

jar uvf x-pack-core-6.3.0.jar org/elasticsearch/xpack/core/XPackBuild.class
```

# 开启ES的登录功能
## 1.重置登陆权限密码,默认为changeme
```bash
bin/elasticsearch-setup-passwords interactive
```

* 按步骤分别重置elastic/kibana等账号的密码
* elastic就是登陆elasticsearch服务的最高权限账号
## 2. 修改elasticsearch.yml配置
# 添加如下2行,打开安全配置功能
```yaml
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
```


## 3. 修改kibana配置
在kibana.yml下添加如下两行
```yaml
elasticsearch.username: elastic
elasticsearch.password: {你修改的password}
```

此处修改完后，重启ES和kibana服务就需要登陆账号和密码了
## 4.x-pack设置完毕后，head无法登陆的问题
在elasticsearch.yml中添加如下三行配置
```yaml
http.cors.enabled: true
http.cors.allow-origin: "*"
http.cors.allow-headers: Authorization,X-Requested-With,Content-Length,Content-Type
```

重启服务，并通过如下形式访问head端口
http://192.168.36.61:9100/?auth_user=elastic&auth_password=passwd
