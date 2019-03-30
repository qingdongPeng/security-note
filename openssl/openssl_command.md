
 openssl genrsa -out rsa_private_pkcs1.key 2048
 其中 rsa_private_pkcs1OB为key的名字 2048为长度



2 查看rsa内容
  2.1 查看字符串内容
      命令 cat rsa_private_pkcs1.key 
       	 得到的内容如下
-----BEGIN RSA PRIVATE KEY-----
     Base64 encode data
-----END RSA PRIVATE KEY-----
        开头和结尾格式固定, 中间内容为base64加密后的数据

  2.2 查看各项参数(module,publicExponent,privateExponent,prime1,prime2,exponent1,exponent2,coefficiento)内容
      命令  openssl -rsa -text -in rsa_private_pkcs1.key -noout
      得到的内容格式如下 (8项参数如下,2048 bit表示该key的密码学长度为2048 bit)
Private-Key: (2048 bit)
modulus:
    00:bf:28:a0:de:65:34:aa:29:2e:c4:65:40:e3:e1:
    27:2e:bf:42:2b:6f:13:d8:72:ff:06:e8:74:9a:60:
    ............(省略了后面的长数据)
publicExponent: 65537 (0x10001)
privateExponent:
    63:ed:46:22:db:be:eb:10:ba:2c:da:4d:50:92:7b:
    31
    ............
prime1:
    00:f1:4a:f3:11:07:4a:b1:05:5b:d8:d0:42:57:fa:
    bc:18:86:6e:9b:35:b4:02:c9
    ............
prime2:
    00:ca:cf:67:69:4f:b9:d5:3f:21:ce:2a:63:57:bf:
    01:4f:21:e4:d9:51:1b:a4:33
    ............
exponent1:
    00:9f:60:7a:1c:8d:4c:70:90:b1:92:0c:3d:46:0f:
    3f:ca:93:41:0b:93:f5:4a:c1
    ............
exponent2:
    78:c6:92:9a:d0:73:a6:5e:76:4f:44:46:ec:d6:43:
    53:f6:a7:46:13:5b:16:a9
    ............
coefficient:
    06:80:fb:2f:01:33:a9:89:47:2c:b1:51:e5:32:cf:
    53:a2:1f:04:bd:53:9f:2b
    ............


3 从私钥中生成公钥
  命令 openssl rsa -in rsa_private.pkcs1_key -pubout -out rsa_public_pkcs1.key 
  rsa_private.pkcs1_key 为私钥名
  rsa_public_pkcs1.key 为要生成的公钥名

  te
