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

### 3.코드 설명
##### Open a video
``` python
video = cv.VideoCapture(video_file)
assert video.isOpened(), 'Cannot read the given input, ' + video_file
```

##### Prepare a 3D box for simple AR
``` python
box_lower = board_cellsize * np.array([[4, 2,  0], [5, 2,  0], [5, 4,  0], [4, 4,  0]])
box_upper = board_cellsize * np.array([[4, 2, -1], [5, 2, -1], [5, 4, -1], [4, 4, -1]])
```

##### Draw the fixed sphere on the image
``` python
        fixed_sphere_center = np.array([0.5, 0.5, 0.5]) * board_cellsize  # Center of the fixed sphere
        fixed_sphere_radius = 0.5 * board_cellsize  # Radius of the fixed sphere
        num_pts = 50  # Number of points to approximate the sphere
        fixed_sphere_points_3d = []
        for theta in np.linspace(0, np.pi, num_pts):
            for phi in np.linspace(0, 2 * np.pi, num_pts):
                x = fixed_sphere_center[0] + fixed_sphere_radius * np.sin(theta) * np.cos(phi)
                y = fixed_sphere_center[1] + fixed_sphere_radius * np.sin(theta) * np.sin(phi)
                z = fixed_sphere_center[2] + fixed_sphere_radius * np.cos(theta)
                fixed_sphere_points_3d.append([x, y, z])
        fixed_sphere_points_3d = np.array(fixed_sphere_points_3d)
        fixed_sphere_points_3d = cv.Rodrigues(np.array([0, np.pi / 2, 0]))[0] @ fixed_sphere_points_3d.T
        fixed_sphere_points_2d, _ = cv.projectPoints(fixed_sphere_points_3d.T, rvec, tvec, K, dist_coeff)
        fixed_sphere_points_2d = np.int32(fixed_sphere_points_2d).reshape(-1, 2)
        
        for i in range(num_pts - 1):
            for j in range(num_pts - 1):
                pt1 = tuple(fixed_sphere_points_2d[i * num_pts + j])
                pt2 = tuple(fixed_sphere_points_2d[i * num_pts + j + 1])
                pt3 = tuple(fixed_sphere_points_2d[(i + 1) * num_pts + j])
                pt4 = tuple(fixed_sphere_points_2d[(i + 1) * num_pts + j + 1])
                cv.line(img, pt1, pt2, (0, 255, 255), 2)
                cv.line(img, pt2, pt4, (0, 255, 255), 2)
                cv.line(img, pt4, pt3, (0, 255, 255), 2)
                cv.line(img, pt3, pt1, (0, 255, 255), 2)
```

### 4.결과
<p align="center" width="100%">
  <img src="https://github.com/b0v0d/Augmented-Reality/assets/162780235/fc783241-cfd6-432e-8654-965b2baeef99" width="49%">
  <img src="https://github.com/b0v0d/Augmented-Reality/assets/162780235/7a53bc14-7b9c-461c-921c-5f9ac2745a7a" width="49%">
</p>
