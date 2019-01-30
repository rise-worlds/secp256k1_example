# secp256k1_example

```c
//创建secp256k1_context与验证私钥
ctx = secp256k1_context_create(SECP256K1_CONTEXT_VERIFY | SECP256K1_CONTEXT_SIGN);
unsigned char seed[32] = {0};
result = secp256k1_context_randomize(ctx, seed);  //随机化
std::cout << "random:" << result << std::endl;

//创建根据私钥生成公钥
pubkeylen = 33;
result = secp256k1_ec_pubkey_create(ctx, pubkey, &pubkeylen, key, 1);
std::cout << "pubkey:" << result << std::endl;

//使用私钥签名 实际是对数据的sha256哈希值签名
siglen = 72;
result = secp256k1_ecdsa_sign(ctx, msg, sig, &siglen, key, nullptr, nullptr);
std::cout << "sign:" << result << std::endl;
result = secp256k1_ecdsa_verify(ctx, msg, sig, siglen, pubkey, pubkeylen);
std::cout << "verify:" << result << std::endl;

//销毁context
secp256k1_context_destroy(ctx);
```
