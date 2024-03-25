### --volumes-fromとは  
- runコマンドにつけれるオプション  
- --volumes-from <コンテナ名> とすると、指定したコンテナのボリュームのマウントと同じようにボリュームをマウントする

<br>

例(バインドマウント)
```bash
# ローカルの/folderとコンテナの/homeをバインドマウントする
docker run --name c1 -dit -v /folder:/home ubuntu

docker inspect c1
# "Mounts": [
#             {
#                 "Type": "bind",
#                 "Source": "/folder",
#                 "Destination": "/home",
#                 "Mode": "",
#                 "RW": true,
#                 "Propagation": "rprivate"
#             }
#         ]

# c1と同じようなボリュームのマウントでコンテナ作成
# この時c1は停止しててもいい

docker run --name c2 -dit --volumes-from c1 ubuntu

docker inspect c2
# "Mounts": [
#             {
#                 "Type": "bind",
#                 "Source": "/folder",
#                 "Destination": "/home",
#                 "Mode": "",
#                 "RW": true,
#                 "Propagation": "rprivate"
#             }
#         ]
```
<br>

例(ボリュームマウント)
```bash
docker volume create v1

docker run --name c1 -dit -v v1:/usr ubuntu

docker inspect c1
# "Mounts": [
#             {
#                 "Type": "volume",
#                 "Name": "v1",
#                 "Source": "/var/lib/docker/volumes/v1/_data",
#                 "Destination": "/usr",
#                 "Driver": "local",
#                 "Mode": "z",
#                 "RW": true,
#                 "Propagation": ""
#             }
#         ]

# この時c1は停止しててもいい
docker run --name c2 -dit--volumes-from c2 ubuntu

docker inspect c2
# "Mounts": [
#             {
#                 "Type": "volume",
#                 "Name": "v1",
#                 "Source": "/var/lib/docker/volumes/v1/_data",
#                 "Destination": "/usr",
#                 "Driver": "local",
#                 "Mode": "z",
#                 "RW": true,
#                 "Propagation": ""
#             }
#         ]
```

<br>

例(両方)
```bash
docker run --name c1 -dit -v /folder:/home ubuntu

docker volume create v1

docker run --name c2 -dit -v v1:/usr ubuntu

# c1とc2の両方のようにボリュームをマウントする
docker run --name c3 -dit --volumes-from c1 --volumes-from c2 ubuntu

docker inspect c3
# "Mounts": [
#             {
#                 ☆c1と同様のマウント 
#                 "Type": "bind",
#                 "Source": "/folder",
#                 "Destination": "/home",
#                 "Mode": "",
#                 "RW": true,
#                 "Propagation": "rprivate"
#             },
#                ☆c2と同様のマウント
#             {
#                 "Type": "volume",
#                 "Name": "v1",
#                 "Source": "/var/lib/docker/volumes/v1/_data",
#                 "Destination": "/usr",
#                 "Driver": "local",
#                 "Mode": "",
#                 "RW": true,
#                 "Propagation": ""
#             }
#         ]
```
