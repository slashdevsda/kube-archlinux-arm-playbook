# ARM k8s

setup vanilla kubernetes on rasperry pi

Archlinux ARM


```
useradd -m ansible
echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
mkdir /home/ansible/.ssh
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCycKuiAzvLiVjdKvH6RSTZN6laGioS4iBDv4kqSbFer1SercW4vbwZnMeKPBgRj6dOEZclRJn8xfCjsqDRmdfCA4Ew2/QuaEIevKXNJjHIMHI19vxa2Ev0K82h+C4iW/EjWcBg7tasOAEU7EA5bwQ1JdBuZKVZugkaIKyyjV5E0lPbCAzND7DBAlDvXlQQwjkAZ80a4Dyneq8QqYPInWNGei6zLCCGYKfp5VTvTXPHYkyyvF9jeAIJ54q9kzmmoiLj9XcDGZRnT25fyZCj4o/5/QpjXQFam12aDxdDzUNbMAyne0swrQDamKbbU3+7V87eOP33tV+W/gluIuHgrUpOEMM+S3SCL7AU39hcKFJhNNFQ/WA13NUMyw6c9/tnNaytEqUgZ/8X5zaylDdfKmzilGGBJPYN6WMzv6FwuYV6WC1eE/9HGER0ifU0AwcuxR3+1/tSRv44+YqNLZP/0G0uF/L07DFLL8TmmaypJHMp/cQxsXyeNhMxg7ho5fKJ+PAUT98LeulyZSmfWFaPFWyJDRxhniN6dmhXmE+mn0r8h0C2cW/cZWzhFjsFhooTw02gDUitj6HhUdayzccJH849g7pep3UJtXHqFH/BIUNBi7MMp5hlgL6DZWBLSHQfnfeIHsMxEEhMrEle247m7IFc1d3TGckF8wsNLb+HHFCPiw== thomas' > /home/ansible/.ssh/authorized_keys
chmod g-w /home/ansible/.ssh2
chown -R ansible:ansible /home/ansible/.ssh
```


