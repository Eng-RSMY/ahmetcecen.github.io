---
layout: post
title:  "Testing E-mail Parsing with Zapier"
date:   2014-08-01 03:06:25
categories: main
---

---------- Forwarded message ----------
From: Ahmet Cecen <ahmetcecen@gmail.com>
Date: Thu, Jul 31, 2014 at 3:51 AM
Subject: Re: 回覆︰ Regarding 3D volume image
To: 小明 李 <tt611611@yahoo.com.hk>


Don't get me wrong I enjoy exploring different datasets, however your
problem is no longer a simple image processing task I can handle in 3-5
minutes. It is extremely specific to your data and how it was handled
before. I need to put in significant thought and/or time out of my life to
figure that out. At this point you have 2 options:

1) Repose your question at the Mathworks Answers forum and look for someone
who maybe have worked with similar datasets before. I have fairly extensive
experience with image processing but not with any similar datasets. Someone
in computational biology might not have to put much thought into it since
they have the experience. I would suggest splitting the problem as much as
possible so that you can find people with experience on smaller subsections
easier. I will keep checking the forum, so if I see something else I can
answer relating to your project I will help out.

2) Less preferably: You would have to extend a collaboration with an e-mail
conversation with you and your professor, and I would need a stake in the
overall project. We would go over the overall project and I would help in
any portion I can. Keep in mind my expertise is in image processing and
statistical analysis of data in *materials science*, so I am not sure how
attractive of an option this is. I am just covering all bases.

Regardless, it was interesting being exposed to this dataset. Let me tell
you what I can see in 2-3 minutes looking at the first dataset you sent me
with the images. It looks like for the 3D dataset, you have multiple images
corresponding to the same z coordinate (at the same spatial location). The
first one is the microscopy image that I am guessing you don't want, and
the rest are things you do want. My guess is that you want to grab all
images that are *RED *and stack them on top of each other, then do the same
thing with *GREEN *etc. and obtain several 3D datasets that are each
512x512x(however many slices). Then you can use each of those 3D datasets
at paraview, you can display them individually or overlayed together like
in the image "hSMC-CONTROL" you sent earlier. The details on how to achieve
those tasks is something I can't tell without a full briefing on the data
and some time spent experimenting on it myself, which as I said, I can't
since I have many other tasks to work on, unless you choose option 2 for
some reason.

Good luck with your project and studies, in case we don't communicate again.



On Thu, Jul 31, 2014 at 2:44 AM, 小明 李 <tt611611@yahoo.com.hk> wrote:

> Thanks a lot.
> It worked much better. However,the code to read stacks images is not good .
> ____1___________________
>
> folderOfInterest = 'C:\Users\user\Desktop\matlab\b'; % this is the path to your folder
> cd(folderOfInterest); % make current folder your folder
> ext = '.tif'; % here's the extension of the images
> files = dir; % get a listing of the current directory
>
> % counts how many images are in current folder
> isImageOfInterest = ~cellfun(@isempty,...
>   regexp(cellstr(char(files.name)),'.tif','ignorecase'));
> N = nnz(isImageOfInterest) ;
>
> Images = cell(N,1);
> cnt = 0;
> for i = 1:length(files)
>   if isImageOfInterest(i)
>     cnt = cnt+1;
>     Images{cnt} = imread(files(i).name);
>
>
>   end
> end
>
>
> This code will separate every tif image to (:,:,1) (:,:,2) (:,:,3)
> so this became 630 frames at the end.
> this contains a lot of useless fram because my original image already were
> taken from  (:,:,1) (:,:,2) (:,:,3).
>
> _2_________________________
>
> %process a bunch of TIFF files into a volume
> %the original images were had-colored by region to one of 7 colors
> %so the scalar value on the output runs from from 0 to 7
> %file setup
> filebase = 'e:\Audax';
> startFrame = 1;
> endFrame = 74;
> %read frames, reduce size, show frames, and build volume
> for i=startFrame:endFrame
>     filename=[filebase, num2str(i,'%2d'),'.tif']
>     temp=double(imresize(imread(filename), 0.5));
>     slice(:,:,i) = (temp(:,:,1)==255) + 2*(temp(:,:,2)==255) + 4*(temp(:,:,3)==255);
>     imagesc(slice(:,:,i));
>     colormap('gray')
>     drawnow
> end
>
>
> This code functions Well,However, it only combined images in black and white (binary).
>
> ________________________________________________
>
> most of the code I found online are same as the first one.
>
> Q1) So, Any ways which can read the stacks images without separating every slice into 3?
>
> Sorry to ask stupid question. I am quite new to image processing.
>
>
> Regarding the background information, I actually don't know much about the experiment. all I know is that it is the stacks of confocal cell and my professor request me to do that for him. I guess
>  it is from biology field.
>
>
> __________________________
>
> Besides, I have sent you two images(using matlab 3D viewer to view that) in binary .
>
> I guess the layering problem still there. But,the side view seems much more smoother than before.
>
>
> Q2)So I should keep deleting the useless layers/slices until it looks very smooth?
>
>
> Thanks
>
> UG Student from mechanical dept
>
> Steven
>
>
>
>
>   Ahmet Cecen <ahmetcecen@gmail.com> 於 2014年07月31日 (週四) 7:46 AM 寫道﹕
>
>
>  Okay, lets see:
>
> 1) Q1 is my mistake:
>
> A=permute(A,[1 3 2]);
>  newZ=round(100*1.126); %100 being the reduced dimension I assume.
> *B=zeros(512,newZ,512);  % Allocation error.*
> for i=1:512
>      B(:,:,i)=imresize(A(:,:,i),[512 newZ],'bilinear'); % You wanna keep X
> scale the same (512) while streching Z a little bit (z is now the second
> index thanks to permute).
> end
> Afixed=permute(B,[1 3 2]); %Permute back the image to X Y Z.
>
> 2) Q2: This is hard to explain in detail, but think about this. You copy
> paste an image on te a powerpoint slide, you can *stretch* or *shrink *it
> at will, we are doing something similar. We are streching one side of the
> image while keeping the other length the same (square -> rectangle for
> example). It is actually interpolation, not extrapolation as I suggested
> before.
>
> 3) Q3 is problematic. WriteToVTK is to my knowledge the only
> implementation of VTK that is readily available in the file exchange, and
> it is very outdated. You have 3 options here: 1) Use an older version of
> paraview (which is what I usually do) for example *paraview-3.6.2* 2)
> Find/write a new writetovtk file. 3) find another visualization software
> that can use another file format.
>
> The layering problem is still not fixed in your vtk file by the way. You
> still have a mix of actual microscopy images and segmented (I am assuming)
> protein images. Clear out the microscopy images.
>
> Let me know if it works.
>
> On a side note: I am curious about the background information on this
> data. If it is not a particularly kept secret, I would like to learn what
> is actually being imaged (the purpose), how the image was acquired (the
> tomography technique), what process it went through before the current
> stage (segmentation, cleaning, refining etc.), and what will happen after
> this stage (what kind of analysis? statistics?). This dataset appears to be
> completely outside of my field so I am just curious about it. I generally
> work with materials science datasets. I am also curious if any of my
> analysis tools have any applications in your field.
>
>
>
>
 On Wed, Jul 30, 2014 at 1:14 PM, 小明 李 <tt611611@yahoo.com.hk> wrote:
>
>  Sorry, I don't know I should send you email to which email address.
>
> I combine new problem into this email for you easily to read.
>
> Q1
>
> I used a 3D array A 512X512 X100 to test it.
> after permute, Array A became 512X100X512.
> newZ=round(1000*1.126)=113
>
>
>
> for i=1:512
>      B(:,:,i)=imresize(A(:,:,i),[512 newZ],'bilinear'); % You wanna keep X
> scale the same (512) while streching Z a little bit (z is now the second
> index thanks to permute).
> end
>
> Subscripted assignment dimension mismatch.
>
> when I run for loop , Subscripted assignment dimension mismatch.
> popped up.
>
> Did you encounter this problem?
>
> _________________________________________
>
> Q2
>
> newZ=round(100*1.126); %100 being the reduced dimension I assume.
> for i=1:512
>      B(:,:,i)=imresize(A(:,:,i),[512 newZ],'bilinear'); % You wanna keep
> X scale the same (512) while streching Z a little bit (z is now the second
> index thanks to permute).
> end
>
> I know you are doing interpolation in [512 ,newZ], right?
> However, the Array A only have 512X100x512 dimension.
> So , how to interpolate in [512 newZ]?
> newZ >100
>
> _______________________________________________________________________________
> Q3
>
> First, I should apologize for sending you few emails in short time.
> I have sent you two rar file which contains the vtk file after the
> operation and the original stacks images
> For the resize part, I use [512 100] to do the operation since I don't
> know how to do the operation using [512 100*1.126]..
>
> I loaded the VTK into ParaView and switch the Representation to Volume.
> It popped up
>
> ERROR: In
> C:\DBD\pvs-x64\paraview\src\paraview\VTK\Rendering\OpenGL\vtkOpenGLExtensionManager.cxx,
> line  757
> vtkOpenGLExtensionManager (000000000B5DFC00): Extension GL_VERSION_1_2
> could not be loaded.
>
>
> ERROR: In
> C:\DBD\pvs-x64\paraview\src\paraview\VTK\Rendering\OpenGL\vtkOpenGLExtensionManager.cxx,
> line 757
> vtkOpenGLExtensionManager (000000000B5DFC00): Extension GL_VERSION_1_2
> could not be loaded.
>
>
> ERROR: In
> C:\DBD\pvs-x64\paraview\src\paraview\VTK\Rendering\Volume\vtkGPUVolumeRayCastMapper.cxx,
> line 337
> vtkOpenGLGPUVolumeRayCastMapper (000000001D1314E0): scalar of type
> VTK_CHAR is not supported because this type is platform dependent. Use
> VTK_SIGNED_CHAR or VTK_UNSIGNED_CHAR instead.
>
>
> I don't Know which part I did wrong.
> If i switched slice, it is okay to show every slice.
> But volume, can't show that
>
> Thank you
>
> __________________________________________________
>
> https://www.dropbox.com/home/Data
>  小明 李從 Dropbox 分享的檔案﹕
>  rar data01.rar <https://www.dropbox.com/s/ato7q6f1fsqhr73/data01.rar>
>
>
>
>
> --
> Ahmet Cecen
> Graduate Student @ Georgia Tech
> Computational Science and Engineering
>
> E-mail: ahmetcecen@gatech.edu
> Cell: +12675864505
> Skype: +16787015869 (ahmetcecen)
>
>
>
>


-- 
Ahmet Cecen
Graduate Student @ Georgia Tech
Computational Science and Engineering

E-mail: ahmetcecen@gatech.edu
Cell: +12675864505
Skype: +16787015869 (ahmetcecen)