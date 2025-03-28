2025-03-27 17:02
Tags: #linux 

# Auto extend disk

```bash
sudo crontab -e
```

- Thêm dòng

```bash
@reboot /etc/extend-disk.sh
```

```bash
nano /etc/extend-disk.sh
```

```bash
#!/bin/bash

# Extend the partition
growpart /dev/sda 2

# Resize the filesystem
resize2fs /dev/sda2

# Self-delete the script
rm -- "$0"
```

```bash
sudo chmod +x /etc/extend-disk.sh
```


# References
