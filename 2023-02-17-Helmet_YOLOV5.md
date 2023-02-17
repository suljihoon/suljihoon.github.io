---
layout: post
title: "YOLO V5를 이용한 개인형 이동장치 이용자 헬멧 착용 유무 확인 프로그램"
---

# 분석 배경
개인형 이동장치와 이륜차의 이용량이 증가하면서 사고 역시 동시에 증가하는 추세이고 헬멧 미착용 시 사망 사고 건수의 비율이 3배 이상으로
안전에 큰 영향이 있는 것을 확인하여 프로그램 개발을 시작함

# 분석 방법
- 이미지 데이터를 수집하였지만 데이터 양과 질이 부족하여 Data Augmentation 진행 - Roboflow로 진행
1. Ratation(-15°~+15°)
2. Hue(-25°~+25°)
3. Saturation(-25°~+25°)
4. Brightness(-25°~+25°)
5. Exposure(-15°~+15°)
6. Noise(픽셀의 최대 3%)

- 객체 별 클래스 구분
·헬멧을 착용하지 않은 개인형 이동장치 이용자(helmet)
·헬멧을 착용한 개인형 이동장치 이용자(non_helmet)

# 분석 결과
성능은 높지 못한 것으로 나옴, 따라서 파라미터 설정과 데이터의 다양성을 추구하여 성능 향상 필요!
때때로 헬멧 착용 이용자와 헬멧 미착용 이용자를 인식하지 못하거나 다르게 인식하는 경우가 존재...


# YOLO V5 실행 CODE
```python
!git clone https://github.com/ultralytics/yolov5
```

    Cloning into 'yolov5'...
    


```python
%cd yolov5 
!pip install -r requirements.txt
```

    C:\Users\wlgns\working directory\python\yolov5
    

    WARNING: Ignore distutils configs in setup.cfg due to encoding errors.
    ERROR: Exception:
    Traceback (most recent call last):
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\cli\base_command.py", line 160, in exc_logging_wrapper
        status = run_func(*args)
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\cli\req_command.py", line 247, in wrapper
        return func(self, options, args)
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\commands\install.py", line 344, in run
        reqs = self.get_requirements(args, options, finder, session)
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\cli\req_command.py", line 433, in get_requirements
        for parsed_req in parse_requirements(
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\req\req_file.py", line 145, in parse_requirements
        for parsed_line in parser.parse(filename, constraint):
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\req\req_file.py", line 327, in parse
        yield from self._parse_and_recurse(filename, constraint)
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\req\req_file.py", line 332, in _parse_and_recurse
        for line in self._parse_file(filename, constraint):
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\req\req_file.py", line 363, in _parse_file
        _, content = get_file_content(filename, self._session)
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\req\req_file.py", line 541, in get_file_content
        content = auto_decode(f.read())
      File "C:\Users\wlgns\Anaconda3\lib\site-packages\pip\_internal\utils\encoding.py", line 34, in auto_decode
        return data.decode(
    UnicodeDecodeError: 'cp949' codec can't decode byte 0xf0 in position 9: illegal multibyte sequence
    


```python
pip install six numpy scipy Pillow matplotlib scikit-image opencv-python imageio Shapely
```


```python
python -m pip install Shapely-1.6.4.post1-cp36-cp36m-win_amd64.whl
```


      Input In [1]
        python -m pip install Shapely-1.6.4.post1-cp36-cp36m-win_amd64.whl
                  ^
    SyntaxError: invalid syntax
    



```python
python3.6 -m pip install jupyter notebook
```


      Cell In [2], line 1
        python3.6 -m pip install jupyter notebook
               ^
    SyntaxError: invalid syntax
    



```python
import numpy as np
from matplotlib.pyplot import imshow, subplots, title
from PIL import Image
from torchvision import transforms
import albumentations
import random
```


    ---------------------------------------------------------------------------

    ModuleNotFoundError                       Traceback (most recent call last)

    <ipython-input-1-c0b4bcd3ac7e> in <module>
          2 from matplotlib.pyplot import imshow, subplots, title
          3 from PIL import Image
    ----> 4 from torchvision import transforms
          5 import albumentations
          6 import random
    

    ModuleNotFoundError: No module named 'torchvision'



```python
pip install torchvision
```

    Collecting torchvision
      Downloading torchvision-0.11.3-cp36-cp36m-win_amd64.whl (985 kB)
    Requirement already satisfied: numpy in c:\users\wlgns\anaconda3\envs\py36\lib\site-packages (from torchvision) (1.17.0)
    Requirement already satisfied: pillow!=8.3.0,>=5.3.0 in c:\users\wlgns\anaconda3\envs\py36\lib\site-packages (from torchvision) (7.2.0)
    Collecting torch==1.10.2
      Downloading torch-1.10.2-cp36-cp36m-win_amd64.whl (226.6 MB)
    Requirement already satisfied: typing-extensions in c:\users\wlgns\anaconda3\envs\py36\lib\site-packages (from torch==1.10.2->torchvision) (3.7.4.2)
    Collecting dataclasses; python_version < "3.7"
      Downloading dataclasses-0.8-py3-none-any.whl (19 kB)
    Installing collected packages: dataclasses, torch, torchvision
    Successfully installed dataclasses-0.8 torch-1.10.2 torchvision-0.11.3
    Note: you may need to restart the kernel to use updated packages.

