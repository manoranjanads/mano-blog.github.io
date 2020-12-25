---
title: "How to Setup XCode for OpenCV Development"
permalink: /fundamentals/how-to-setup-xcode-for-opencv-development.html
excerpt: "Meetrix Simple Classroom"
# categories:
#   - Environment Setup
# tags:
#   - xcode
#   - opencv
#   - c++
header:
  overlay_image: /assets/images/fundamentals/2018-10-14-how-to-setup-x-code-for-open-cv-development/opencv_xcode.png
  teaser: /assets/images/fundamentals/2018-10-14-how-to-setup-x-code-for-open-cv-development/opencv_xcode.png
  overlay_filter: 0.8
last_modified_at: 2018-09-24T15:58:49-04:00
toc: true
author: Mano
---
2018-10-14-how-to-setup-x-code-for-open-cv-development.md
## Install OpenCV and pkg-config

`brew install opencv` then
`brew install pkg-config`


## Header and Library Search Paths

Open CV get installed in `/usr/local/Cellar/opencv/<VERSION_NUMBER>/`

Header Search Path : `/usr/local/Cellar/opencv/<VERSION_NUMBER>/include`

ex: `/usr/local/Cellar/opencv/3.4.2/include`

Library Search Path : `/usr/local/Cellar/opencv/<VERSION_NUMBER>/lib`

ex: `/usr/local/Cellar/opencv/3.4.2/lib`


#Linker Flags

To get the linker flags, we need outputs from both of following commands

`pkg-config --cflags --libs opencv`

ex: `-I/usr/local/Cellar/opencv/3.4.2/include/opencv -I/usr/local/Cellar/opencv/3.4.2/include`

`pkg-config --libs opencv`

ex: `-L/usr/local/Cellar/opencv/3.4.2/lib -lopencv_stitching -lopencv_superres -lopencv_videostab -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dpm -lopencv_face -lopencv_photo -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_line_descriptor -lopencv_optflow -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_dnn -lopencv_plot -lopencv_xfeatures2d -lopencv_shape -lopencv_video -lopencv_ml -lopencv_ximgproc -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_flann -lopencv_xobjdetect -lopencv_imgcodecs -lopencv_objdetect -lopencv_xphoto -lopencv_imgproc -lopencv_core`

when concatenated

`-I/usr/local/Cellar/opencv/3.4.2/include/opencv -I/usr/local/Cellar/opencv/3.4.2/include -L/usr/local/Cellar/opencv/3.4.2/lib -lopencv_stitching -lopencv_superres -lopencv_videostab -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dpm -lopencv_face -lopencv_photo -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_line_descriptor -lopencv_optflow -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_dnn -lopencv_plot -lopencv_xfeatures2d -lopencv_shape -lopencv_video -lopencv_ml -lopencv_ximgproc -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_flann -lopencv_xobjdetect -lopencv_imgcodecs -lopencv_objdetect -lopencv_xphoto -lopencv_imgproc -lopencv_core`