## Pixelated (crypto)
1. Description
We are given two images that have to be combined in some way to get the flag.

2. Solution
I loaded up both the images in python and started looking at the RGB values for each pixel. Since the hint for the challenge mentions that the images are to be stacked, I started adding the RGB value of both the images. For solving this challenge, I used the imageio module in python. The script is as follows:
```python
import imageio
img1=imageio.imread("scrambled1.png")
img2=imageio.imread("scrambled2.png")
res=[]
temp=[]
for i in range(256):
    temp=[]
    for j in range(256):
            temp.append(img1[i][j]+img2[i][j])
    res.append(temp)

imageio.imwrite("res2.png",res)
```

Flag: picoCTF{da8fcef8}
