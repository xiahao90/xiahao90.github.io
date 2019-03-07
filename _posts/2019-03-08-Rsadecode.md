---
layout: post
title:  PHP-Rsa私钥加密，公钥解密
tags:
- RSA
- php加密
- 加密
description: RSA php加密 加密
---

------
    /**
    *私钥加密
    */
    function rsaEnCode($data,$priKeyStr){
        $privateKey = openssl_pkey_get_private($priKeyStr);
        $encryptData='';
        if (openssl_private_encrypt($data, $encryptData, $privateKey)) { 
        // 加密后 可以base64_encode后方便在网址中传输 
           return base64_encode($encryptData); 
        } else { 
            exit('加密失败');
        }
    }
    /**
    *公钥解密
    */
    function rsaDeCode($data,$pubKeyStr){
        $decryptData ='';
        $data=base64_decode($data);
        $publicKey = openssl_pkey_get_public($pubKeyStr); 
        if (openssl_public_decrypt($data, $decryptData, $publicKey)) { 
            return $decryptData;
        } else { 
            exit('解密失败'); 
        }
    }
    $priK='-----BEGIN PRIVATE KEY-----
    MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAKh6xj1T8IBkFU1z
    vCjksPWmZDjOkTUMmsXIwBwkhB29j2S9HLUwuHkaQmhkSJezIqmWuUYYTLhvY/NK
    L+3myeUF3CWoDERpNK0Z8+XhQ/zFCbWF/uZfftGGkfzeXk/DVAlOxeTmEdeirX7c
    jVcTfeg2g/3pkI0V6iOSyA78f1sBAgMBAAECgYBTePC69dmUYwHC6/JveE7MhnHl
    S0LaO7J7B6lWyCjE62QeDywoAUmEhqQcQINO1av4xfppZiSsxFOK+Fiiz9N63OuO
    XDsU+dBmEY6lCiJmHPHURizNxM3YDQTfIzwxxzx9nNW0zQffFHHlGobDHVTyS9+9
    sw/SRnleELazG/43OQJBANeK+I9BtmDUUd8rsSzg+SUibMaPfbb21Qx/2VBtYV5H
    OPKJzsLIE0IZuRZSukuZtuvyWVeCfYljPb10SxFdC1MCQQDIGl7xQ6WJTcQvN9tz
    B/sxlpJm5yOEWIRAciK70RIxiFY9RVx1h+TLao65OJjOc1dNbDZHrdzqDVnHGRu2
    JEnbAkB+tdzfYrMTg13KA4Z5qpvsv6sQo4Q1DjA60+HuDQ9DfD03H9pj2Xiw0Cxl
    uiZ4kRne5S6uajDglm3SNU54G6mvAkEAtc7H/puA1tyyaFhu6fgRcf+lqxtNON0X
    5FoPVuhhiGGgQXE4zYyYA34giQd0RvejFbRBKSU99+YDcRNOC1KzSwJAA+a9/5oE
    cisCL+JFxShNRmFOg0o9/2/BZl6fsereOKIPuiq85RCHMn5cPm9EXrvYwcdpnyrF
    3mSO1k9GSQ+JTA==
    -----END PRIVATE KEY-----';
    $pubK='-----BEGIN PUBLIC KEY-----
    MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCoesY9U/CAZBVNc7wo5LD1pmQ4
    zpE1DJrFyMAcJIQdvY9kvRy1MLh5GkJoZEiXsyKplrlGGEy4b2PzSi/t5snlBdwl
    qAxEaTStGfPl4UP8xQm1hf7mX37RhpH83l5Pw1QJTsXk5hHXoq1+3I1XE33oNoP9
    6ZCNFeojksgO/H9bAQIDAQAB
    -----END PUBLIC KEY-----';
    $data=json_encode(array('a'=>1,'tme'=>time()));
    $code=rsaEnCode($data,$priK);
    echo $code;
    echo "\r\n";
    echo rsaDeCode($code,$pubK);
    exit();
