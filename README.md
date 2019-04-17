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

#### 2-b) WG-VAE full/all: no private variables; induce q(z | xA, xB) from Product-of-Experts

- Wu-Goodman's multi-modal VAE
- no partition of latent variables, just shared z <br />
- dim(z) = 7 <br />

- full = full data {(xA,xB)} VAE loss <br />
- all = all data VAE losses, {xA}, {xB}, and {(xA,xB)} <br />

---

### + Latent traversal: (xA,xB) -> z or (zA,zS,zB), from which traverse along each axis -> (xA',xB')

(at iter 300K) <br />

#### Trv-a) MMVAE-full

3 instances, each: <br />
True xA | xA w/ zA(1) change |  xA w/ zA(2) | xA w/ zS(1) | xA w/ zS(2) | ... | xA w/ zB(1) | xA w/ zB(2) <br />
True xB | xB w/ zB(1) change |  xB w/ zB(2) | xB w/ zS(1) | xB w/ zS(2) | ... | xB w/ zB(1) | xB w/ zB(2) <br />

![fixed0](https://user-images.githubusercontent.com/44901665/56276690-e6b19900-60fa-11e9-97c1-6aa44f5cb40f.gif) <br />
![fixed2](https://user-images.githubusercontent.com/44901665/56276691-e74a2f80-60fa-11e9-8fb4-670b355d874f.gif) <br />
![fixed1](https://user-images.githubusercontent.com/44901665/56276692-e74a2f80-60fa-11e9-8116-755016c05e18.gif) <br />

(note: zA(1) = elev, zA(2) = id?, zS(1) = id/azim, zS(2) = azim, zB(1) = illum) <br />

#### Trv-b) MMVAE-all

3 instances, each: <br />
True xA | xA w/ zA(1) change |  xA w/ zA(2) | xA w/ zS(1) | xA w/ zS(2) | ... | xA w/ zB(1) | xA w/ zB(2) <br />
True xB | xB w/ zB(1) change |  xB w/ zB(2) | xB w/ zS(1) | xB w/ zS(2) | ... | xB w/ zB(1) | xB w/ zB(2) <br />

![fixed0](https://user-images.githubusercontent.com/44901665/56276779-0d6fcf80-60fb-11e9-9a5b-7acfedbc9054.gif) <br />
![fixed2](https://user-images.githubusercontent.com/44901665/56276780-0d6fcf80-60fb-11e9-9333-efa028e6e3d2.gif) <br />
![fixed1](https://user-images.githubusercontent.com/44901665/56276783-0e086600-60fb-11e9-9446-5a52117bc91b.gif) <br />

(note: zA(2) = elev, zA(2) = id?, zS(1,3) = azim, zS(2) = id, zB(1) = illum) <br />

#### Trv-c) WGVAE-full

3 instances, each: <br />
True xA | xA w/ z(1) change |  xA w/ z(2) | ... | xA w/ z(7) <br />
True xB | xB w/ z(1) change |  xB w/ z(2) | ... | xB w/ z(7) <br />

![fixed0](https://user-images.githubusercontent.com/44901665/56276923-57f14c00-60fb-11e9-9120-fcd54e96f6c9.gif) <br />
![fixed2](https://user-images.githubusercontent.com/44901665/56276924-5889e280-60fb-11e9-9b36-4df3dcf504ad.gif) <br />
![fixed1](https://user-images.githubusercontent.com/44901665/56276926-5889e280-60fb-11e9-899f-8078c8283bbf.gif) <br />

(note: z(1,6) = illum?, z(2,3) = id?, z(4,5) = azim, z(7) = elev) <br />

#### Trv-d) WGVAE-all

3 instances, each: <br />
True xA | xA w/ z(1) change |  xA w/ z(2) | ... | xA w/ z(7) <br />
True xB | xB w/ z(1) change |  xB w/ z(2) | ... | xB w/ z(7) <br />

![fixed0](https://user-images.githubusercontent.com/44901665/56276992-6fc8d000-60fb-11e9-8afc-d39acd15dab2.gif) <br />
![fixed2](https://user-images.githubusercontent.com/44901665/56276994-70616680-60fb-11e9-92bc-84f1d30169a0.gif) <br />
![fixed1](https://user-images.githubusercontent.com/44901665/56276995-70616680-60fb-11e9-92e9-8a01951e34f3.gif) <br />

(note: z(1,2) = id?, z(3) = illum, z(5) = azim, z(6) = id/azim?, z(7) = elev) <br />
