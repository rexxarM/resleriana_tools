
# resleriana_tools
Tool to play around with Atelier Resleriana ( レスレリアーナのアトリエ ) encrypted AssetBundle files

Fork from https://github.com/hax0r31337/resleriana_tools

## Improvement:

Provide feature: resumes transmission at break-points.

Display progress during downloading.

## Usage:

```
git clone https://github.com/rexxarM/resleriana_tools

cd resleriana_tools

# change BASE_URL in main/main.go if needed

go run ./main
```

## Proxy

Using system proxy by default. Confirm you can access https://asset.resleriana.jp/asset/1697082844_f3OrnfHInH1ixh1s/Android/catalog.json normally, or you may need a proxy.

Set proxy:
```
In Powershell

$ENV:HTTP_PROXY='http://<proxy_host>:<proxy_port>'
```

```
In bash/zsh
export all_proxy=http://<proxy_host>:<proxy_port>
```
## File Format

### Header
| Name           | Size | Type       | Default | Comment                                                    |  
|----------------|:----:|------------|:-------:|------------------------------------------------------------|
| Magic Number   | 4    | \[\]byte   | "Aktk"  | |
| Version        | 2    | uint16     | 0x01    | |   
| Reserved       | 2    | uint16     | 0x00    | Why does the game checks it must be 0, what's the purpose |
| Encryption     | 4    | enum       | 0x01    | 0x00 => None, 0x01 => Encrypted |
| MD5 Checksum   | 16   | \[\]byte   |         | Hash of the rest of the file whether the file encrypted or not |

### Encryption
A modified version of HChaCha which generates 512-bytes xor block.
Each block generation do 8 normal HChaCha block generation with chained initial state.   
I'll explain details of the format later, you can check out `./encryptor/keygen.go` for details.

## Caution
Unsafe zero-copy type conversion is used to improve performance