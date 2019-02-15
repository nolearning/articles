# SSH Tips
## SOCKS Proxy
```shell
ssh -D proxy_port proxy_server
# multiple ports
ssh -D proxy_port1 -D proxy_port2 -f -C -q -N user@proxy_server
# socks proxy via an intermediate host
ssh -D proxy_port -J user1@middle.server user2@proxy_server
# older clients
ssh -L local_port:localhost:middle_port user1@middle.server -t ssh -D middle_port user2@proxy_server
```

## Jump Hosts
```
ssh -J middle.server:middle_port proxy.server
# older version
ssh -o ProxyCommand="ssh -W %h:%p middle.server" proxy.server
```

* [OpenSSH/Cookbook/Proxies and Jump Hosts](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Proxies_and_Jump_Hosts)
* [SSH through multiple hosts using ProxyCommand?](https://serverfault.com/questions/368266/ssh-through-multiple-hosts-using-proxycommand)
* [An SSH tunnel via multiple hops](https://superuser.com/questions/96489/an-ssh-tunnel-via-multiple-hops)
* [SSH Examples, Tips & Tunnels](https://hackertarget.com/ssh-examples-tunnels/)
* [内网穿透](https://zhuanlan.zhihu.com/p/56443700)
