# MyOkHttpClient
框架使用文档
使用方法：
在项目根目录下
allprojects {
    repositories {
        mavenCentral()
        maven {url "https://raw.githubusercontent.com/liangjunzhi/MyOkHttpClient/master"}
    }
}
在需要用到的地方
    implementation 'com.android:library:1.0'
一、网络框架
（一）介绍：封装okhttp3
（二）使用方法：
1)设置接口请求公共部分：MyOkHttpClient.BaseUrl不设置默认为空
例：MyOkHttpClient.BaseUrl = "https://api-app.saler365.com/";
2)设置是否打印log：MyOkHttpClient.is_log 默认为false
    例:MyOkHttpClient.is_log = true;
3)一共支持三种请求方法 
第一种表单get  
        RequestParams params = new RequestParams();
        params.put("telephone", "15000611491");
        MyOkHttpClient.Get("api/send/verify", params, new DisposeDataListener() {
            @Override
            public void onSuccess(Object responseObj) {
               
            }

            @Override
            public void onFailure(int code, String message) {

            }
        });
200成功onSuccess方法 其他onFailure方法
responseObj为后台返回的数据
Code为错误码 -1为网络错误 -2为json解析异常 其他为后台返回的错误码
Message为错误信息
2)表单post  
        RequestParams params = new RequestParams();
        params.put("telephone", "15000611491");
        MyOkHttpClient.Post("api/send/verify", params,new DisposeDataListener() {
            @Override
            public void onSuccess(Object responseObj) {
            }
            @Override
            public void onFailure(int code, String message) {
            }
        });
3)JsonPost
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("name", "tsy");
            jsonObject.put("age", 24);
            jsonObject.put("type", "json");
        } catch (JSONException e) {
            e.printStackTrace();
        }
        MyOkHttpClient.JsonPost("api/send/verify", jsonObject.toString(),new DisposeDataListener() {
            @Override
            public void onSuccess(Object responseObj) {
            }
            @Override
            public void onFailure(int code, String message) {
            }
        });
    
二、Socket
（一）介绍：socket封装
（二）使用:
    private void startConnect(){
        String WEBSOCKET_URL="wss://crm.zihai.cn/ws?terminal=100&identity="+"";
        WsManager wsBaseManager = new WsManager.Builder(getBaseContext())
                .client(new OkHttpClient().newBuilder()
                        .pingInterval(15, TimeUnit.SECONDS)
                        .retryOnConnectionFailure(true)
                        .build())
                .needReconnect(true)
                .wsUrl(WEBSOCKET_URL)
                .build();
        wsBaseManager.setWsStatusListener(wsBaseStatusListener);
        wsBaseManager.startConnect();
    }
    WsStatusListener wsBaseStatusListener = new WsStatusListener() {
        @Override
        public void onOpen(Response response) {
            super.onOpen(response);
            //协议初始化  心跳等
            Log.i("i", "onOpen: ");
        }
        @Override
        public void onMessage(String text) {
            super.onMessage(text);
            //ReceiveMsg(text);
            //消息处理
        }
        @Override
        public void onMessage(ByteString bytes) {
            super.onMessage(bytes);
        }
        @Override
        public void onClosing(int code, String reason) {
            super.onClosing(code, reason);
        }
        @Override
        public void onClosed(int code, String reason) {
            super.onClosed(code, reason);
        }
        @Override
        public void onFailure(Throwable t, Response response) {
            super.onFailure(t, response);
        }
}
三、已集成的依赖
    //Okhttp3依赖
    compile 'com.squareup.okhttp3:okhttp:3.10.0'
    //Gson依赖
    compile 'com.google.code.gson:gson:2.8.0'
    //动态申请权限
    compile 'com.yanzhenjie:permission:2.0.0-rc12'
    //本地组件间通讯EventBus
    compile 'org.greenrobot:eventbus:3.0.0'
    //本地数据库
    compile 'org.greenrobot:greendao:3.2.0'
    //Glide图片加载
    compile 'com.github.bumptech.glide:glide:4.5.0'
    //上拉加载下拉刷新
compile 'com.scwang.smartrefresh:SmartRefreshLayout:1.1.0-alpha-14'
四、Dialog
（一）时间选择器DatePickDialogAfter(yyyy-MM-dd)
        DatePickDialogAfter dialog = new DatePickDialogAfter(this, time);
        dialog.dateTimePicKDialog(view);
        time是格式为yyyy-MM-dd的时间 view为显示日期的控件
（二）时间选择器DateTimePickDialogAfter(yyyy-MM-dd HH:mm)
        DateTimePickDialogAfter dialog = new DateTimePickDialogAfter(this, time);
        dialog1.dateTimePicKDialog(view);
        ime是格式为yyyy-MM-dd HH:mm的时间
五、Utils
（一）PictureUtil文件工具类 
1)compressImage质量压缩
    Bitmap new_bitmap = PictureUtil.compressImage(old_bitmap);
2)getimage图片先按比例大小压缩
Bitmap bitmap = PictureUtil.getimage(filePath);
3)getExtensionName获取文件扩展名
    String name = PictureUtil.getExtensionName(filename);
（二）TimeUtils 时间工具类
1)stampToDate毫秒转换成日期格式
TimeUtils.stampToDate(time,"HH:mm:ss");
Time为时间戳(毫秒)
2)dateToStamp日期格式转换成毫秒
    TimeUtils.dateToStamp("yyyy-MM-dd","2018-11-08");
（三）Validator 校验工具类
1)校验手机号 Validator.isMobile() 校验规则("^1\\d{10}$")
2)校验邮箱 Validator.isEmail() 
3)校验汉字Validator.isChinese() 
4)校验身份证 Validator.isIDCard()
5)校验邮编 Validator.isPostCode()
6)校验座机 Validator.isZuoJi()
7)校验银行卡号 Validator.isBankNum()
