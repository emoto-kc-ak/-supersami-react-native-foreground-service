# Installation


To get started, firstly install the package with either npm or yarn on your project
```
yarn add @raja0sama/react-native-foreground-service@1.0.1
````

## Setting Up
Unfortunately, we only support manual linking at the moment, also looking for a person to help us set up automatic linking for this library. You need to follow these 3 steps carefully and make sure you don't miss any.
 
 - Adding Permissions.
 - Registering Service in the Manifest.
 - Setting up listeners to get data on the interaction with the notification. 



 # Introduction

### 1#
To Start using foreground service you need to add Permissions for foreground service as well as for wake lock by adding these 2 lines in your android manifest.

```
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/> 
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

### 2#
The Next Step is to Register Service in your manifest inside the application.

```
<meta-data android:name="com.supersami.foregroundservice.notification_channel_name" android:value="supersami Service"/> 
<meta-data android:name="com.supersami.foregroundservice.notification_channel_description" android:value="supersami Service."/> 
<meta-data android:name="com.supersami.foregroundservice.notification_color" android:resource="@color/orange"/>
<service android:name="com.supersami.foregroundservice.ForegroundService"></service>
<service android:name="com.supersami.foregroundservice.ForegroundServiceTask"></service>

```


Go to your Android -> App -> src -> main - res
looks for the values-color.xml, If not exist Create one.
and then add this, otherwise, just add the orange color and integer array

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
<item  name="orange"  type="color">#FF4500
</item>
<integer-array  name="androidcolors">
<item>@color/orange</item>
</integer-array>
</resources>
```

###  3#
The Next Step will be to go to your main Activity and add support to receive interactions of notification in your js code. To do that Declare a global variable like this.
```
	public  boolean  isOnNewIntent = false;
```
then you will need to override two methods of MainActivity. OnNewIntent as Well as OnStart 


> Make sure you override onNewIntent First and then OnStart.

- onNewIntent
```
  

	@Override
	public  void  onNewIntent(Intent  intent) {
		super.onNewIntent(intent);
		isOnNewIntent = true;
		ForegroundEmitter();
	}

```
- onStart
``` 

	@Override
	protected  void  onStart() {
		super.onStart();
		if(isOnNewIntent == true){}else {
			ForegroundEmitter();
		}
	}
```
By overriding these methods you are instructing java to emit the interactions of the notification on both dead and background state.
Finally, we will declare our main Function - ForegroundEmitter
```
  

public  void  ForegroundEmitter(){
// this method is to send back data from java to javascript so one can easily
// know which button from notification or the notification button is clicked
	String  main = getIntent().getStringExtra("mainOnPress");
	String  btn = getIntent().getStringExtra("buttonOnPress");
	WritableMap  map = Arguments.createMap();
	if (main != null) {
		map.putString("main", main);
	} 
	if (btn != null) {
		map.putString("button", btn);
	}
	try {
		getReactInstanceManager().getCurrentReactContext()
		.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
		.emit("notificationClickHandle", map);  	
	} catch (Exception  e) {	
	Log.e("SuperLog", "Caught Exception: " + e.getMessage());
	}
}
```


That's it, You have completed the setup.
