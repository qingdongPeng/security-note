# openssl



## 1 生成key

 **openssl  genrsa  -out  rsa_private_pkcs1.key  2048**

 其中 rsa_private_pkcs1为key的名字 2048为长度

## 2 查看rsa内容

### 2.1 查看字符串内容

  命令 cat rsa_private_pkcs1.key 
   	 得到的内容如下
  -----BEGIN RSA PRIVATE KEY-----
      Base64 encode data
  -----END RSA PRIVATE KEY-----
开头和结尾格式固定, 中间内容为base64加密后的数据

### 2.2 查看各项参数
(module,publicExponent,privateExponent,prime1,prime2,exponent1,exponent2,coefficiento)
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

## 3 从私钥中生成公钥

  命令 openssl rsa -in rsa_private.pkcs1_key -pubout -out rsa_public_pkcs1.key 
  rsa_private.pkcs1_key 为私钥名
  rsa_public_pkcs1.key 为要生成的公钥名


## 4 从私钥中生成csr文件 

(csr即Certificate Signing Request,证书请求, 生成证书时需要将csr文件提交给权威的证书颁发机构, 即CA)
  命令 openssl req -new -key rsa_private_pkcs1.key -out rsa_private.csr
  需要输入一些信息,如城市,地址等
  rsa_private_pkcs1.key 为目标私钥
  rsa_private.csr 为要生成的csr文件

## 5 查看csr文件内容

###    5.1 查看文本内容

      命令  cat rsa_private.csr
      内容格式如下
-----BEGIN CERTIFICATE REQUEST-----
	base64 encode data
-----END CERTIFICATE REQUEST-----
     开头和结尾格式固定, 中间内容为base64加密后的数据
###    5.2 校验csr内容 (查看具体信息)

      命令 openssl req -text -in rsa_private.csr -noout -verify
      内容格式如下
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=CN, ST=SH, L=SH, O=PB, OU=PB, CN=PQD/emailAddress=pqd.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:bf:28:a0:de:65:34:aa:29:2e:c4:65:40:e3:e1:
                    52:0b
    	    .............
                Exponent: 65537 (0x10001)
        Attributes:
            unstructuredName         :pingbo
            challengePassword        :qwer123..
    Signature Algorithm: sha256WithRSAEncryption
    	 0e:0c:1f:f9:67:32:14:45:36:b7:60:8c:12:d6:0d:d1:e1:cb:
         37:fa:ed:8b
     .................
verify OK

## 6 由私钥生成crt文件

(certificate的缩写，即证书, 其实生成证书应该用csr文件提交到CA, 但是要花钱, 就可以用自己私钥进行签名, 生成证书玩玩了)

    命令  openssl  req -new -x509 -key rsa_private_pkcs1.key -out rsa_private.crt -days 3650
    rsa_private_pkcs1.key 为目标私钥
    rsa_private.crt 为生成的私钥
    3650 为过期天数
    注意:生成的过程中需要输入国家地区地址等


## 7 查看crt文件内容

###    7.1 查看文本内容

  命令  cat rsa_private.crt
  内容格式如下
-----BEGIN CERTIFICATE-----
    base64 encode data 
-----END CERTIFICATE-----
  内容格式固定, 中间内容为base64加密过的数据

###    7.2 查看文本内容

  命令  openssl x509 -text -in rsa_private.crt -noout
  rsa_private.crt 为待查看的crt文件
  内容格式如下
  Certificate:

    Data:
        Version: 3 (0x2)
        Serial Number:
            9f:d0:66:f2:cd:40:05:8b
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=SH, ST=SHANGHAI, L=MINHANG, O=PINGBP, OU=PINGBO, CN=PQD/emailAddress=PQD.COM
        Validity
            Not Before: Mar 31 13:13:36 2019 GMT
            Not After : Mar 28 13:13:36 2029 GMT
        Subject: C=SH, ST=SHANGHAI, L=MINHANG, O=PINGBP, OU=PINGBO, CN=PQD/emailAddress=PQD.COM
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:bf:28:a0:de:65:34:aa:29:2e:c4:65:40:e3:e1:
                    52:0b
    	    ..........
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                78:57:31:66:85:C9:A0:5E:4D:AB:6F:E1:02:BD:EE:4F:89:46:1B:33
            X509v3 Authority Key Identifier:
                keyid:78:57:31:66:85:C9:A0:5E:4D:AB:6F:E1:02:BD:EE:4F:89:46:1B:33
    
            X509v3 Basic Constraints:
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption
         4b:15:07:81:f8:ce:ce:d1:bf:f3:37:a1:6d:a1:a4:38:90:e7:
         69:77:c9:c1
     ...............


## 8 校验public key 跟 private key 是否匹配

   用modulus对csr, key , crt 生成对应的hash值, 如果三个值都一样, 则匹配
   命令 openssl rsa -modulus -in rsa_private_pkcs1.key -noout | openssl sha256

       输出 (stdin)= 054b1ff6013bf0ac7cacb0034124fa8de89cbad91f0049327e91fd6d1e132ae4
   命令 openssl req -modulus -in rsa_private.csr -noout | openssl sha256
       输出 (stdin)= 054b1ff6013bf0ac7cacb0034124fa8de89cbad91f0049327e91fd6d1e132ae4   
   命令 openssl x509 -modulus -in rsa_private.crt -noout | openssl sha256
       输出 (stdin)= 054b1ff6013bf0ac7cacb0034124fa8de89cbad91f0049327e91fd6d1e132ae4
  判断输出的hash值是否一样


## 9 查看公钥内容
   openssl rsa -in -pubin pub.key -noout -text
   pub.key为公钥名


