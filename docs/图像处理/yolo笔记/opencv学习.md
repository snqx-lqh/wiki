
检测实时帧率

```Python
import cv2
import time

def main():
    # 使用GStreamer管道从MIPI摄像头捕获视频，添加视频帧率
    gst_pipeline = 'v4l2src device=/dev/video11 ! video/x-raw,format=NV12,width=640,height=480,framerate=30/1 ! appsink'
    # 从GStreamer管道创建OpenCV VideoCapture对象
    cap = cv2.VideoCapture(gst_pipeline, cv2.CAP_GSTREAMER)

    if not cap.isOpened():
        # 如果无法打开摄像头，则输出提示信息
        print("无法打开摄像头")
        return
    time0 = time.time() # 获取开始时间
    while True:
        # 从摄像头捕获帧
        ret, frame = cap.read()
        frame = cv2.cvtColor(frame, cv2.COLOR_YUV2BGR_NV12)
        # 如果捕获到帧，则显示它
        if ret:
            time1 = time.time() # 获取结束时间
            fps = 1 / (time1 - time0) # 计算实时帧率
            time0 = time1 # 更新开始时间
            cv2.putText(frame, "FPS: {:.2f}".format(fps), (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2) # 在图像上显示帧率
            cv2.imshow("MIPI Camera", frame)
        # 按下'q'键退出循环
        if cv2.waitKey(1) & 0xFF == ord("q"):
            break

    # 释放资源并关闭窗口
    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()

```