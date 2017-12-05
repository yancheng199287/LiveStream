# LiveStream
如何搭建一个直播视频平台


### 开源项目：采用srs（Simple-RTMP-Server）开源项目  https://github.com/ossrs/srs

### 官网地址：http://www.ossrs.net/srs.release/releases/

### 系统选择：推荐使用Ubuntu14 或Centos6


### 安装步骤：

假设服务器的IP是：192.168.0.200

 

- 第一步，获取SRS。详细参考GIT获取代码

git clone https://github.com/ossrs/srs
cd srs/trunk
或者使用git更新已有代码：
git pull

- 第二步，编译SRS。详细参考Build
./configure && make

- 第三步，编写SRS配置文件。详细参考HTTP FLV

将以下内容保存为文件，譬如conf/http.flv.live.conf，服务器启动时指定该配置文件(srs的conf文件夹有该文件)。

 conf/http.flv.live.conf
 

    listen              1935;
    max_connections     1000;
    http_server {
        enabled         on;
        listen          8080;
        dir             ./objs/nginx/html;
    }
    vhost __defaultVhost__ {
        http_remux {
            enabled     on;
            mount       [vhost]/[app]/[stream].flv;
            hstrs       on;
        }
    }


- 第四步，启动SRS。

第一种只支持RTMP协议  启动 rtmp推流
  ./objs/srs -c conf/rtmp.conf

  推流地址 rtmp://192.168.0.200/live/livestream
  观看RTMP流
  RTMP流地址为：rtmp://192.168.0.200/live/livestream


 第二种支持RTMP和HTTP-FLV
 ./objs/srs -c conf/http.flv.live.conf

  推流地址：     rtmp://192.168.0.200/live/livestream
  观看HTTP FLV流地址为： http://192.168.0.200:8080/live/livestream.flv
  
  推流项目软件地址：https://obsproject.com/download 
  
### 最后 使用本项目中的 play.html 页面打开，输入你的播放地址就可以看了

