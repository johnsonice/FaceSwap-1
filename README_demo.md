## face swap app repo
### environment
1.conda env_python = 3.6+
2.tensorflow-gpu
```
conda install -c anaconda tensorflow-gpu 
```
3.kerasffmpeg -i vid.mp4 -f mp3 outaudio.mp3. 

```
conda install -c conda-forge keras
```
### operations step by step

1. extract pictures
```
# To convert trump:
python faceswap.py extract -i ~/faceswap/photo/trump -o ~/faceswap/data/trump
# To convert cage:
python faceswap.py extract -i ~/faceswap/photo/cage -o ~/faceswap/data/cage
```
At the same time 
extract keypoint frame
```
ffmpeg -i output.mp4 -strict -2  -qscale 0 -intra keyoutput.mp4
```
extract constant duration of the video
```
//截取从30s开始的30s
    ffmpeg -ss 00:00:30 -t 00:00:30 -i keyoutput.mp4 -vcodec copy -acodec copy
```
ioslate the audio from the extracted video
```
ffmpeg -i vid.mp4 -f mp3 outaudio.mp3. 
```

generate pictures from video
```
ffmpeg -i vedio1.mp4 frame%3d.png. 
```

2. Train data and generate model parameters
```
python faceswap.py train -A ~/faceswap/data/trump -B ~/faceswap/data/cage -m ~/faceswap/models/
# or -p to show a preview
python faceswap.py train -A ~/faceswap/data/trump -B ~/faceswap/data/cage -m ~/faceswap/models/ -p 
```
3. swap face with the pictures generated by video using trained model
```
python faceswap.py convert -i ~/faceswap/photo/trump/ -o ~/faceswap/output/ -m ~/faceswap/models/
```
4. generate video with audio and increase the speed of the generated video
```
ffmpeg -i trump/f%3d.png -i aud.mp3 -c:v libx264 -vf “fps=30,format=yuv420p,setpts=0.833333*PTS” out.mp4
```
