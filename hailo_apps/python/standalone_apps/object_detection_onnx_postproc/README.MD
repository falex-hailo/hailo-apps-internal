Object detection - *yolo26* with lightweight onnx postprocessing
=============================================================

This example demonstrates object detection using a Hailo device.<br>
It processes input images, videos, or a camera stream and annotates it with the detected objects.<br>
Optionally, object tracking across frames can be enabled for video and camera streams.

<p align="center">
    <img src="../object_detection/output.gif" width="480" alt="demo" />    
</p>

Similar to [baseline object_detection](https://github.com/falex-hailo/hailo-apps-internal/tree/yolo26_onnx/hailo_apps/python/standalone_apps/object_detection), adding and exemplifying the following capabilities:
1. Using the new 2026 Ultralytics release of top performing, NMS-free networks. 
1. Using onnx-runtime engine for the lightweight postprocessing, exemplifying this easy integration pathway.

Requirements
------------
Same as for [baseline object detection](https://github.com/falex-hailo/hailo-apps-internal/tree/yolo26_onnx/hailo_apps/python/standalone_apps/object_detection#requirements) + onnx-runtime


Supported Models
----------------

The variants currently supported include:
- yolov26n, compiled for H8

ONNX postprocessing
-------------------
This example uses onnxruntime for the postprocessing part. It's somewhat similar to [hailo_apps/cpp/onnxrt_hailo_pipeline](https://github.com/hailo-ai/hailo-apps/tree/main/hailo_apps/cpp/onnxrt_hailo_pipeline) but in a stand-alone way, not within HaloRT. 
 This makes integration of new networks especially convenient, by following these steps:
1. Split the ONNX into the "neural processing" and the "postprocessing" parts using extract_postprocessing.py script
2. Process the first part into a HEF using the DFC
3. In runtime, apply the second part on the HEF outputs with onnx-runtime engine to complete an accelerated equivalent of the original ONNX. This runtime part is implemented and exemplified in this app.
4. The desired "full-onnx = HEF + postproc-onnx" equivalence can be conveniently debugged (isolating pipeline vs. compilation issues and HEF degradation) using --full-onnx flag that applies a bypass of the HEF (using the 'neural-processing' split-onnx 1st part). This is also useful for quick 'dry' tests without hardware or compilation at all - as well as benchmarking the acceleration provided by Hailo's offloading of the neural part to HEF running on hardware.

Install and Usage
-----------------
Same as for baseline object detection:

[Linux Installation](https://github.com/falex-hailo/hailo-apps-internal/tree/yolo26_onnx/hailo_apps/python/standalone_apps/object_detection#linux-installation)

[Windows Installation](https://github.com/falex-hailo/hailo-apps-internal/tree/yolo26_onnx/hailo_apps/python/standalone_apps/object_detection#windows-installation)


[Arguments and Examples](https://github.com/falex-hailo/hailo-apps-internal/tree/yolo26_onnx/hailo_apps/python/standalone_apps/object_detection#arguments)