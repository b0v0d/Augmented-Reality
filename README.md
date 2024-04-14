# Augmented-Reality
Draw circle on chessboard

### 1.기능
녹화된 chessboard 영상에 카메라 각도가 달라져도 고정된 위치에 원을 띄워놓는다.

### 2.camera callibration으로 얻은 수치
``` python
K = np.array([[432.7390364738057, 0, 476.0614994349778],
              [0, 431.2395555913084, 288.7602152621297],
              [0, 0, 1]])
dist_coeff = np.array([-0.2852754904152874, 0.1016466459919075, -0.0004420196146339175, 0.0001149909868437517, -0.01803978785585194])
```
### 3.결과
<p align="center" width="100%">
  <img src="https://github.com/b0v0d/Augmented-Reality/assets/162780235/fc783241-cfd6-432e-8654-965b2baeef99" width="49%">
  <img src="https://github.com/b0v0d/Augmented-Reality/assets/162780235/7a53bc14-7b9c-461c-921c-5f9ac2745a7a" width="49%">
</p>
