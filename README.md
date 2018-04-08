# USERSIR

USERSIR 适用于 Android 8.0+ 设备.

## 什么是 USERSIR？

在一部手机上,拥有设备管理员权限的 APP 只能有一个.USERSIR 提供 API 接入方式来支持多个 APP 运行在特权模式.

## 为何使用 USERSIR？

相比其他管理软件(如果有), USERSIR 有以下优点:

USERSIR 提供了一个较为健全的权限管理系统.应用申请特权模式时, USERSIR 会弹出对话框让用户来选择授权与否.同时你可以随时取消应用的特权模式权限.



## 开发者如何适配？

1.添加依赖

```groovy
  //todo
```
2.在 manifest 中添加权限
```xml
<uses-permission android:name="vc.https.usersir.permission.privileged_API" />
```
3.判断 USERSIR 是否已安装:

```java
UsersirCompat.isUsersirInstalled(packageManager);
```

4.判断应用是否已有特权模式权限:

```java
UsersirCompat.checkSelfPermission(this);
```

5.调用系统流程申请权限：

```java
ActivityCompat.requestPermissions(this, new String[]{UsersirCompat.usersir_permission}, REQUEST_CODE);
```
在`onRequestPermissionsResult()`中回调确认后,再调用 `UsersirCompat.requestRefresh(this);` 即可刷新 APP 的权限.
```java
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch (requestCode) {
            case REQUEST_CODE:
                if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    UsersirCompat.requestRefresh(this);
                } else {
                    // user denied or error.
                }
                return;
            default:
                super.onActivityResult(requestCode, resultCode, data);
        }
    }
```

6.调用

直接调用 devicePolicyManager 方法,需要 componentName 传空值即可,eg:

 停用应用:

```java
devicePolicyManager.setApplicationHidden(null, pkg, freeze);
```

是否停用:

```java
devicePolicyManager.isApplicationHidden(null, pkg);
```
