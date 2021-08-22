---
title: Android DownloadManager
tags: Android
---

DownloadManager를 이용해 HTTP파일을 다운로드 받는 방법  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### DownloadManager?  

**앱에 저장될 위치의 URI와 다운받을 HTTP URI를 DownloadManger에게 알려주면 알아서 다운로드해준다.**  
**DownloadManager는 백그라운드에서 다운로드를 받기 때문에 앱에서 쓰레드를 생성할 필요가 없다.**  
**노티피케이션에 상태를 보여주므로 개발자가 따로 노티피케이션을 구성하지 않아도 된다.**  
**불안정한 네트워크 상태에 대한 예외처리가 되어있다.**  


### UI 및 권한  

**UI**  
![그림1](/assets/Android/DownloadManager/1.jpg){: width="50% height="50%"}  
uri를 입력하여 다운로드 버튼을 클릭하면 다운로드를 시작한다.  

**권한**  
![그림2](/assets/Android/DownloadManager/2.PNG)  
안드로이드 앱에서 인터넷에 접속할 수 있도록 하는 권한과 storage권한.    
**usesCleartextTraffic을 true로 설정 -> 안전하지 않은 URL을 실행가능**  

### 예제 소스코드  
```java
/* Android 7.0이상 지원 */
package com.example.downloadmanager_ex;

import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AppCompatActivity;

import android.app.DownloadManager;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.database.Cursor;
import android.net.Uri;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import java.io.File;

public class MainActivity extends AppCompatActivity {
    String DOWNLOADURL = null;

    Button button;
    EditText textbox;

    // JAVA에서 long형 변수에 int의 범위를 벗어나는 수를 저장할 때 접미사 L을 붙여야함
    private long downloadId = -1L;
    private DownloadManager downloadManager;

    /*
    API level9
    이 API를 통해 다운로드를 요청하는 앱 ACTION_NOTIFICATION_CLICKED은 사용자가 알림 또는 다운로드 UI에서
    실행중인 다운로드를 클릭 할 때 적절하게 처리 할 브로드 캐스트 수신기를 등록해야한다.
     */
    private BroadcastReceiver broadcastReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            long id = intent.getLongExtra(DownloadManager.EXTRA_DOWNLOAD_ID, -1);
            if (DownloadManager.ACTION_DOWNLOAD_COMPLETE.equals(intent.getAction())) {
                if (downloadId == id) {
                    DownloadManager.Query query = new DownloadManager.Query();
                    query.setFilterById(id);
                    Cursor cursor = downloadManager.query(query);
                    if (!cursor.moveToFirst()) {
                        return;
                    }

                    int columnIndex = cursor.getColumnIndex(DownloadManager.COLUMN_STATUS);
                    int status = cursor.getInt(columnIndex);

                    // 다운로드 성공 및 실패시 토스트메시지 출력
                    if (status == DownloadManager.STATUS_SUCCESSFUL) {
                        Toast.makeText(MainActivity.this, "Download succeeded", Toast.LENGTH_SHORT).show();
                    } else if (status == DownloadManager.STATUS_FAILED) {
                        Toast.makeText(MainActivity.this, "Download failed", Toast.LENGTH_SHORT).show();
                    }
                }
            } else if (DownloadManager.ACTION_NOTIFICATION_CLICKED.equals(intent.getAction())) {
                Toast.makeText(context, "Notification clicked", Toast.LENGTH_SHORT).show();
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        button = findViewById(R.id.button);
        textbox = findViewById(R.id.text);

        downloadManager = (DownloadManager) getSystemService(DOWNLOAD_SERVICE);

        // BroadcastReceiver 동적 등록
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction(DownloadManager.ACTION_DOWNLOAD_COMPLETE);
        intentFilter.addAction(DownloadManager.ACTION_NOTIFICATION_CLICKED);
        registerReceiver(broadcastReceiver, intentFilter);

        button.setOnClickListener(new View.OnClickListener() {
            @RequiresApi(api = Build.VERSION_CODES.N)
            @Override
            public void onClick(View view) {
                try {
                    DOWNLOADURL = textbox.getText().toString();
                    download();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        downloadManager.remove(downloadId);
    }

    @RequiresApi(api = Build.VERSION_CODES.N)
    // Android 7.0 버전체크,
    // .setRequiresCharging에서 요구
    private void download() throws InterruptedException {
        File file = new File(getExternalFilesDir(null), "download.mp4");
        Uri downloadUri = Uri.parse(DOWNLOADURL);

        DownloadManager.Request request = new DownloadManager.Request(downloadUri)
                // 노티피케이션에 보이는 타이틀
                .setTitle("Downloading a video")
                // 노티피케이션에 보이는 디스크립션
                .setDescription("Downloading Dev Summit")
                // VISIBILITY_VISIBLE로 설정시 노티피케이션에 보여짐
                .setNotificationVisibility(DownloadManager.Request.VISIBILITY_VISIBLE)
                // 파일이 저장될 위치의 URI
                .setDestinationUri(Uri.fromFile(file))
                // True로 설정 시 단말기가 충전중일 떄만 다운로드
                .setRequiresCharging(false)
                // True로 설정 시 모바일네트워크가 연결되었을 때도 다운로드
                .setAllowedOverMetered(true)
                // True로 설정 시 로밍네트워크가 연결되었을 때도 다운로드
                .setAllowedOverRoaming(true);

        downloadId = downloadManager.enqueue(request);

    }

    ;
}
```

**DownloadManager는 다운로드 요청이 완료되면 브로드캐스트로 결과를 전달하기 때문에 브로드캐스트 리시버를 구현해야 한다.**  

### 참고자료  
[developers](https://developer.android.com/reference/android/app/DownloadManager#COLUMN_LOCAL_URI)  
[codechacha_github](https://github.com/codechacha/DownloadManager)  
[codechacha_blog](https://codechacha.com/ko/android-downloadmanager/)  