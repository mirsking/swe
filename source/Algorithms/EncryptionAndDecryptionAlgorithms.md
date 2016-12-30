<!-- toc -->

- [加密算法和解密算法](#加密算法和解密算法)
	- [加密算法的作用](#加密算法的作用)
	- [加密算法介绍](#加密算法介绍)
		- [散列算法：只能加密的算法](#散列算法只能加密的算法)
			- [SHA(Secure Hash Algorithm)](#shasecure-hash-algorithm)
			- [MD5(Message-Digest Algorithm 5)](#md5message-digest-algorithm-5)
		- [加解密算法：既能加密，也能解密](#加解密算法既能加密也能解密)
			- [对称加密算法](#对称加密算法)
				- [DES](#des)
				- [RC4](#rc4)
			- [非对称加密算法](#非对称加密算法)
				- [RSA](#rsa)
				- [DSA(Digital Signature Algorithm)](#dsadigital-signature-algorithm)
				- [ECC(Elliptic Curves Cryptography)](#eccelliptic-curves-cryptography)
	- [参考博客](#参考博客)

<!-- tocstop -->
# 加密算法和解密算法

## 加密算法的作用
* 保密性：防止用户的标识或数据被读取。
* 数据完整性：防止数据被更改。
* 身份验证：确保数据发自特定的一方。

## 加密算法介绍
### 散列算法：只能加密的算法
1. 散列算法具有如下特征：
* 散列是信息的提炼，通常其长度要比信息小得多，且为一个固定长度。
* 加密性强的散列一定是不可逆的，这就意味着通过散列结果，无法推出任何部分的原始信息。
* 任何输入信息的变化，哪怕仅一位，都将导致散列结果的明显变化，这称之为**雪崩效应**。
* 散列还应该是防冲突的，即找不出具有相同散列结果的两条信息。

散列结果一般用于产生消息摘要，密钥加密，从而验证信息是否被修改。

#### SHA(Secure Hash Algorithm)
可以对任意长度的数据运算生成一个160位的数值；

#### MD5(Message-Digest Algorithm 5)
信息-摘要算法5

### 加解密算法：既能加密，也能解密
#### 对称加密算法
加密和解密均采用同一把秘密钥匙，而且通信双方都必须获得这把钥匙，并保持钥匙的秘密。
##### DES
1. DES(Data Encryption Standard)
  1997年美国国家标准局公布的“美国数据加密标准（DES）。数据加密标准，速度较快，适用于加密大量数据的场合。
2. 3DES(Triple DES)
  基于DES，对一块数据用三个不同的密钥进行三次加密，强度更高。
3. AES(Advanced Encryption Standard)
  高级加密标准，是下一代的加密算法标准，速度快，安全级别高；
##### RC4
ARCFOUR开发密码系统的商标名称

#### 非对称加密算法
采用不同的加密钥匙（公钥）和解密钥匙（私钥）。
##### RSA
三位数学家Rivest、Shamir、Adleman
##### DSA(Digital Signature Algorithm)
数字签名算法，是一种标准的 DSS（数字签名标准）
##### ECC(Elliptic Curves Cryptography)
椭圆曲线密码编码学

## 参考博客

http://blog.csdn.net/zuiyuezhou888/article/details/7557048
