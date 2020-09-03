
# Introduction : 
This is package enables a developer to use Foreground service within its React native app. The library works only with android since IOS doesn't support foreground service.

### Foreground service
A **foreground service** performs some operation that is noticeable to the user. For example, an audio app would use a **foreground service** to play an audio track. **Foreground services** must display a Notification. **Foreground services** continue running even when the user isn't interacting with the app. [Read More](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwia7qDp2MjrAhUS5uAKHb3OCgAQFjADegQICxAI&url=https%3A%2F%2Fdeveloper.android.com%2Fguide%2Fcomponents%2Fservices&usg=AOvVaw09DFr2GRCWtnLbq_m8UTRv)
  
  In short, a Foreground service is a service that runs even when you don't have your app running on recent apps. A foreground service is a notification that lets you run a background task.

Before anything you need to set up foreground service and do the manual linking, here is the link to the 
[Installation Docs](https://github.com/Raja0sama/-supersami-react-native-foreground-service/blob/master/Installation.md#installation)


[Working Example](https://github.com/Raja0sama/ForegroundSerivceExample.git)

# Basic Usage:

#### 1
The very first step will be to register the foreground service in your root index.js file.
```
import  ReactNativeForegroundService  from  '@supersami/react-native-foreground-service';
// This will register your headless task.
ReactNativeForegroundService.register();
```
#### Usage : 
```
const App = () => {
  const onStart = () => {
    // Checking if the task i am going to create already exist and running, which means that the foreground is also running.
    if (ReactNativeForegroundService.is_task_running('taskid')) return;
    // Creating a task.
    ReactNativeForegroundService.add_task(
      () => console.log('I am Being Tested'),
      {
        delay: 100,
        onLoop: true,
        taskId: 'taskid',
        onError: (e) => console.log(`Error logging:`, e),
      },
    );
    // starting  foreground service.
    return ReactNativeForegroundService.start({
      id: 144,
      title: 'Foreground Service',
      message: 'you are online!',
    });
  };

  const onStop = () => {
    // Make always sure to remove the task before stoping the service. and instead of re-adding the task you can always update the task.
    if (ReactNativeForegroundService.is_task_running('taskid')) {
      ReactNativeForegroundService.remove_task('taskid');
    }
    // Stoping Foreground service.
    return ReactNativeForegroundService.stop();
  };
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <SafeAreaView
        style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
        <Text>An Example for React Native Foreground Service. </Text>
        <Button title={'Start'} onPress={onStart} />
        <Button title={'Stop'} onPress={onStop} />
      </SafeAreaView>
    </>
  );
};
```



## Methods
  
| methods         | purpose  | parameters |
|------------------|---|--|
| register         | to Register a foreground service on start-up of the app  | - |
| start          | to start the foreground service   | id*, title ,message  ,vibration ,visibility ,icon,largeicon ,importance,number,button,buttonText ,buttonOnPress,  mainOnPress |
| update           | to update foreground service | same as start paramters |
| stop             | to stop the foreground service notification  | - |
| is_running       | to Check if foreground service is running | - |
| add_task         | to add task to the queue | task*, object{    delay,    onLoop ,    taskId ,  onSuccess ,    onError } |
| update_task      | Update task in the queue  | same as add task |
| remove_task      | Remove task from the queue  | taskid |
| is_task_running  |  Check if the task is running | taskid |
| remove_all_tasks | Remove all the task  | - |
| get_task         |  Get Specific task | taskid |
| get_all_tasks    |  Get All Tasks. |



### Parameters

| Key                                        | Purpose  |
|--------------------------------------------|---|
| id - Notification                                      |  Unique Id of the notification with which you can latter stop or update a foreground (Required) service  |
| title - Notfication                                | String title of the notification  |
| message - Notification  |  String Message of the Notification  |
| vibration - Notification                        | [Link the the docs]([https://developer.android.com/reference/kotlin/android/app/Notification](https://developer.android.com/reference/kotlin/android/app/Notification))  |
| visibility       - Notification              |  [Link the the docs]([https://developer.android.com/reference/kotlin/android/app/Notification](https://developer.android.com/reference/kotlin/android/app/Notification))  |
| icon         - Notification             | Icon name, the one in drawable, by default it is ic_launcher |
| largeicon          - Notification       |  Icon name, the one in drawable, by default it is ic_launcher  |
| importance                - Notification        | [Link the the docs]([https://developer.android.com/reference/kotlin/android/app/Notification](https://developer.android.com/reference/kotlin/android/app/Notification))   |
| number   - Notification                           | for displaying a badge, just like the one in icons  |
| button        - Notification                   | Boolean, true if you want a button in the notification  |
| buttonText               - Notification            |  Text to display in the button  |
| buttonOnPress        - Notification   | a unique identifier as a string as you will going to receive this on click of your button  |
| mainOnPress     - Notification          | a unique identifier as a string as you will going to receive this on click of your notification  |
| delay - task                    |  Delay of the task  |
| onLoop - task                   | if the task is recurring  |
| taskId - task      |  string identifier of the task  |
| onSuccess  - task         | function callback  |
| onError - task            | function callback  |
