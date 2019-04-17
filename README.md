# Multi-Modal VAE 

## Preliminary Experiments on (Contrived) 3D-Face dataset


### 1) Setup (brief)

- Two-modal paired image data by: <br />
  1) private-1 (zA) = elevation (illumination = neutral value fixed) <br />
  2) private-2 (zB) = illumination (elevation = neutral value fixed)  <br />
  3) shared (zS) = (azimuth, id) <br />
  
- Let: xA = modality 1, xB = modality 2 <br />


### 2) Models

#### 2-a) MMVAE-full/all: induce q(zA, zB, zS | xA, xB) from Product-of-Experts

- partition of latent variables = (zA, zB, zS) <br />
- dim(zA) = 2, dim(zB) = 2, dim(zS) = 3 <br />

- full = full data {(xA,xB)} VAE loss <br />
- all = all data VAE losses, {xA}, {xB}, and {(xA,xB)} <br />

#### 2-b) WG-VAE full/all: no private variables; induce q(z | xI, xT) from Product-of-Experts

- Wu-Goodman's multi-modal VAE
- no partition of latent variables, just shared z <br />
- dim(z) = 7 <br />

- full = full data {(xI,xT)} VAE loss <br />
- all = all data VAE losses, {xA}, {xB}, and {(xA,xB)} <br />

---

### + Latent traversal: (xA,xB) -> z or (zA,zS,zB), from which traverse along each axis -> (xI',xT')

(at iter 300K) <br />

#### Trv-a) MuMo-VAE model

3 instances, each: <br />
True xI | xI w/ zI(1) change |  xI w/ zI(2) | xI w/ zS(1) | xI w/ zS(2) | ... | xI w/ zT(1) | xI w/ zT(2) <br />
True xT | xT w/ zI(1) change |  xT w/ zI(2) | xT w/ zS(1) | xT w/ zS(2) | ... | xT w/ zT(1) | xT w/ zT(2) <br />

![fixed3](https://user-images.githubusercontent.com/44901665/55629573-6b232400-57ab-11e9-8cef-b84f3a651b9a.gif)<br />
![fixed2](https://user-images.githubusercontent.com/44901665/55629559-6494ac80-57ab-11e9-9ab3-4947889314c6.gif)<br />
![fixed1](https://user-images.githubusercontent.com/44901665/55629533-53e43680-57ab-11e9-87fd-82af64fe49a6.gif)<br />

(note: quite accurately identify private and shared factors, but computational issue of having dyadic inf net) <br />

#### Trv-b) Vanilla VAE regarding (xI,xT) as (concatenated) observation

3 instances, each: <br />
True xI | xI w/ z(1) change |  xI w/ z(2) | ... | xI w/ z(10) <br />
True xT | xT w/ z(1) change |  xT w/ z(2) | ... | xT w/ z(10) <br />

![fixed3](https://user-images.githubusercontent.com/44901665/55629683-b3424680-57ab-11e9-9aa2-38293cd12790.gif)<br />
![fixed2](https://user-images.githubusercontent.com/44901665/55629680-afaebf80-57ab-11e9-911d-b6ffda29fae3.gif)<br />
![fixed1](https://user-images.githubusercontent.com/44901665/55629640-97d73b80-57ab-11e9-8f76-36f2cc3561c4.gif)<br />

