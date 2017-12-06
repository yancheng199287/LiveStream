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


> 以上的HTPP-FLV和RTMP流只能在FLASH下播放，然而很多浏览器已经不支持，尤其MAC下几乎都默认屏蔽，所以只能手动开启或者下载专有的客户端，如VLC播放。
那么如果想要在各大平台浏览器上播放，得使用支持H5的video标签播放，我们一般采用m3u8格式的流。那么如果使用srs启动使用m3u8呢？



git clone https://github.com/ossrs/srs
cd srs/trunk

./configure --with-nginx  //注意这一步可能会存在依赖不全的问题，需要下载对应依赖项安装再编译

make  //安装


sudo ./objs/nginx/sbin/nginx   //启动分发hls（m3u8/ts）的nginx
注意假如你要改nginx配置文件，则需要更改此文件  /home/srs.oschina/trunk/objs/nginx/conf/nginx.conf   比如你可以改端口，虚拟主机等。

./objs/srs -c conf/hls.conf       // 启动SRS

RTMP流地址为：rtmp://192.168.1.170/live/livestream
HLS流地址为： http://192.168.1.170/live/livestream.m3u8






