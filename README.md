# 3D-Printer-Buildtime
yolov4-deepsort를 이용하여 3D 프린터의 실제 출력 시간을 측정한다. 3D 프린터의 익스트루더를 객체 추적하여 데이터를 얻는다. 

## Resulting Video
![3Dprinter](https://user-images.githubusercontent.com/98510376/204823446-335aaf00-2dbc-4ecf-8e2b-bd622c4569f5.gif)

## Downloading Pre-trained Weights
darknet을 통해 추적할 객체의 이미지를 학습하여 가중치 파일(.weights)을 얻는다.
다운로드 링크로 연결된 ‘yolov4.weights’ 파일은 Flashforge Creator3의 익스트루더를 학습시킨 미리 훈련된 가중치이다. yolov4.weights 파일을 yolov4_deepsort/data로 이동한다.

만약 다른 프린터의 객체를 추적하고자 한다면 새롭게 학습을 진행해야 한다.

+ [다운로드](https://drive.google.com/file/d/1PkZwGka0YJuydM6Lor-N7R7yNGs6T1v-/view?usp=share_link)

## Getting Started
3D 프린터 출력 시간 측정 시스템을 구현할 수 있는 ‘object_tracker.py’ 파일을  yolov4_deepsort로 이동한다.

이때, 카메라의 위치에 따라 익스트루더의 위치가 변화할 경우 line 101에서 초기 위치를 변경해 주어야 한다.

Anaconda를 통해 CPU or GPU를 이용한다.

+ Conda

```
# Tensorflow CPU
conda env create -f conda-cpu.yml
conda activate yolov4-cpu
```

```
# Tensorflow GPU
conda env create -f conda-gpu.yml
conda activate yolov4-gpu
```

+ Pip

```
# TensorFlow CPU
pip install -r requirements.txt
```

```
# TensorFlow GPU
pip install –r requirements-gpu.txt
```

## Running the Tracker with YOLOv4-deepsort
먼저 weights파일을 tensorflow 모델로 변환한다.
```
# Convert darknet weights to tensorflow model
python save_model.py —model yolov4
```

다음으로 object_tracker.py를 실행하여 yolov4, deepsort 및 tensorflow로 객체 추적을 진행하고, 출력 시간을 측정한다.
```
# webcamvi
python object_tracker.py --video 0 --output ./outputs/webcam.avi —model yolov4
```

## 원본 코드와 차이점
+ line 101~105 – extruder 초기 위치 지정
+ line 107~110 – 시작, 종료시간 변수 정의
+ line 112~113 – 측정 시간 확인을 위한 변수 정의
+ line 185 – class name 지정
+ line 226~227 – extruder 초기 위치 선 그리기
+ line 245~246 – 중심점(좌표) 계산
+ line 249~268 – 출력 시간 측정 및 log 파일 저장
+ line 271~276 – 추적 객체 좌표값 info 표시 및 log 파일 저장
