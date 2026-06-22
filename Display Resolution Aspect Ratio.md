
# How to Scale an Image keeping the Aspect Ratio
[Source article](https://andrew.hedges.name/experiments/aspect_ratio/)
## Formula for finding the Height
$$ \frac{H_1}{W_1} \times W_2 = H_2 $$
### Problem 1. Finding the Height
Say you have a photo that is 1600 pixels wide x 1200 pixels height, but your blog only has space for a photo 400 pixels wide. How to find the new **height** of your photo preserving the aspect ratio?
#### Variables syntax
$H_1$ = Original Height
$W_1$ = Original Width
$W_2$ = New Width
$H_2$ = unknown
#### Sustitution variables
$H_1$ = 1200
$W_1$ = 1600
$W_2$ = 400
$H_2$ = ?
$$ \dfrac{1200}{1600} \times 400 = H_2 $$
---
$$ 0.75 \times 400 = H_2 $$
---
$$0.75 \times 400 = 300 $$
---
$$ H_2= 300 $$
#### Verification
$$ \frac{W_1}{H_1} = \frac{W_2}{H_2} $$
---
$$ \frac{1600}{1200} = \frac{400}{300} $$
---
$$ 1.\overline{3} = 1.\overline{3} $$
If the result is the same, they are equivalent ✅
If the result is different, they are not equivalent ❌
## Formula for finding the Width
$$ \frac{W_1}{H_1} \times H_2 = W_2 $$
### Problem 2. Finding the Width
Say you have a photo that is 3456 pixels wide x 5184 pixels height, but your blog only has space for a photo 2880 pixels height. How to find the new **width** of your photo preserving the aspect ratio?
#### Variables syntax
$W_1$ = Original Width
$H_1$ = Original Height
$H_2$ = New Height
$W_2$ = unknown
#### Sustitution variables
$W_1$ = 3456
$H_1$ = 5184
$H_2$ = 2880
$W_2$ = unknown
$$ \dfrac{3456}{5184} \times 2880 = W_2 $$
---
$$ 0.\overline{6} \times 2880 = W_2 $$
---
$$ 0.\overline{6} \times 2880 = 1920 $$
---
$$ W_2 = 1920 $$
#### Verification
$$ \frac{W_1}{H_1} = \frac{W_2}{H_2} $$
---
$$ \frac{3456}{5184} = \frac{1920}{2880} $$
---
$$ 0.\overline{6} = 0.\overline{6} $$
If the result is the same, they are equivalent ✅
If the result is different, they are not equivalent ❌
# Table of Aspect Ratios

| $19:16$  | $5:4$  |      $4:3$       | $11:8$  | $143:100$ | $3:2$ | $16:10$ |      $3:5$       |      $6:13$       |      $9:16$      |
| :------: | :----: | :--------------: | :-----: | :-------: | :---: | :-----: | :--------------: | :---------------: | :--------------: |
| $1.1875$ | $1.25$ | $1.\overline{3}$ | $1.375$ |  $1.43$   | $1.5$ |  $1.6$  | $1.\overline{6}$ | $2.1\overline{6}$ | $2.\overline{7}$ |
More about aspect ratios [here](https://en.wikipedia.org/wiki/Aspect_ratio_(image)#/media/File:Aspect_Ratio_Chart.svg)
# We are the firehouse
**Aspect Ratio** cheat sheet with common sizes for photography and videography.

[Aspect Ratio Cheat Sheet](https://www.wearethefirehouse.com/aspect-ratio-cheat-sheet)
<iframe src="https://www.wearethefirehouse.com/aspect-ratio-cheat-sheet" title="Aspect Ratio Cheat Sheet" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>

# Wikipedia
[![Display_Resolutions_Aspect_Ratios|637](https://upload.wikimedia.org/wikipedia/commons/0/0c/Vector_Video_Standards8.svg)](https://upload.wikimedia.org/wikipedia/commons/0/0c/Vector_Video_Standards8.svg)
![Display_Resolutions](https://scontent.fcjs3-2.fna.fbcdn.net/v/t39.30808-6/707740397_122190340952476627_1427341291790884738_n.jpg?_nc_cat=109&ccb=1-7&_nc_sid=127cfc&_nc_ohc=DH2aYs4X03wQ7kNvwHxs7xE&_nc_oc=Ado0LgOSgGLTZSvHMBU85TTwDzFGx13GovTvO0f_sEWAvIbhgl8AUs7fMlf-MPAKVM1YLgEyj617LcIX2ETBAMNU&_nc_zt=23&_nc_ht=scontent.fcjs3-2.fna&_nc_gid=pXfalLd_Q0b8sZCvmCB6hA&_nc_ss=7b2a8&oh=00_Af46UsGFVYvpSubJds1P3DKrEncZgcFgIzAQjVaKDbjDvQ&oe=6A1CC1E8)
# Further Reading
- [Display Aspect Ratio](https://en.wikipedia.org/wiki/Display_aspect_ratio)
- [Display Resolution Standards](https://en.wikipedia.org/wiki/Display_resolution_standards)
- [Aspect Ratio - Image](https://en.wikipedia.org/wiki/Aspect_ratio_(image))
