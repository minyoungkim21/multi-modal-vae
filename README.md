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

### + Latent traversal: (xA,xB) -> z or (zA,zS,zB), from which traverse along each axis -> (xA',BT')

(at iter 300K) <br />

#### Trv-a) MMVAE-full

3 instances, each: <br />
True xA | xA w/ zA(1) change |  xA w/ zA(2) | xA w/ zS(1) | xA w/ zS(2) | ... | xA w/ zB(1) | xA w/ zB(2) <br />
True xB | xB w/ zB(1) change |  xB w/ zB(2) | xB w/ zS(1) | xB w/ zS(2) | ... | xB w/ zB(1) | xB w/ zB(2) <br />

![fixed3](https://user-images.githubusercontent.com/44901665/55707692-1a464200-59dc-11e9-9dd7-74e4b2689830.gif)
![fixed2](https://user-images.githubusercontent.com/44901665/55707696-1c100580-59dc-11e9-9f5c-acc72ba4bc16.gif)
![fixed1](https://user-images.githubusercontent.com/44901665/55707689-187c7e80-59dc-11e9-8d32-fd81339967f0.gif)

(note: problematic! eg, zS(1) learns elevation factor, but it should be a private factor in zI)<br />

#### Trv-b) MM-VAE-all

3 instances, each: <br />
True xA | xA w/ zA(1) change |  xA w/ zA(2) | xA w/ zS(1) | xA w/ zS(2) | ... | xA w/ zB(1) | xA w/ zB(2) <br />
True xB | xB w/ zB(1) change |  xB w/ zB(2) | xB w/ zS(1) | xB w/ zS(2) | ... | xB w/ zB(1) | xB w/ zB(2) <br />

![fixed3](https://user-images.githubusercontent.com/44901665/55708062-eddef580-59dc-11e9-81bb-5d276bf26f6f.gif)
![fixed2](https://user-images.githubusercontent.com/44901665/55708073-f33c4000-59dc-11e9-932b-37b55004d768.gif)
![fixed1](https://user-images.githubusercontent.com/44901665/55708077-f5060380-59dc-11e9-8f58-bcfe19f5ddbf.gif)

(note: better identify/discern the private and shared factors, which implies that the loss terms for marginal data, ie, {xI} and {xT}, are necessary?)<br />

#### Trv-c) WGVAE-full

3 instances, each: <br />
True xA | xA w/ z(1) change |  xA w/ z(2) | ... | xA w/ z(7) <br />
True xB | xB w/ z(1) change |  xB w/ z(2) | ... | xB w/ z(7) <br />


![fixed3](https://user-images.githubusercontent.com/44901665/55861783-054de800-5b6f-11e9-81f9-4dde8d71347f.gif)
![fixed2](https://user-images.githubusercontent.com/44901665/55861768-febf7080-5b6e-11e9-9560-1107a7c60450.gif)
![fixed1](https://user-images.githubusercontent.com/44901665/55861753-f830f900-5b6e-11e9-8912-2fba57c4aa5f.gif)

#### Trv-d) WGVAE-all

3 instances, each: <br />
True xA | xA w/ z(1) change |  xA w/ z(2) | ... | xA w/ z(7) <br />
True xB | xB w/ z(1) change |  xB w/ z(2) | ... | xB w/ z(7) <br />

![fixed3](https://user-images.githubusercontent.com/44901665/55861845-28789780-5b6f-11e9-9154-e5490b6555c6.gif)
![fixed2](https://user-images.githubusercontent.com/44901665/55861850-2b738800-5b6f-11e9-8f85-21d4b8e21e47.gif)
![fixed1](https://user-images.githubusercontent.com/44901665/55861825-21ea2000-5b6f-11e9-90b2-ac322220357c.gif)

