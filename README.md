{
  "dns": {
    "final": "local-dns",
    "rules": [
      {
        "clash_mode": "Global",
        "server": "proxy-dns",
        "source_ip_cidr": [
          "172.19.0.0/30"
        ]
      },
      {
        "server": "proxy-dns",
        "source_ip_cidr": [
          "172.19.0.0/30"
        ]
      },
      {
        "clash_mode": "Direct",
        "server": "direct-dns"
      },
      {
        "rule_set": [
          "geosite-ir"
        ],
        "server": "direct-dns"
      }
    ],
    "servers": [
      {
        "address": "tls://208.67.222.123",
        "address_resolver": "local-dns",
        "detour": "proxy",
        "tag": "proxy-dns"
      },
      {
        "address": "local",
        "detour": "direct",
        "tag": "local-dns"
      },
      {
        "address": "rcode://success",
        "tag": "block"
      },
      {
        "address": "local",
        "detour": "direct",
        "tag": "direct-dns"
      }
    ],
    "strategy": "prefer_ipv4"
  },
  "inbounds": [
    {
      "address": [
        "172.19.0.1/30",
        "fdfe:dcba:9876::1/126"
      ],
      "auto_route": true,
      "endpoint_independent_nat": false,
      "mtu": 9000,
      "platform": {
        "http_proxy": {
          "enabled": true,
          "server": "127.0.0.1",
          "server_port": 2080
        }
      },
      "sniff": true,
      "stack": "system",
      "strict_route": false,
      "type": "tun"
    },
    {
      "listen": "127.0.0.1",
      "listen_port": 2080,
      "sniff": true,
      "type": "mixed",
      "users": []
    }
  ],
  "outbounds": [
    {
      "outbounds": [
        "auto",
        "direct",
        "kalirezask"
      ],
      "tag": "proxy",
      "type": "selector"
    },
    {
      "interval": "10m",
      "outbounds": [
        "kalirezask"
      ],
      "tag": "auto",
      "tolerance": 50,
      "type": "urltest",
      "url": "http://www.gstatic.com/generate_204"
    },
    {
      "tag": "direct",
      "type": "direct"
    },
    {
      "tag": "dns-out",
      "type": "dns"
    },
    {
      "tag": "block",
      "type": "block"
    },
    {
      "obfs": {
        "password": "@KevinZakarian",
        "type": "salamander"
      },
      "password": "@KevinZakarian",
      "server": "keviny.iserver.store",
      "server_port": 8443,
      "tag": "kalirezask",
      "tls": {
        "enabled": true,
        "insecure": true
      },
      "type": "hysteria2"
    }
  ],
  "route": {
    "auto_detect_interface": true,
    "final": "proxy",
    "rule_set": [
      {
        "download_detour": "direct",
        "format": "binary",
        "tag": "geosite-ads",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ads-all.srs"
      },
      {
        "download_detour": "direct",
        "format": "binary",
        "tag": "geosite-private",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/private.srs"
      },
      {
        "download_detour": "direct",
        "format": "binary",
        "tag": "geosite-ir",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ir.srs"
      },
      {
        "download_detour": "direct",
        "format": "binary",
        "tag": "geoip-private",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/private.srs"
      },
      {
        "download_detour": "direct",
        "format": "binary",
        "tag": "geoip-ir",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/ir.srs"
      }
    ],
    "rules": [
      {
        "clash_mode": "Direct",
        "outbound": "direct"
      },
      {
        "clash_mode": "Global",
        "outbound": "proxy"
      },
      {
        "outbound": "dns-out",
        "protocol": "dns"
      },
      {
        "outbound": "direct",
        "rule_set": [
          "geoip-private",
          "geosite-private",
          "geosite-ir",
          "geoip-ir"
        ]
      },
      {
        "outbound": "block",
        "rule_set": [
          "geosite-ads"
        ]
      }
    ]
  }
}
