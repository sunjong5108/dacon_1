DACON 빛번짐 제거 모델 경진대회
* 데이터는 저작권 문제가 있을 수도 있고, 용량 문제로 올리지 않았습니다.

경진대회가 끝날 시점에 가장 높은 PSNR값을 가졌을 때, 사용한 layer, loss function, optimizer

- 참고 자료 1: [https://github.com/JunHeon-Ch/Image_Denoising_UNet](https://github.com/JunHeon-Ch/Image_Denoising_UNet)
- 참고 자료 2: [https://github.com/sguarnaccio/MSSSIM-ResidualAutoencoder](https://github.com/sguarnaccio/MSSSIM-ResidualAutoencoder)

Dacon 제공 baseline code에서 바꾼 layer 

1. BatchNormalization → InstanceNormalization
2. LeakyReLU → ReLU

Dacon 제공 baseline code에서 바꾼 loss function

1. mae → MS-SSIM + L1 loss

결과

- validation input image 60장을 model에 넣어 예측, 예측 image의 60장의 PSNR값을 평균을 내보았다.
    
    PSNR = 25.84816687488966
    
- 경진대회 당시 제일 잘 나온 PSNR = 25.5994491802
    
    약 0.3점 증가!
    

예측 이미지, PSNR값

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99b5676d-c8d1-4488-8bf5-5b1cb2681090/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99b5676d-c8d1-4488-8bf5-5b1cb2681090/Untitled.png)

- 전체적으로 어두운 환경의 이미지에서 빛 번짐을 더 잘 잡았고, 밝은 환경의 이미지에서는 그렇지 않았다.
생각해보면 전체적으로 이미지가 밝은 경우 모델이 빛이 번진 부분과 주변 배경을 혼동하는 것 같다.
어두운 배경의 경우에는 특징이 잘 드러나 학습을 잘하는 반면, 밝은 배경의 경우 특징이 잘 드러나지 않아 학습을 제대로 못한 것 같다.
- 색감이 변하는 문제는 더 고민을 해봐야 할 것 같다.
- 그렇다면 '데이터에 PCA 기법을 적용해보면 어떨까?' 생각해보았다.
PCA 기법을 통해  데이터에서 학습에 불필요한 특징은 버리고 필요한 특징을 고려하도록 해보면 모델이 더 학습을 잘할 것이라고 생각해보았다.
