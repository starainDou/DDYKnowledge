```
ffmpeg -y -i 「视频全名」 -vf subtitles=「字幕文件名」 「导出的视频文件名」
// wav 转 mp3
ffmpeg -i /Users/mac/Pictures/Work/yidaozhang/222.wav -b:a 64k -acodec mp3 -ar 44100 -ac 1 2.mp3 
// 音频拼接 
ffmpeg -i "concat:1.mp3|2.mp3" -acodec copy 3.mp3 
// 一张图片 + 音频  -t 30 长度秒
ffmpeg -r 15 -f image2 -loop 1 -i 3.png -i 3.mp3 -c:v libx265 -movflags faststart  -tag:v hvc1 -t 30 -y 666.mp4

ffmpeg -i ${oldFille} -acodec copy -map_metadata -1 -metadata title="com.nwyunlingvideo.mryjios" -y ${newFile}

python3 /Users/mac/Desktop/cocosProject/AiShangXiaoFangKuai/encrypt.py /Users/mac/Downloads/lottie/pic/ com.nwyunlingvideo.mryjios

ffmpeg -i ./MQNewMessageRingStyle23.wav -acodec copy -map_metadata -1 -metadata title="com.fkbgyes.luckysongios" -y ./MQNewMessageRingStyle2.wav

ffmpeg -i ./MQNewMessageRing3.mp3 -acodec copy -map_metadata -1 -metadata title="com.fkbgyes.luckysongios" -y ./MQNewMessageRing.mp3

ffmpeg -i ./MQSendMessageRing3.mp3 -acodec copy -map_metadata -1 -metadata title="com.fkbgyes.luckysongios" -y ./MQSendMessageRing.mp3

ffmpeg -i 240103001.mp4 -c:a copy -c:v libx265 -preset slow -vf "deblock,hqdn3d" -movflags faststart -max_muxing_queue_size 2048 -threads 8 -tag:v hvc1 -y 240103009.mp4

ffmpeg -i 0.mp4 -c:a copy -c:v libx265 -preset slow -vf "deblock,hqdn3d" -movflags faststart -max_muxing_queue_size 2048 -threads 8 -tag:v hvc1 -y 4.mp4


ffmpeg -i 240103002.mp4 -c:a copy -c:v libx265 -preset slower -vf "deblock,hqdn3d" -crf 20 -movflags faststart -max_muxing_queue_size 2048 -threads 16 -tag:v hvc1 -y 240103007.mp4
```

* [Mac平台下的FFmpeg的安装编译](https://juejin.cn/post/7005112278706028551)
* [在Linux/Mac/Windows上配置FFmpeg开源音频工具，轻松完成视频转码、音频混合等操作](https://www.mintimate.cn/2020/02/25/FFmpeg/)
* [ffmpeg_cheatsheet](https://gitlab.fhnw.ch/hgk-ml/hgk-ml-tools/-/tree/master/ffmpeg_cheatsheet)
* [视频文件太大？使用FFmpeg来无损压缩它](https://zhuanlan.zhihu.com/p/564071964)
* [FFmpeg命令行：如何压制与QuickTime兼容的HEVC视频](https://blog.csdn.net/iBobbyTS/article/details/109402983)