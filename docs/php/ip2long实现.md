# ip2long 实现

![ip2long](./assets/php-ip2long.png)

```
124.205.30.150=2093817494

list($p1,$p2,$p3,$p4) = explode('.','124.205.30.150');

$realNum = ($p1<<24)+($p2<<16)+($p3<<8)+$p4;
```
