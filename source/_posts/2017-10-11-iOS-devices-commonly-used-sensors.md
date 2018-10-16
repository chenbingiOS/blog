---
layout:     post
title:      "iOS 平台设备常用传感器"
subtitle:   "Implementation Of 	 With Swift4 Code"
date:       2017-10-11
author:     "ChenBing"
header-img: "img/post-bg-2015.jpg"
header-mask: 0.3
catalog:    true
tags:       [iOS,iPhone,Swift]
---

# iOS 平台设备传感器

iPhone 设备传感器类型与作用
<table>
	<thead>
		<tr class="header">
			<th>传感器类型</th>
			<th>作用</th>
		</tr>
	</thead>
	<tbody>
		<tr class="odd">
			<td>环境光传感器</td>
			<td>感应周边环境光线的强弱(自动调节屏幕亮度)</td>
		</tr>
		<tr class="even">
			<td>距离传感器</td>
			<td>感应是否有其他物体靠近设备屏幕（打电话自动锁屏）</td>
		</tr>
		<tr class="odd">
			<td>磁力计传感器</td>
			<td>感应周边的磁场</td>
		</tr>
		<tr class="even">
			<td>内部温度传感器</td>
			<td>感应设备内部的温度（提醒用户降温，防止损伤设备）</td>
		</tr>
		<tr class="odd">
			<td>湿度传感器</td>
			<td>感应设备是否进水（方便维修人员）</td>
		</tr>
		<tr class="even">
			<td>陀螺仪</td>
			<td>感应设备的持握方式（赛车类游戏）</td>
		</tr>
		<tr class="odd">
			<td>加速计</td>
			<td>感应设备的运动（摇一摇、计步器）</td>					
		</tr>
	</tbody>
</table>

[查看 Demo 源码](https://github.com/chenbingiOS/iOS-Sensor)

---

*   [加速度传感器](#accelerometer)
*   [陀螺仪传感器](#Gyro)
*   [距离传感器](#Proximity)
*   [电池电量](#Level)
*   [距离传感器](#Proximity)

---

<h2 id="accelerometer">加速度传感器(accelerometer)</h2>

加速度传感器是根据x、y和z三个方向来检测在设备位置的改变。<br />
通过加速度传感器可以知道当前设备相对于地面的位置。<br />
以下实例代码需要在真实设备上运行，在模拟器上是无法工作的。

Swift Code

<pre class="brush:swift;toolbar:false">

import CoreMotion

class ViewController: UIViewController {

    var cmm:CMMotionManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        cmm = CMMotionManager();
    }
    
    override func viewWillAppear(_ animated: Bool) {
        cmm.accelerometerUpdateInterval = 1.0
        if cmm.isAccelerometerAvailable {
            cmm.startAccelerometerUpdates(to:OperationQueue(), withHandler: {(data:CMAccelerometerData!, err:Error!) in
                print(data)
            })
        } else {
            print("加速度传感器不可用")
        }
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        if cmm.isAccelerometerActive {
            cmm.stopAccelerometerUpdates()
        }
    }
}

</pre>

<h2 id="Gyro">陀螺仪传感器(Gyro)</h2>

Swift Code

<pre class="brush:swift;toolbar:false">

import CoreMotion

class ViewController: UIViewController {

    var cmm: CMMotionManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        cmm = CMMotionManager()
    }

    override func viewWillAppear(_ animated: Bool) {
        cmm.gyroUpdateInterval = 1.0
        if cmm.isGyroAvailable {
            cmm.startGyroUpdates(to: OperationQueue(), withHandler: { (data:CMGyroData!, err:Error!) in
                print("Gyro Data \(data)")
            })
        } else {
            print("陀螺仪传感器不可用")
        }
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        if cmm.isGyroActive {
            cmm.stopGyroUpdates()
        }
    }
}

</pre>

<h2 id="Proximity">距离传感器(Proximity)</h2>

Swift Code

<pre class="brush:swift;toolbar:false">

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @objc func proximityChanged() {
        print("\(UIDevice.current.proximityState)")
    }
    
    override func viewWillAppear(_ animated: Bool) {
        UIDevice.current.isProximityMonitoringEnabled = true
        NotificationCenter.default.addObserver(self, selector: #selector(self.proximityChanged), name: NSNotification.Name.UIDeviceProximityStateDidChange, object: nil)
    }    
    
    override func viewWillDisappear(_ animated: Bool) {
        NotificationCenter.default.removeObserver(self, name: NSNotification.Name.UIDeviceProximityStateDidChange, object: nil)
    }
}

</pre>

<h2 id="Level">电池电量(Level)</h2>

Swift Code

<pre class="brush:swift;toolbar:false">

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @objc func levelChanged() {
        print("\(UIDevice.current.batteryLevel)")
    }
    
    override func viewWillAppear(_ animated: Bool) {
        UIDevice.current.isBatteryMonitoringEnabled = true
        
        print("\(UIDevice.current.batteryLevel)")
        
        NotificationCenter.default.addObserver(self, selector: #selector(self.levelChanged), name: NSNotification.Name.UIDeviceBatteryLevelDidChange, object: nil)
    }    
    
    override func viewWillDisappear(_ animated: Bool) {
        NotificationCenter.default.removeObserver(self, name: NSNotification.Name.UIDeviceBatteryLevelDidChange, object: nil)
    }
}

</pre>

<h2 id="Heading">磁力计传感器(Heading)</h2>

Swift Code
<pre class="brush:swift;toolbar:false">
class ViewController: UIViewController, CLLocationManagerDelegate {

    @IBOutlet weak var imgCompass: UIImageView!
    var lm:CLLocationManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        lm = CLLocationManager()
        lm.delegate = self
    }

    override func viewWillAppear(_ animated: Bool) {
        lm.startUpdatingHeading()
    }
    
    // 指南针
    func locationManager(_ manager: CLLocationManager, didUpdateHeading newHeading: CLHeading) {
        print(newHeading)
        imgCompass.transform = CGAffineTransform(rotationAngle: CGFloat((180-newHeading.magneticHeading)/180.0 * Double.pi))
    }
}

</pre>
---