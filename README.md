Download Link: https://assignmentchef.com/product/solved-ee5175-lab2-occlusion-detection-image-mosaicing
<br>
1          Occlusion Detection

In this problem, we are going to revisit the Occlusion Detection task from Lab-1. The task is to estimate the homography matrix using SIFT based features, assuming that you don’t know the underlying transformations and the point correspondences.

<h1>1.1       Steps</h1>

<ul>

 <li>Taking IMG2.pgm as the reference image, determine homography <em>H </em>between <em>I</em><sub>2 </sub>= IMG2.pgm and <em>I</em><sub>1 </sub>= IMG1.pgm such that <em>I</em><sub>1 </sub>= <em>H </em>× <em>I</em><sub>2</sub><em>.</em></li>

 <li>Transform <em>I</em><sub>2 </sub>using the estimated homography <em>H </em>(target-to-source mapping) and determine the changes between the two images.</li>

</ul>

Verify that the underlying transformations consist of in-plane rotation and translation. And also that the values matches with those computed in Lab-1.

<h1>1.2             Determining homography between two images</h1>

<ol>

 <li>Determine SIFT features of the two images and determine correspondences between them.File sift_corresp.m returns the SIFT correspondences between two images (see Section 1.4).</li>

</ol>

Now to find <em>H </em>such that:

 <sub>0</sub>   <em>x</em><em>i         </em><em>x</em><em>i</em>

<em>y</em><em>i</em>0 ∼ <em>H </em><sub></sub><em>y<sub>i</sub></em><sub></sub>

               

1                  1

<ol start="2">

 <li>Run RANSAC on matched points (correspondences) to remove outliers (wrong matches), andfind the homography between the two images.

  <ul>

   <li>Input: Matched points (<em>x<sub>i</sub>,y<sub>i</sub></em>) and (<em><sup>x</sup></em><sup>0</sup><em><sub>i</sub></em><em>,y<sub>i</sub></em><sup>0</sup>) with <em>i </em>∈</li>

   <li>Randomly pick four correspondences (so that we can form eight equations), i.e. (<em>x<sub>i</sub>,y<sub>i</sub></em>) and with <em>i </em>∈ R ⊂ M and |R| = 4, where | · | denotes the cardinality of the set.</li>

   <li>Calculate the homography <em>H </em>using the above four correspondences (see Section 1.3).</li>

   <li>For each of the remaining correspondences (<em>x<sub>i</sub>,y<sub>i</sub></em>) and (<em><sup>x</sup></em><sup>0</sup><em><sub>i</sub></em><em>,y<sub>i</sub></em><sup>0</sup>) with <em>i </em>∈ P = MR, check whether they satisfy the homography (within an error bound). If yes, add the index of that correspondence to the consensus set.</li>

  </ul></li>

</ol>

 <sub>00</sub>   <em>x</em><em>i         </em><em>x</em><em>i</em>

<sup></sup><em>y</em><sup>00</sup><sup></sup> ← <em>H </em><sup></sup><sub></sub><em>y<sub>i</sub></em><sup></sup><sub></sub><em>, </em>and normalize so that<em>, <sub>i</sub></em>

                 

00

<em>z<sub>i                              </sub></em>1

i.e. <em>x</em>00<em>i </em>← <em>x</em>00<em>i </em><em>/z</em><em>i</em>00 and <em>y</em><em>i</em>00 ← <em>y</em><em>i</em>00<em>/z</em><em>i</em>00

If , then update consensus set C ← C ∪ {<em>i</em>}.

(e) If the consensus set is large enough i.e. if |C| <em>&gt; d</em>(= 0<em>.</em>8|P|), then return this homography

<table width="402">

 <tbody>

  <tr>

   <td width="319"><em>H</em>, else go to step (b).(f) Output: Homography <em>H</em>.1.3          Calculating homography1. Consider a correspondence (<em>x,y</em>) and (<em>x</em><sup>0</sup><em>,y</em><sup>0</sup>),</td>

   <td width="25"> </td>

   <td width="58"> </td>

  </tr>

  <tr>

   <td width="319"> <sub>0</sub> <em>x             h</em><sub>1</sub> 0 = <sub></sub><em>h</em><sub>4 </sub><em>y </em>  <em>z</em>0                  <em>h</em>7</td>

   <td width="25"><em>h</em><sub>2 </sub><em>h</em><sub>5 </sub><em>h</em><sub>8</sub></td>

   <td width="58"> <em>h</em><sub>3            </sub><em>x</em><em>h</em>6<em>y</em><em>.</em> <em>h</em><sub>9            </sub>1</td>

  </tr>

 </tbody>

</table>

Upon normalizing <em>z</em><sup>0</sup>,

<em>x</em><sup>0 </sup>= <em>h</em><sub>1</sub><em>x </em>+ <em>h</em><sub>2</sub><em>y </em>+ <em>h</em><sub>3</sub><em>/h</em><sub>7</sub><em>x </em>+ <em>h</em><sub>8</sub><em>y </em>+ <em>h</em><sub>9</sub><em>, y</em><sup>0 </sup>= <em>h</em><sub>4</sub><em>x </em>+ <em>h</em><sub>5</sub><em>y </em>+ <em>h</em><sub>6</sub><em>/h</em><sub>7</sub><em>x </em>+ <em>h</em><sub>8</sub><em>y </em>+ <em>h</em><sub>9</sub><em>.</em>

Form two equations for each correspondence (corresponding to two rows of matrix <em>A</em>).

(<em>x</em>)<em>h</em><sub>1</sub>+(<em>y</em>)<em>h</em><sub>2</sub>+(1)<em>h</em><sub>3</sub>+(0)<em>h</em><sub>4</sub>+(0)<em>h</em><sub>5</sub>+(0)<em>h</em><sub>6</sub>

− (<em>x</em><sup>0</sup><em>x</em>)<em>h</em><sub>7 </sub>− (<em>x</em><sup>0</sup><em>y</em>)<em>h</em><sub>8 </sub>− (<em>x</em><sup>0</sup>)<em>h</em><sub>9 </sub>= 0

(0)<em>h</em><sub>1</sub>+(0)<em>h</em><sub>2</sub>+(0)<em>h</em><sub>3</sub>+(<em>x</em>)<em>h</em><sub>4</sub>+(<em>y</em>)<em>h</em><sub>5</sub>+(1)<em>h</em><sub>6</sub>

− (<em>y</em><sup>0</sup><em>x</em>)<em>h</em><sub>7 </sub>− (<em>y</em><sup>0</sup><em>y</em>)<em>h</em><sub>8 </sub>− (<em>y</em><sup>0</sup>)<em>h</em><sub>9 </sub>= 0

<ol start="2">

 <li>Solve the system,</li>

</ol>

  <em>h</em><sub>1</sub>

<em>h</em><sub>2</sub>



<em>A</em>8×9 …  = 0





  <em>h</em><sub>9</sub>

i.e., find the null space of <em>A</em>.

<ol start="3">

 <li>Homography matrix</li>

</ol>

<table width="121">

 <tbody>

  <tr>

   <td width="66"><em>h</em><sub>1</sub><em>H </em>= <em>h</em>4<em>h</em><sub>7</sub></td>

   <td width="27"><em>h</em><sub>2 </sub><em>h</em><sub>5 </sub><em>h</em><sub>8</sub></td>

   <td width="28"> <em>h</em><sub>3</sub><em>h</em>6<em>.</em><em>h</em><sub>9</sub></td>

  </tr>

 </tbody>

</table>

<h1>1.4         Using SIFT</h1>

Files needed:

<ol>

 <li>m</li>

 <li>m and</li>

 <li>sift (for GNU/Linux) or siftWin32.exe (for Windows).</li>

</ol>

Copy the above three files to the working directory. In GNU/Linux, check that the file sift has executable permission. If not, run chmod +x sift in a terminal.

Usage: [corresp1, corresp2] = sift_corresp(‘img1.pgm’,‘img2.pgm’)

2         Image mosaicing

<h1>2.1         Problem Statement</h1>

Image mosaicing is the alignment and stitching of a collection of images having overlapping regions into a single image. In this assignment, you have been given three images which were captured by panning the scene left to right. These images (mosaic1.pgm, mosaic2.pgm and mosaic3.pgm) capture overlapping regions of the same scene from different viewpoints. The task is to determine the geometric transformations (homographies) between these images and stitch them into a single image.

<h1>2.2       Steps</h1>

<ol>

 <li>Take mosaic2.pgm as the reference image.</li>

 <li>Determine homography <em>H</em><sub>21 </sub>between <em>I</em><sub>2 </sub>= pgm and <em>I</em><sub>1 </sub>= mosaic1.pgm such that <em>I</em>1 = <em>H</em>21<em>I</em>2.</li>

 <li>Determine homography <em>H</em><sub>23 </sub>between <em>I</em><sub>2 </sub>= pgm and <em>I</em><sub>3 </sub>= mosaic3.pgm such that <em>I</em>3 = <em>H</em>23<em>I</em>2.</li>

 <li>Create an empty canvas. For every pixel in the canvas, find corresponding points in <em>I</em><sub>1</sub>, <em>I</em><sub>2 </sub>and <em>I</em><sub>3 </sub>using <em>H</em><sub>21</sub>, identity matrix and <em>H</em><sub>23 </sub>respectively (target-to-source mapping). Blend the three values by averaging them. Employ the values in blending only if it falls within the corresponding image bounds. Choose the origin so as to get a full mosaic.</li>

</ol>

2.2.1            Creating the mosaic

Pseudo-code:

canvas = zeros(NumCanvasRows,NumCanvasColumns); for jj = 1:NumCanvasColumns

for ii = 1:NumCanvasRows

i = ii – OffsetRow; j = jj – OffsetColumn;

tmp = <em>H</em><sub>21 </sub>* [i;j;1]; i1 = tmp(1) <em>/ </em>tmp(3); j1 = tmp(2) <em>/ </em>tmp(3);

tmp = <em>H</em><sub>23 </sub>* [i;j;1]; i3 = tmp(1) <em>/ </em>tmp(3); j3 = tmp(2) <em>/ </em>tmp(3);

v1 = BilinearInterp(i1,j1,<em>I</em><sub>1</sub>); v2 = BilinearInterp(i,j,<em>I</em><sub>2</sub>); v3 = BilinearInterp(i3,j3,<em>I</em><sub>3</sub>);

canvas(ii,jj) = BlendValues(v1,v2,v3);

end

end

where (OffsetRow, OffsetColumn) can be chosen such that the final mosaic will fit within the canvas.

<h1>2.3          Capturing your own data</h1>

Use your mobile phone camera to capture images (three or more), and run your code to generate the mosaic. Ensure that there is adequate overlap between successive images, and the camera is imaging a far-away scene (think why). Bring down the resolution of the input images such that the <em>width &lt; </em>1000 pixels, and convert them to grayscale before using them for mosaicing. (In MATLAB, use ‘imresize’ to reduce the image resolution, and ‘rgb2gray’ for conversion to grayscale)

–end–