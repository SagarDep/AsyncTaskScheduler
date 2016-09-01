#AsyncTaskScheduler
[![](https://jitpack.io/v/SilenceDut/AsyncTaskScheduler.svg)](https://jitpack.io/#SilenceDut/AsyncTaskScheduler)
##Background
[详细解读AsyncTask的黑暗面以及一种替代方案](http://silencedut.coding.me/2016/07/08/%E5%9F%BA%E4%BA%8E%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC%E7%9A%84AsyncTask%E6%BA%90%E7%A0%81%E8%A7%A3%E8%AF%BB%E5%8F%8AAsyncTask%E7%9A%84%E9%BB%91%E6%9A%97%E9%9D%A2/)
##Characters
- execute tasks in parallel as default, rather than processing them sequentially
- execute single task use a thread rather than an executor 
- you can set a default executor to execute tasks
- a callback for task error
- manage multiple  tasks easily
- you can you it on any thread ,anf it will have a callback on main thread。

##Methods Introduction
methods like AsyncTask ,use easily

- doInBackground : background thread
- onProgressUpdate : main thread
- onExecuteSucceed : main thread
- onExecuteCancelled : main thread
- onExecuteFailed : main thread,when unexpected happened
      
##Add to project
**latest-version**:
[![](https://jitpack.io/v/SilenceDut/AsyncTaskScheduler.svg)](https://jitpack.io/#SilenceDut/AsyncTaskScheduler)

Step 1. Add the JitPack repository to your build file

**gradle**
```groovy
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
    }
}
```
**maven**
```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```

Step 2. Add the dependency

**gradle**

```groovy
compile 'com.github.SilenceDut:AsyncTaskScheduler:{latest-version}'
```
**maven**

```xml
<dependency>
    <groupId>com.github.SilenceDut</groupId>
    <artifactId>AsyncTaskScheduler</artifactId>
    <version>{latest-version}</version>
</dependency>
```
##How to use
**Single background task ,use a single thread rather than an Executor, save resource**

```java
SingleAsyncTask singleTask = new SingleAsyncTask<Void,String>() {   
   @Override    
   public String doInBackground() {   
       return null;   
   }
   @Override
   public void onExecuteSucceed(String result) {      
       super.onExecuteSucceed(result);      
   }
   @Override
   public void onExecuteFailed(Exception exception) {      
       super.onExecuteFailed(exception);    
       Log.i(TAG,"onExecuteCancelled:"+exception.getMessage()+Thread.currentThread());
   }
};
singleTask.executeSingle();

//取消通过executeSingle执行的任务
mSingleAsyncTask.cancel(true);
```

**multiple background tasks,create a task scheduler to manage them.**

```java
//多个任务新建一个任务调度器
AsyncTaskScheduler mAsyncTaskScheduler = new AsyncTaskScheduler();

SingleAsyncTask singleTask1 = new  SingleTask() { ... }；
SingleAsyncTask singleTask2 = new  SingleTask() { ... }；
SingleAsyncTask singleTask3 = new  SingleTask() { ... }；
...

//并行执行多个任务
mAsyncTaskScheduler.execute(singleTask1)
.execute(singleTask2).execute(singleTask3).

 //取消通过AsyncTaskScheduler任务
mAsyncTaskScheduler.cancelAllTasks(true);
```

**you can set a default Executor to execute tasks**

```java
//设置默认的线程池
Executor defaultPoolExecutor = ...
AsyncTaskScheduler mAsyncTaskScheduler = new AsyncTaskScheduler(Executor defaultPoolExecutor);
```

**make sure  cancel task to avoid memory leak**

```java
//取消通过executeSingle执行的任务
mSingleAsyncTask.cancel(true);

//取消通过AsyncTaskScheduler任务
mAsyncTaskScheduler.cancelAllTasks(true);
```      
License
-------

    Copyright 2015-2016 SilenceDut

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
