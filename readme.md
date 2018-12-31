```bash
# 安装 Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 验证是否安装成功
brew -v

# 安装 ffmpeg ，用于转码
brew install ffmpeg
# 验证是否安装成功
ffmpeg
# 安装 gpac ，用到里面包含的 mp4box
brew install gpac
# 验证是否安装成功
mp4box

# 用 ffmpeg 转码得到不同质量的视频
ffmpeg -i input.mp4 -s 160x90 -c:v libx264 -b:v 250k -g 90 -an input_video_160x90_250k.mp4
ffmpeg -i input.mp4 -s 320x180 -c:v libx264 -b:v 500k -g 90 -an input_video_320x180_500k.mp4
ffmpeg -i input.mp4 -s 640x360 -c:v libx264 -b:v 750k -g 90 -an input_video_640x360_750k.mp4
ffmpeg -i input.mp4 -s 640x360 -c:v libx264 -b:v 1000k -g 90 -an input_video_640x360_1000k.mp4
ffmpeg -i input.mp4 -s 1280x720 -c:v libx264 -b:v 1500k -g 90 -an input_video_1280x720_1500k.mp4

# 用 ffmpeg 得到音频
ffmpeg -i input.mp4 -c:a aac -b:a 128k -vn input_audio_128k.mp4

# 用 mp4box 得到 MPEG-Dash 需要的音视频和 .mpd 文件
mp4box -dash 5000 -rap -profile dashavc264:onDemand -mpd-title BBB -out manifest.mpd -frag 2000 input_audio_128k.mp4 input_video_160x90_250k.mp4 input_video_320x180_500k.mp4 input_video_640x360_750k.mp4 input_video_640x360_1000k.mp4 input_video_1280x720_1500k.mp4

# 用 http-server 作为音视频和 .mpd 文件的服务器
brew install node
# 验证是否安装成功
node -v
npm install http-server -g
# --cors 代表接受任意域名的跨域资源共享，-c-1 代表不启用缓存
http-server -a 127.0.0.1 -p 9999 --cors -c-1

# 找另一个目录下载构建 dash.js，一个开源的支持 MPEG-Dash 协议的播放器
npm install -g grunt-cli
git clone https://github.com/Dash-Industry-Forum/dash.js.git
cd dash.js
npm install
grunt dev

# 选择不同的 bitrate list，看看显示效果
# 回到选择 auto switch，用 Charles 限速 512 kbps
```