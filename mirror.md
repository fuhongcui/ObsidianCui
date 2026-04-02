初始化镜像
`repo init -u "ssh://cuifh@192.168.3.39:39999/manifest/benz" --mirror`
使用镜像
`repo init -u "ssh://cuifh@192.168.3.39:39999/manifest/benz" --reference=/home/cuifh/Work/code/benz_base -b chery_x9sp_navi/sop`
> `-u` (Manifest URL) 需要相同