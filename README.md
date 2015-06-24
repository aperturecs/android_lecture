# Android #1
객체지향 소재가 다 떨어진 관계로^^ 는 두번째 이유고... 객체지향이나 설계에 대한 더 깊은 사항을 파보기 전에, 실제 객체지향적 코드를 직접 만져보고 사용해가면서 직접 이론 및 설계의 필요성과 "아하! 이게 이거였구나!" 하는 걸 느끼고, 어차피 2학기때 OpenCV 프로젝트를 한다면 안드로이드가 필수적이기에 그냥 안드로이드 강의를 해보기로 했습니다. 나름 오래 판 전문분야라 여유롭게 할 수 있을듯 합니다.

#### Introduction
안드로이드의 역사는 2005년 7월에 Google사가 Android라는 작은 회사를 인수한 것으로부터 시작된다. 이 이후, 구글은 이 조그마한 "모바일 기기를 위한 OS"라는 프로젝트를 발전시켜서, 48개 제조사를 모아서 *OHA (Open Handset Alience)*를 구성한 뒤, 모바일 기기를 위한 공개 OS로서 무료 공개하겠다고 선언했다.

Android는 Java 언어로 작성되었고, Java로 앱을 개발할 수 있으며, 이를 위해 **Linux Kernel** (리눅스 커널)을 기반으로, 위에 자체개발한 **Dalvik VM** (Dalvik 가상 머신)을 탑재해서 가상 머신 위에서 자바 코드를 돌릴 수 있게 설계되었다.

#### Let's create some Android App!
###### 1. 프로젝트 생성
안드로이드 스튜디오 열고 New Project 누릅시다.  
 ㅡ  Application Name: 어플 이름 입력  
 ㅡ  Company Name : 회사 주소 (ex: thedeblur.com)을 입력하는건데 이걸 왜 하냐면...  
 ㅡ  **Package Name** : 어플리케이션의 식별자 (Identifier). com.thedeblur.appname 과 같은 방식으로 주로 쓰임.

그 다음은 어떤 디바이스에서 실행될지 물어보는데 스마트폰 & 태블릿, TV, 시계, 구글 글래스등 많이 물어본다. 스마트폰 & 태블릿만 체크한다.  
 - Minimum SDK : 이 앱이 실행될 수 있는 최소의 Android 버전. 

그 다음에 **반드시** "Blank Activity"로 되어있는거 "Add No Activity"로 바꾸고 넘어가자.  
Gradle Build라는 창이 뜨는데, 이건 Gradle이란 요망한 놈이 우리의 앱을 돌리기 위한 의존 모듈을 다운로드받는 과정이다.

###### 2. 프로젝트 톺아보기
우리가 만든 앱의 프로젝트 디렉터리 구조를 보자.  
![](file:/Users/vista/Desktop/스크린샷/스크린샷 2015-06-24 오후 8.28.05.png) 

 - app : 휴대전화용 앱 모듈. 
   - 인텔리제이에선 Project라는 큰 개념과 안에 Module이라는 개념이 있는데,
     - Visual Studio에 비유하면 Project -> Solution, Module -> Project
     - Eclipse에 비유하면 Project -> Workspace, Module -> Project이다.
   - 휴대전화용 앱이여서 휴대전화 아이콘. 다른 기종 추가시 app 이외에도 tv, wear, glass 등의 모듈이 추가됨.
 - app/build : 빌드된 결과물이 나오는 디렉터리
 - app/libs : 라이브러리 (.jar) 파일을 넣는 곳
 - app/src/main : 소스 코드 디렉터리
 - app/src/main/java : 자바 소스코드
 - app/src/main/res : 리소스
   - 리소스에는 이미지, 문자열, 레이아웃, Drawable, Style 등등이 있다. 나중에 설명!
 - app/src/main/AndroidManifest.xml : 해당 앱의 정보를 저장한다.
   - 앱 이름, 버전 정보, 아이콘 정보, 사용 권한, 액티비티 목록 등...

###### 3. 안녕, 세계!

src/main/res 폴더로 들어가서, layout이란 폴더를 생성한다.  
layout 안에 `activity_main.xml`을 추가하고 이렇게 적는다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello, world!" />

</LinearLayout>
```

그 다음, src/main/<패키지이름>으로 들어가서 `MainActivity`라는 클래스를 만든다. 내용은 이렇게 추가하자.

```java
package <패키지명>;

import android.app.Activity;
import android.os.Bundle;

public class MainActivity extends Activity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

마지막으로, AndroidManifest.xml을 수정하자. `<application>` 태그 안에 다음 내용을 추가한다.

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

자, 이제 요 요망한 로봇놈 옆에 X 표시가 사라진걸 볼 수 있는가? 앱을 빌드해서 폰에 올릴 준비가 됐다!

![](file:/Users/vista/Desktop/스크린샷/스크린샷 2015-06-24 오후 8.58.13.png)

#### 우리가 짠 코드 자세히 파헤쳐보기

**Activity** : 액티비티는 안드로이드의 컴포넌트 (구성 요소) 중 하나로, *사용자와 상호작용할 수 있는 화면*을 의미한다.  

 - 액티비티는 앱마다 하나 이상 존재할 수 있다. 그냥 우리가 다른 앱 쓸때 보는 모든 화면 하나하나가 액티비티다!  
 - 앱마다 하나씩의 메인 액티비티가 있다.
 - 각각의 액티비티에는 뭔가를 표시할 수 있는 창(Window)이 주어진다.
 - 액티비티는 화면 크기에 딱 맞을수도, 더 작을수도 있다.
 - 액티비티는 특정한 작업을 실행하기 위해서 다른 액티비티를 실행할 수 있다. 실행하게 되면, 기존 액티비티는 정지되고, 프레임워크가 그걸 Back Stack이란 곳에 쌓는데, (액티비티를 새로 실행할때도) 액티비티를 종료하면 그 액티비티는 POP되서 나오고 그 밑에 있는 액티비티가 Resume되게 되어 다시 화면에 보여진다. 
 
액티비티를 만들기 위해선 `android.app.Activity`를 상속받은 클래스를 만들면 된다.

```java
package <패키지명>;

import android.app.Activity;
import android.os.Bundle;

public class MainActivity extends Activity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```


######

기존이 코드의 특정 부분부터 시작해 쭉 읽어 내려가는 절차 지향적 프로그램이였다면, 이제부터는 **이벤트 기반 프로그래밍**이다. 즉, 시작점이 존재하는 것이 아닌, 특정 이벤트가 발생하면 그에 따른 코드가 실행되는 것이다. 이를 핸들러 함수 (Handler Function)이라고 하고, onSomeEvent와 같은 이름을 가진다. (ex: onClick, onTouch, onStart, onCreate...)

###### 액티비티의 생명주기 (Lifecycle)

위에서 액티비티는 시작되고 중지되고 재개되고 종료된다고 설명했다. 이렇듯이 액티비티에는 다음과 같은 생명 주기가 존재한다.

![](http://cfile3.uf.tistory.com/image/193517384F540BA41890C0)

그리고, 기존의 콘솔 프로그램을 main() 함수로부터 짰던 것과는 이제부터 차이를 보일 것이다.




앱에 있는 모든 액티비티들은 다 AndroidManifest.xml에 다음과 같이 정의해야 한다.

```xml
<activity android:name="com.vista.myapp.MainActivity" />
```

#### System Structure

![Android 구조](file:/Users/vista/Desktop/스크린샷/스크린샷 2015-06-24 오후 7.29.59.png)

<strike>뭔 개소리야</strike> 쉽게 해석해보자.

- Linux Kernel : 가장 저수준에 위치하며, 하드웨어 드라이버, 전원 관리, 프로세스, 멀티태스킹 등의 기초적인 OS의 동작을 담당한다.
  - 사실 Linux와 Android는 다른 프로젝트이므로 Linux 부분까지 안드로이드라고 치진 않는다.
  - 위에 있는 모든 안드로이드 컴포넌트들 및 우리가 실행시키는 앱들은 다 **리눅스 OS의 프로세스로서 리눅스 위에서 실행된다.**
  - 리눅스 커널을 그대로 가져온 덕분에, 기존에 존재하던 리눅스용 드라이버나 라이브러리(OpenGL, OpenCV 등...) 등을 가져올 수 있게 되었다.
- Dalvik VM : **달빅 가상머신이라고 부르며, Java 코드를 실행한다.**
  - 자바의 신조 - Write Once, Run Everywhere.
  - 근데, 만약 자바로 작성했는데 기계어로 컴파일되면? 이 코드는 Mac에선 안되고 AMD CPU에선 안되고... 말썽...
  - 그렇다고 Python처럼 인터프리터로 돌리면 성능이 망하죠
  - 그래서 자바는 컴파일 시 기계어가 아닌 **Java IR이라는 중간 언어**로 컴파일해두고,
  - 각각의 플랫폼에서 각각의 플랫폼별로 개발된 자바 가상 머신 (JVM)을 이용해서 JVM이란 가상기계가 IR을 실행하는 식으로 프로그램을 돌립니다.
  - 근데 자바는 누구꺼? **Oracle꺼!**
  - 맘대로 못씀! *유료에요!*
  - 그래서 Android에서는 Oracle(Sun)의 Hotspot VM을 버리고 Dalvik VM이란 JVM을 만듬
  - 이것때문에 구글이랑 오라클이랑 법적으로 싸웁니다...
- Framework : Java로 코딩되어 있으며, 실질적인 안드로이드이다. 
  - 실질적인 액티비티 관리, 뷰 관리, 앱 관리 등을 제공한다.
  - 안드로이드 애플리케이션을 돌리기 위한 API를 제공한다.
  - Android의 기본 UI 요소들 (버튼, 타이틀바, 팝업창, 토스트 등...)이 구현되어 있다.
- App : 우리가 돌리는 **앱, 앱, 앱!**
  - System App : 시스템 기본 앱. 설정 앱, 상태바 및 알림창 앱 (SystemUI), 홈런쳐 앱 등등...
  - User App : 우리가 마켓에서 깔거나 설치하는 앱들
  
