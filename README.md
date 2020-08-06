### 通过伪造一个应用包名为和平精英，获取系统权限，防止应用被卸载和覆盖，可以修改为你想替换的应用包名
![](https://raw.githubusercontent.com/zhao-pf/Prevent-Play-Games/master/image/img.jpg)  
免Root不让他玩游戏安装游戏
<!--more-->
# 开撸-新建一个Android项目  
1.直接创建一个空的接收者
```java
public class mDeviceAdminReceiver extends DeviceAdminReceiver {

}
```
2.填写我们超级管理员功能权限--[官方文档](https://developer.android.com/guide/topics/admin/device-admin "https://developer.android.com/guide/topics/admin/device-admin")
直接添加所有政策权限即可
```xml
<device-admin>
    <uses-policies>
        <limit-password />
        <watch-login />
        <reset-password />
        <force-lock />
        <wipe-data />
        <expire-password />
        <encrypted-storage />
        <disable-camera />
    </uses-policies>
</device-admin>
```
3.注册广播
```java
android:label="@string/activity_sample_device_admin" 指的是 Activity 的用户可读标签。
android:label="@string/sample_device_admin" 标签。
android:description="@string/sample_device_admin_description"说明通常比标签长，并且信息更丰富。
android:permission="android.permission.BIND_DEVICE_ADMIN"  是 DeviceAdminReceiver 子类必须具有的权限，用于确保仅系统可以与接收者互动（不应向任何应用授予此权限）。该权限可防止其他应用滥用您的设备管理应用。
android.app.action.DEVICE_ADMIN_ENABLED 是 DeviceAdminReceiver 子类必须处理才能获准管理设备的主要操作。当用户启用设备管理应用后，系统会针对接收者设置此操作。您的代码通常会在 onEnabled() 中处理此操作。要获得支持，接收者还必须请求 BIND_DEVICE_ADMIN 权限，以防止其他应用滥用。
```
```xml
<!--注册超级管理员的广播接收者-->
<receiver
        android:name=".mDeviceAdminReceiver"
        android:description="@string/description"
        android:label="系统优化"
        android:permission="android.permission.BIND_DEVICE_ADMIN">
    <meta-data
        android:name="android.app.device_admin"
        android:resource="@xml/admin" />
    <intent-filter>
        <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
    </intent-filter>
</receiver>
```
4.最后一步，启动即可
```java
        //利用设备超级管理员上锁App
        Intent intent = new Intent(DevicePolicyManager.ACTION_ADD_DEVICE_ADMIN);
        ComponentName mDeviceAdminSample = new ComponentName(MainActivity.this, mDeviceAdminReceiver.class);
        intent.putExtra(DevicePolicyManager.EXTRA_DEVICE_ADMIN, mDeviceAdminSample);
        intent.putExtra(DevicePolicyManager.EXTRA_ADD_EXPLANATION, "系统优化");
        //激活界面中显示的内容
        startActivity(intent);
```
### 没难度，记录一下黑科技，其实还可以进行其他的操作，比如隐藏启动图标，隐藏后通过其他应用打开入口，比如用拨号列表打开app等等操作，对了这只是对付一般的熊孩子，如果你还想安全一点，把设置界面加个安全锁就完全没问题了。
效果图:
![](https://image.smodel.top/images/2020/22/1.jpg)
![](https://image.smodel.top/images/2020/22/2.jpg)
![](https://image.smodel.top/images/2020/22/3.jpg)
