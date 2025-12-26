可以用 `ls -ld` 命令查看目录的详细信息，包括属主和权限。例如：
`ls -ld /data/linmiaoju/.cache ls -ld /data/linmiaoju/.cache/pip`

修改目录属主为 linmiaoju
sudo chown -R linmiaoju:linmiaoju /data/linmiaoju/.cache/pip


