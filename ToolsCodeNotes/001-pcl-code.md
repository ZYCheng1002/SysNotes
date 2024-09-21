# 1. PCL闪退
```c++
pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
cloud->points.push_back(pcl::PointXYZ(0, 0, 0));
pcl::visualization::PCLVisualizer viewer("Simple Viewer");
viewer.addPointCloud(cloud, "sample cloud");
while (!viewer.wasStopped()) {
    viewer.spinOnce(100);
}
```
gdb 调试信息为:
```bash
#0  0x00007ffff3b04c10 in _XEventsQueued () from /lib/x86_64-linux-gnu/libX11.so.6
#1  0x00007ffff3af1291 in XPending () from /lib/x86_64-linux-gnu/libX11.so.6
#2  0x00007ffff6852b8f in vtkXRenderWindowInteractor::StartEventLoop() ()
   from /lib/x86_64-linux-gnu/libvtkRenderingUI-9.1.so.1
#3  0x00007ffff7f0ff8c in pcl::visualization::PCLVisualizer::spinOnce(int, bool) ()
   from /lib/x86_64-linux-gnu/libpcl_visualization.so.1.12
#4  0x0000555555556a6f in main ()
```
解决方法:
viewer.spinOnce(100) 更换为viewer.spin()
