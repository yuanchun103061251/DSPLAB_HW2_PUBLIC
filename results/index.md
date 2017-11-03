# Your Name <span style="color:red">(yout cs id)</span>

# Project 1 / Image Filtering and Hybrid Images

## Overview
The project is related to 
> corner detection by gradience calculation


## Implementation
1. transform the image from RGB to grayscale
	```
	I0(1:xmax,1:ymax) = 0.299*I(:,:,1)+ 0.587*I(:,:,2) + 0.114*I(:,:,3);
	```
2. convolve horizontal/vertical gradient filter to I, becoming Ix/Iy
	```
	Ix = conv2(I0,dx,'same');
	Iy = conv2(I0,dy,'same');
	```
3. to create matrix A, it requires Ix2, Iy2, Ixy.
	```
	Ix2 =conv2(Ix.*Ix,g ,'same');
	Iy2 =conv2(Iy.*Iy,g ,'same');
	Ixy = conv2(Ix.*Iy,g ,'same');
	```
4. use eigenvalue of A to determine how "corner" a pixel is.
	```
	for x=xmin:xmax
		for y=ymin:ymax
			M = [Ix2(x,y),Ixy(x,y);Ixy(x,y),Iy2(x,y)];
			R(x,y) = det(M)-alpha*trace(M)^2;      
		end
	end
	```
5. make max R value to be 1000
6. using B = ordfilt2(A,order,domain) to complment a maxfilter
	```
	sze = 2*r+1;
	domain = ones(sze,sze);
	order = sze*sze;
	B = ordfilt2(R,order,domain);
	```
7. if B(pixel1)=R(pixel1) and B(pixel1) > threshold, pixel one is a corner and set its value to 1 in RBbinary, 0 otherwise
	```
	RBinary = (B==R)&(B>Thrshold);
	```
8. circle all the corners on the image

### Results
* result1  <br />
from left to right: house_origin, house_corner
<table border=1>
<tr>
<td>
<img src="/data/Im.jpg" width="48%"/>
<img src="/house_corner.jpg" width="48%"/>
</td>
</tr>
</table>

* result2  <br />
from left to right: use_flag, usa_flag_corner
<table border=1>
<tr>
<td>
<img src="/usa_flag.png" width="48%"/>
<img src="/usa_flag_corner.jpg" width="48%"/>
</td>
</tr>
</table>

* result3  <br />
from left to right: piano, piano_corner
<table border=1>
<tr>
<td>
<img src="/piano.jpg" width="48%"/>
<img src="/piano_corner.jpg" width="48%"/>
</td>
</tr>
</table>

* result4  <br />
from left to right: game_of_go, game_of_go_corner
<table border=1>
<tr>
<td>
<img src="/game_of_go.jpg" width="48%"/>
<img src="/game_of_go_corner.jpg" width="48%"/>
</td>
</tr>
</table>

* result5  <br />
from left to right: use_flag, usa_flag_high_threshold_corner
<table border=1>
<tr>
<td>
<img src="/roc_flag.jpg" width="48%"/>
<img src="/roc_flag_high_threshold_corner.jpg" width="48%"/>
</td>
</tr>
</table>

* result5  <br />
from left to right: use_flag, usa_flag_low_threshold_corner
<table border=1>
<tr>
<td>
<img src="/roc_flag.jpg" width="48%"/>
<img src="/roc_flag_low_threshold_corner.jpg" width="48%"/>
</td>
</tr>
</table>
