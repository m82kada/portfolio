---
title: "Robustifying Routers Against Input Perturbations for Sparse Mixture-of-Experts Vision Transformers"
summary: "OJSP, ICASSP 2025"
tags: [""]
categories: ["themes", "syntax"]
series: ["Themes Guide"]
draft: false
params:
  ShowBreadCrumbs: true
---

**<span style="font-size: 20px;">OJSP, ICASSP 2025</span>**

**Masahiro Kada<sup>1</sup>&nbsp;&nbsp;Ryota Yoshihashi<sup>1</sup>&nbsp;&nbsp;Satoshi Ikehata<sup>1, 2</sup>&nbsp;&nbsp;Rei Kawakami<sup>1</sup>&nbsp;&nbsp;Ikuro Sato<sup>1, 3</sup>**

<span style="font-size: 15px"><sup>1</sup>Institute of Science Tokyo&nbsp;&nbsp;<sup>2</sup>National Institute of Informatics&nbsp;&nbsp;<sup>3</sup>Denso IT Laboratory</span>

Link:&nbsp;&nbsp;[Paper](https://ieeexplore.ieee.org/document/10858379)&nbsp;&nbsp;|&nbsp;&nbsp;[Code](https://github.com/m82kada/PRC/blob/main/README.md?plain=1)



![PRC](../images/prc/demo.png)


## Abstract
Mixture of experts with a sparse expert selection rule has been gaining much attention recently because of its scalability without compromising inference time. However, unlike standard neural networks, sparse mixture-of-experts models 
inherently exhibit discontinuities in the output space, which may impede the acquisition of appropriate invariance to the input perturbations, leading to a deterioration of model performance for tasks such as classification. To address this issue, we propose Pairwise Router Consistency (PRC) that effectively penalizes the discontinuities occurring under natural deformations of input images. With the supervised loss, the use of PRC loss empirically improves classification accuracy on ImageNet-1K, CIFAR-10, and CIFAR-100 datasets, compared to a baseline method. Notably, our method with 1-expert selection slightly outperforms the baseline method using 2-expert selection. We also confirmed that models trained with our method experience discontinuous changes less frequently under input perturbations.
<br>

## Method
We trained V-MoE [C. Riquelme+, NeurIPS 2021] with Pairwise Router Consistency (PRC) as a regularization term to ensure that the same input image patches are routed consistently.

![PRC](../images/prc/prc.png)
<br>

## Evaluation
We conducted quantitative evaluations on the ImageNet-1K, CIFAR-10, CIFAR-100, and Oxford Flowers-102 datasets.
<br>

|   | Top-k | ImageNet-1K | CIFAR-10 | CIFAR-100 | Flowers |
| --- | --- | --- | --- | --- | --- |
| V-MoE-S | 2 | 75.84% | 95.20% | 81.38% | 89.30% |
| **V-MoE-S w/ PRC** | 2 | 76.27% | 95.36% | 82.27% | 90.18% |
| V-MoE-S | 1 | 75.23% | 94.81% | 81.18% | 90.21% |
| **V-MoE-S w/ PRC** | 1 | 75.92% | 95.12% | 82.12% | 91.24% |

<br>
We visualized the changes in routing as Gaussian noise was gradually added.


<div class="images-folder">
    <div>
        <img id="img1" src="../images/prc/4-0-0.png" class="images">
        <p id="mainImgLabel" class="image-label">Original Image</p>
    </div>
    <div>
        <img id="img2" src="../images/prc/4-1-0.png" class="images">
        <p class="image-label">V-MoE</p>
    </div>
    <div>
        <img id="img3" src="../images/prc/4-2-0.png" class="images">
        <p class="prc-label image-label">V-MoE w/ PRC</p>
    </div>
</div>
<div class="slider-container">
    <label class="noise-label">Noise Level</label>
    <input type="range" id="slider" min="0" max="9" step="1" value="0" oninput="selectNoise(this.value)">
</div>

<div class="button-group">
    <button class="img-btn active" onclick="selectImage(0)">Image 1</button>
    <button class="img-btn" onclick="selectImage(1)">Image 2</button>
    <button class="img-btn" onclick="selectImage(2)">Image 3</button>
    <button class="img-btn" onclick="selectImage(3)">Image 4</button>
    <button class="img-btn" onclick="selectImage(4)">Image 5</button>
</div>

<script>
    var image_number = 4;
    var noise_level = 0;
    function selectImage(value) {
        image_number = 4 - value;

        document.querySelectorAll('.img-btn').forEach((btn, i) => {
            btn.classList.toggle('active', i === value);
        });
        updateImages();
    }
    function selectNoise(value) {
        noise_level = value;
        updateImages();
    }
    function updateImages() {
        if (noise_level == 0) {
            document.getElementById('mainImgLabel').innerHTML = "Original Image";
        } else {
            document.getElementById('mainImgLabel').innerHTML = "Noised Image";

        }
        document.getElementById('img1').src = '../images/prc/' + image_number + '-0-' + noise_level + '.png';
        document.getElementById('img2').src = '../images/prc/' + image_number + '-1-' + noise_level + '.png';
        document.getElementById('img3').src = '../images/prc/' + image_number + '-2-' + noise_level + '.png';
    }
</script>

<style>
.button-group {
    display: flex;
    justify-content: center;
    align-items: center;
}

.button-group label {
    font-size: 16px;
    font-weight: bold;
    margin-right: 10px;
}

.button-group button {
    padding: 5px 7px;
    border: 1px solid #888;
    background-color: #f0f0f0;
    cursor: pointer;
    font-size: 14px;
    flex: 1;
    border-radius: 0;
    transition: background-color 0.3s, color 0.3s;
}

.button-group button:first-child {
    border-radius: 5px 0 0 5px;
}

.button-group button:last-child {
    border-radius: 0 5px 5px 0;
}

.button-group button.active {
    background-color: #555;
    color: white;
    border-color: #333;
}
.images-folder {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 20px;
}
.images {
    border-radius: 0px !important;
}
.slider-container {
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
}

input[type="range"] {
    width: 50%;
    -webkit-appearance: none;
    height: 8px;
    background: linear-gradient(to right, #d3d3d3, #888);
    border-radius: 5px;
    outline: none;
    opacity: 0.9;
    transition: opacity 0.2s;
}
input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 16px;
    height: 16px;
    background: #555; /* つまみの色 */
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
}

input[type="range"]::-moz-range-thumb {
    width: 16px;
    height: 16px;
    background: #555; /* つまみの色 */
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
}


input[type="range"]:hover {
    opacity: 1;
}
.noise-label {
    font-weight: bold;
    font-size: 16px;
}
.prc-label{
    font-weight: bold;
}
</style>

<br>

## BibTeX
```
@ARTICLE{10858379,
  author={Kada, Masahiro and Yoshihashi, Ryota and Ikehata, Satoshi and Kawakami, Rei and Sato, Ikuro},
  journal={IEEE Open Journal of Signal Processing}, 
  title={Robustifying Routers Against Input Perturbations for Sparse Mixture-of-Experts Vision Transformers}, 
  year={2025},
  volume={},
  number={},
  pages={1-9},
  keywords={Perturbation methods;Routing;Transformers;Predictive models;Contrastive learning;Data models;Computational modeling;Training;Image classification;Computer vision;Mixture of Experts;Dynamic Neural Network;Image Classification;Vision Transformer},
  doi={10.1109/OJSP.2025.3536853}}
```

## Acknowledgements
This work was supported by JSPS KAKENHI Grant Number JP22H03642 and DENSO IT LAB Recognition and Learning Algorithm Collaborative Research Chair (Science Tokyo).
