# lua53-AES

非原创。搬运，解读，应用。

纯Lua实现，测试Lua 5.3，支持128/192/256bit、EBC/CBC、ZeroPadding，AES。

相关内容包含Padding With ISO10126，或可整合。

## `AES.lua`

`BaseFun_AES` = `AES_Class`，后续简称AES。
需实例化后使用，实例化方法为`AES:new()`，后续实例简称aes。

#### `set_key`

`AES:set_key(key , keylen)`

私有。

- `key`：密钥。[字节数组](#字节数组)。
- `keylen`：密钥长度——16 / 24 / 32 （128/192/256bits）。

参见[`GetStringKey`](#GetStringKey)。

#### `encrypt`

 `aes:encrypt(plain , pPos , cipher , cPos)`

- `plain`：明文。[字节数组](#字节数组)。
- `cipher`：密文。输出。[字节数组](#字节数组)。

#### `decrypt`

`aes:decrypt(cipher , pPos , plain , cPos)`

- `cipher`：密文。[字节数组](#字节数组)。
- `plain`：明文。输出。[字节数组](#字节数组)。

#### `ecb_EncryptDecrypt`

`strData_output = aes:ecb_EncryptDecrypt(strData , strKey , bEncrypt)`

- `strData` ：字符串。
- `bEncrypt`：加密/解密。布尔。`true` - Encrypt，`false` - Decrypt。
- `strData_output`：字符串。

|                  | Encrypt | Decrypt |
| ---------------- | ------- | ------- |
| `bEncrypt`       | `true`  | `false` |
| `strData`        | plain   | cipher  |
| `strData_output` | cipher  | plain   |

#### `cbc_EncryptDecrypt`

`strData_output = aes:cbc_EncryptDecrypt(strData , strKey , strIV , bEncrypt)`

- `strData` ：字符串。
- `bEncrypt`：加密/解密。布尔。`true` - Encrypt，`false` - Decrypt。
- `strIV`：初始向量。字符串。
- `strData_output`：字符串。

|                  | Encrypt | Decrypt |
| ---------------- | ------- | ------- |
| `bEncrypt`       | `true`  | `false` |
| `strData`        | plain   | cipher  |
| `strData_output` | cipher  | plain   |

### `GetStringKey`

`byteList_key = AES.GetStringKey(strKey)`

私有。
包含“零填充”，填充至16/24/32位。
填充后传递给[`set_key`](#set_key)。

#### 其他函数

略。

## `test.lua`

- `Plain_ByteList`：常量，标准值。明文。[字节数组](#字节数组)。
  原名`AES_EData`（Encrypt Data？）。
- `Cipher`_ByteList：常量，标准值。密文。[字节数组](#字节数组)。
  原名`AES_DData`（Decrypt Data？）。
- `plain_ByteList`：返回的结果。明文。[字节数组](#字节数组)。
- `cipher_ByteList`：返回的结果。密文。[字节数组](#字节数组)。
- `plain_Str`：返回的结果。密文。字符串。
- `ciyher_Str`：返回的结果。密文。字符串。

## `util.lua`

### `toHexString`

`byteList_HexString = toHexString(data)`

- `data`：支持32位`number`、number in `table`、`string`。
- `byteList_HexString` ：形如`'01 02 .. FF'`。

自[aeslua/util.lua at master · bighil/aeslua (github.com)](https://github.com/bighil/aeslua/blob/master/src/aeslua/util.lua)。

### `bytesToHex`

`str = byteListToString(byteList)`

- `byteList`：[字节数组](#字节数组)。

### [aeslua/util.lua at master · bighil/aeslua (github.com)](https://github.com/bighil/aeslua/blob/master/src/aeslua/util.lua)

包含可用功能：

- `padByteString` / `unpadByteString`：使用到随机数，貌似为ISO10126。
- `getByte`: get byte at position index
- `putByte`: put number into int at position index
- `bytesToInts`: convert byte array to int array
- `intsToBytes`: convert int array to byte array

## 术语

<a name="字节数组">字节数组</a> 形如`{0x01,0x02,..}`。

## 其他库

[bighil/aeslua: Implementation of aes in nearly pure lua (bitlib is required) (github.com)](https://github.com/bighil/aeslua)

支持AES128 / AES192 / AES256、ECB/CBC/OFB/CFB，Padding with ISO10126?
有多个测试例。

问题：

- 依赖`bitlib`。
  分支[gdyr/aeslua53: Implementation of AES in pure Lua 5.3 \(based on bighil/aeslua) (github.com)](https://github.com/gdyr/aeslua53)使用Lua 5.3位操作。
- 不支持iv传入。
  分支[TobleMiner/aeslua at feature-iv (github.com)](https://github.com/TobleMiner/aeslua/tree/feature-iv)加入支持。
- 貌似数据长度有问题，结果与外部[工具](#工具)测试不一致、不兼容（解密报错）。

## 工具

[AES在线解密 AES在线加密 Aes online hex 十六进制密钥 - The X 在线工具 (the-x.cn)](https://the-x.cn/cryptography/Aes.aspx)
