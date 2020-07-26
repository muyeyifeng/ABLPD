!!!!!!!!!
初步判断Actions环境中路径名过长导致编译失败，项目代码已转移至https://github.com/muyeyifeng/ABLPD

源项目停止更新https://github.com/muyeyifeng/Actions-Build-Lede-Push-Docker

自建docker 


（ps.建议本地编译后直接通过docker生成image）

（ps.GITHUB ACTION云编译流程已完成测试，过几天更新）


全是经验之谈，算不上专业。





1、编译固件



先编译lede 固件(感谢大佬https://github.com/coolsnowwolf/lede)



无论云编译/本地编译，勾选 Target Image>***Root filesystem archives***>tar.gz



其他插件自己按需选取吧！





2、新建Dockerfiles





###############本地环境###################



前提：安装了docker



mkdir dockerimg #文件夹名随便



cd dockerimg



vim Dockerfile  #必须叫Dockerfile（据说是必须）



键入以下内容>>



    FROM scratch



    ADD openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz /



    USER root



    CMD /sbin/init



将前一步编译的openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz文件拷贝至dockerimg文件夹下方



docker build -f ./Dockerfile -t name:tagname .   (仅本地使用的话，name取一个简单的比如rpi，tagname可以随意如当天日期
20200327。最后的 . 不能少）




2、运行-设置lede容器



最终在本地生成image的同学接下来的步骤可参考https://mlapp.cn/376.html



最终在docker.com上生成image的同学可以pull到本地再参考上面的链接。



其中创建并使用容器的指令，



docker run --restart always --name openwrt -d --network macnet --privileged registry.cn-
shanghai.aliyuncs.com/suling/openwrt:latest /sbin/init



将registry.cn-shanghai.aliyuncs.com/suling/openwrt:latest改为image ID即可



image ID：docker images获取本地镜像



REPOSITORY---------------------------------TAG-----------IMAGE ID-----------------CREATED-----------------SIZE



docker.io/muyeyifeng/rpi2-lede-----------latest-----------0077368fb938-----------36 minutes ago-----------288 MB



这里的ID就是0077368fb938


docker run --restart always --name openwrt -d --network macnet --privileged 0077368fb938 /sbin/init





#######云编译直接生成DOCKER说明###########


本地（或其他途径）经make menuconfig 及 ./scripts/diffconfig.sh 后获得 defconfig 配置文件，替换项目中的defconfig


如使用本项目的Actions源码，需添加Setting->Secrets->ALIDOCKER_KEY  (从阿里云镜像服务中获得)


修改env中的DOCKER_REGISTRY参数，提交Action即可
