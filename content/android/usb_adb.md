+++
date = "2015-10-26T15:39:24+08:00"
draft = true
title = "adb识别新的usb连接"

+++


+ 当有一个新的Android设备想要连接电脑时，如果识别不了，则需要主动将该设备的设备码id添加到abd_usb.ini的配置文件中。接下来介绍的是如何查找新设备的设备码。
	<pre>
	//mac下对应的命令
	system_profiler SPUSBDataType > ~/usb.txt  </pre>
	打开文件usb.txt，在最后的```USB Hi-Speed Bus: -> Hub -> Android```可以找到这个Android的条目，其中的```Vendor ID: 0x2a70```就是对应的id，追加到```echo 0x2a70 >> ~/.android/adb_usb.ini```中即可。
