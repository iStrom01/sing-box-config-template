### 如果不能安装 warp-cli 可按如下方式处理

#### 安装 zstd
`apt install zstd`

#### 创建文件夹 && 下载 wgcf-cli
`
mkdir wgcf-cli && cd wgcf-cli && wget https://github.com/ArchiveNetwork/wgcf-cli/releases/download/v0.3.6/wgcf-cli-linux-64.tar.zstd
`

#### 解压
`
tar -xvf wgcf-cli-linux-64.tar.zstd
`

#### 注册
`
./wgcf-cli register
`

#### 生成 wireguard 出站配置
`
./wgcf-cli generate --sing-box
`

#### 查看生成的配置
`
cat wgcf.sing-box.json 
`

#### 复制并去掉server后面的:2408，并粘贴到 sing-box outbounds 模块，类似如下
```json
{
    "type": "wireguard",
    "tag": "wireguard-out",
    "server": "engage.cloudflareclient.com",
    "server_port": 2408,
    "local_address": [
        "172.16.0.2/32",
        "2606:4700:110:8755:9ea:4379:2b43:2785/128"
    ],
    "private_key": "",
    "peer_public_key": "",
    "reserved": "Xarv",
    "mtu": 1280
}
```

#### 修改对应路由规则到 wireguard 出站
```json
{
    "rule_set": [
        "geosite-openai",
        "geosite-bing",
        "geosite-netflix"
    ],
    "outbound": "wireguard-out"
}
```

#### 重启 sing-box
`
systemctl restart sing-box
`
