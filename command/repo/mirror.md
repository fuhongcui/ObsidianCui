### 初始化镜像
`repo init -u "ssh://cuifh@192.168.3.39:39999/manifest/benz" --mirror`

### 定期更新镜像
`repo sync -j14 --prune`
### 使用镜像
`repo init -u "ssh://cuifh@192.168.3.39:39999/manifest/benz" --reference=/home/cuifh/Work/code/benz_base -b chery_x9sp_navi/sop`
*`-u`  需要相同*

`sed -i '/import sys/a \if sys.version_info[0] < 3:\n    reload(sys)\n    sys.setdefaultencoding("utf-8")' .repo/repo/main.py`
`repo sync -j$(nproc) -c --no-tags`
