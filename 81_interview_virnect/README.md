# 버넥트 안드로이드 면접 질문

- 안드로이드
  - 4대 컴포넌트, 그리고 각자 설명
  - 인텐트 설명 - 4대 컴포넌트에서 어떻게 사용했는가
  - 매니페스트에서 launchMode가 무엇인가 - 프래그먼트를 싱글톤으로 사용해야하는 이유
  - 액티비티 라이프사이클 - 스테이트 복원 등도 설명
  - 핸들러와 루퍼에 대해 설명하시오
  - 메인스레드와 워커스레드
  - context를 설명하시오
- 자바
  - 앱스트랙트와 인터페이스의 차이, 어떻게 사용했는가
  - Synchronize에 대해 설명하시오.
- 코틀린
  - 주로 코틀린을 사용하는지
  - 코틀린의 장단점
  - 코틀린과 자바를 동시에 사용해봤는지. 어떻게 사용했는지
  - lateinit과 lazy
  - apply, with, let, also, run
  - ?, !!의 차이
  - 앨비스 연산자


## 안드로이드

### 4대 컴포넌트, 그리고 각자 설명

##### 액티비티

액티비티는 UI 화면을 담당하는 컴포넌트입니다. 액티비티가 기본적으로 가지고 있는 생명주기 메소드를 재정의하여 원하는 기능을 구현하는 방식으로 제작합니다. 가장 많이 쓰이는 컴포넌트 이기때문에 굉장히 중요하다고 볼 수 있습니다. **사용자와 상호작용을** 담당하는 인터페이스라고 할 수 있습니다.

Activity(액티비티)의 특징

안드로이드 어플리케이션은 반드시 하나 이상의 Activity를 가지고 있어야 합니다.
두 개의 액티비티를 동시에 Display할 수 없습니다.
인텐트(Intent)를 통해 다른 애플리케이션의 액티비티를 호출할 수 있습니다.
액티비티 내에는 프래그먼트(Fragment)를 추가하여 화면을 분할시킬 수 있습니다.

##### Service(서비스)

서비스는 백그라운드에서 실행되는 프로세스를 의미합니다. 서비스는 화면이 존재하지 않습니다. 하지만 서비스도 애플리케이션의 구성요소이므로 새로 만든 후에는 항상 매니페스트에 등록을 해주어야 합니다. 메인 액티비티에서 서비스를 시작하고 싶은 경우에는 startService()라는 메서드를 이용해 서비스를 실행시킬 수 있습니다.

Service(서비스)의 특징

1. 화면이 없습니다. 그저 백그라운드에서 돌아가는 컴포넌트입니다.
2. 액티비티와 서비스는 UI스레드라고 불리는 동일한 애플리케이션 스레드로 실행됩니다.
3. 애플리케이션이 종료되어도 이미 시작이 된 서비스(Service)는 백그라운드(Background)에서 계속 동작합니다.
4. 네트워크를 통해서 데이터를 가져올 수 있습니다.

##### Content Provider(콘텐트 제공자)

콘텐트 제공자는 데이터를 관리하고 다른 어플리케이션 데이터를 제공해주는 컴포넌트입니다. 특정한 애플리케이션이 사용하고 있는 데이터베이스(DB)를 공유하기 위해 사용하며 애플리케이션 간의 데이터 공유를 위해 표준화된 인터페이스를 제공합니다. 데이터베이스의 데이터를 전달할때 많이 사용합니다. 콘텐트 제공자는 생명주기를 가지고 있지 않습니다.

Content Provider(콘텐트 제공자)의 특징

1. 파일입출력, SQLiteDB, Web등을 통해서 데이터를 관리합니다.
2. 콘텐트 제공자를 통하여 다른 어플리케이션의 데이터도 변경할 수 있습니다.
3. 외부 애플리케이션이 현재 실행 중인 애플리케이션 내에 있는 데이터베이스(DB)에 함부로 접근하지 못하게 할 수 있으면서 나 자신이 공개하고 공유하고 싶은 데이터만 공유할 수 있도록 도와줍니다.
4. 작은 데이터들은 인텐트(Intent)로 애플리케이션끼리 데이터를 서로 공유가 가능하지만 콘텐 프로바이더는 음악 또는 사진 파일 등과 같이 용량이 큰 데이터들을 공유하는데 적합합니다.
5. 프로바이더는 데이터의 Read(읽기), Write(쓰기)에 대한 퍼미션이 있어야 애플리케이션에 접근이 가능합니다.

##### Broadcast Recevier(방송 수신자)

방송 수신자(BrodCase Receiver)는 안드로이드 OS로부터 발생하는 각종 이벤트와 정보를 받아와 핸들링하는 컴포넌트입니다. 브로드캐스팅은 메시지를 여러 객체에게 전달하는 방법을 의미하는데 이렇게 전달되는 브로드캐스팅 메시지를 방송수신자라는 어플리케이션의 구성요소를 이용해 받을 수 있습니다. 사용자 안드로이드 디바이스의 시스템 부팅시 앱 초기화, 네트워크 끊김 등등 특수한 이벤트에 대한 처리나 배터리 부족 알림 ,문자 수신과 같은 정보를 받아 처리를 해야 할 필요가 있을 때 동작합니다.

즉, 안드로이드 OS에서 메신저앱 또는 문자 메시지가 오면 모든 앱에 "메시지가 왔다"라는 하나의 정보를 방송(BrodCast)을 합니다.

Broadcast Recevier(방송수신자)의 특징

1. 디바이스에서 발생하는 일 중에서 어플리케이션이 알아야 하는 상황이 발생하면 알려줍니다.
2. 수신기를 통해 디바이스의 상황을 감지하고 적절한 작업을 수행합니다.
3. 대부분 UI가 존재하지 않습니다.

<br>

### 인텐트 설명 - 4대 컴포넌트에서 어떻게 사용했는가
Intent는 메시징 객체로, 다른 앱 구성 요소로부터 작업을 요청하는 데 사용할 수 있습니다. 앱 구성 요소는 Android 앱의 필수적인 기본 구성 요소입니다. 각 구성 요소는 시스템이나 사용자가 앱에 들어올 수 있는 진입점입니다. 다른 구성 요소에 종속되는 구성 요소도 있습니다. 앱 구성 요소에는 네 가지 유형이 있습니다. (액티비티, 서비스 , 브로드 캐스트, 콘텐츠 제공자)

인텐트가 구성 요소 사이의 통신을 촉진하는 데는 여러가지 방식이 있지만 기본적으로 액티비티와 서비스 시작, 브로드캐스트 전달에 사용됩니다.

- 액티비티 시작
  - Activity의 새 인스턴스를 시작하려면 **Intent를 `startActivity()`로 전달하면** 됩니다. Intent는 시작할 액티비티를 설명하고 모든 필수 데이터를 담습니다.
  - 액티비티가 완료되었을 때 결과를 수신하려면, startActivityForResult()를 호출합니다. 액티비티는 해당 결과를 이 액티비티의 onActivityResult() 콜백에서 별도의 Intent 객체로 수신합니다.
- 서비스 시작
  - 사용자 인터페이스(UI) 없이 백그라운드에서 작업을 수행하는 구성 요소입니다. Android 5.0(API 레벨 21) 이상부터는 JobScheduler로 서비스를 시작할 수 있습니다.
  - Android 5.0(API 레벨 21) 이하 버전은 Service 클래스의 메서드를 사용하면 서비스를 시작할 수 있습니다. 서비스를 시작하여 일회성 작업을 수행하도록 하려면(예: 파일 다운로드) **Intent를 `startService()`에 전달하면** 됩니다. Intent는 시작할 서비스를 설명하고 모든 필수 데이터를 담고 있습니다.
  - 서비스가 클라이언트-서버 인터페이스로 디자인된 경우, **다른 구성 요소로부터 서비스에 바인딩하려면 Intent를 `bindService()`에 전달하면** 됩니다.
- 브로드캐스트 전달
  - 브로드캐스트는 모든 앱이 수신할 수 있는 메시지입니다. 시스템은 시스템이 부팅될 때 또는 기기가 충전을 시작할 때 등 시스템 이벤트에 대한 다양한 브로드캐스트를 전달합니다.
  - **Intent를 `sendBroadcast()` 또는 `sendOrderedBroadcast()`에 전달하면** 다른 앱에 브로드캐스트를 전달할 수 있습니다.
- 콘텐트 프로바이더 - [인텐트를 통한 데이터 액세스](https://developer.android.com/guide/topics/providers/content-provider-basics?hl=ko#Intents)
  - 인텐트는 콘텐츠 제공자에 간접 액세스를 제공할 수 있습니다. 애플리케이션에 액세스 권한이 없는데도 사용자에게 제공자 내의 데이터에 액세스 권한을 허가하려면, 권한을 가지고 있는 애플리케이션에서 결과 인텐트를 다시 가져오거나 권한이 있는 애플리케이션을 활성화한 다음 사용자에게 그 애플리케이션에서 작업하도록 하면 됩니다.
  - [임시 권한으로 액세스하기](https://developer.android.com/guide/topics/providers/content-provider-basics?hl=ko#heading)
  - [다른 애플리케이션 사용](https://developer.android.com/guide/topics/providers/content-provider-basics?hl=ko#heading_1)
  - [도우미 앱을 사용한 데이터 표시](https://developer.android.com/guide/topics/providers/content-provider-basics?hl=ko#heading_2) (캘린더 프로바이더, 연락처 프로바이더 )

<br>

### 매니페스트에서 launchMode가 무엇인가

#### 부제 : 프래그먼트를 싱글톤으로 사용해야하는 이유

액티비티를 시작하는 방법에 대한 지침입니다. 인텐트를 처리하기 위해 액티비티를 호출할 때 발생하는 것을 결정하기 위해 Intent 객체에서 액티비티 플래그(FLAG_ACTIVITY_* 상수)와 함께 작동하는 4개의 모드가 있습니다.

- standard (Default)
  - 별도로 launchMode를 지정하지 않으면 standard로 지정이 된다.
  - 액티비티를 호출하는 것 마다 계속 스택에 쌓게 된다.
- singleTop
  - singleTop으로 지정된 액티비티가 존재하고 이 액티비티의 인스턴스가 이미 스택에 존재하고 있을 때
  - 해당 액티비티로 또 호출하면 상위 액티비티의 onNewIntent()가 수행되면서 기존 인스턴스를 재활용하게 된다.
  - 예시
    - 스택 : A -> B -> A
    - A를 또 호출 : A -> B -> A(재활용)
- singleTask
  - 재활용을 한다. 그런데 singleTop과 헷갈리면 안된다.
  - 태스크의 루트에만 존재할 수 있다.
  - 그럴려면 방법은 하나, 해당 액티비티는 새로운 태스크를 만듦과 동시에 본인의 인스턴스를 생성한다.
  - 이 액티비티의 인스턴스가 이미 존재한다면 다른 액티비티에서 이 액티비티를 부를 때 해당 인스턴스를 재활용한다.
  - 예시
    - 스택 : A -> B
    - singleTask로 지정된 C 호출 : A -> B // C (A, B가 담긴 태스크와 별도의 태스크)
    - 일반 D 호출 : A -> B // C -> D
- singleInstance
  - 간단하다. 하나의 태스크에 오직 하나의 인스턴스만 들어간다.
  - 태스크와 인스턴스가 매번 새로 호출된다.
  - 예시 : A // B // C

##### 다중 인스턴스 지원 여부

- 지원 : standard, singleTop ( singleTop 은 조건부 지원 )
- 지원 안함 : singleTask, singleInstance

<br>

### 액티비티 라이프사이클 - 스테이트 복원 등도 설명

##### 액티비티의 3가지 상태

1. 재개됨 (Resumed) : 액티비티가 화면 포그라운드에 있고 포커스를 갖습니다.
2. 일시정지됨 (Paused) : 액티비티가 일부가 가려진 상태, 이때는 액티비티가 완전히 살아 있지만(Activity 객체가 메모리에 보관되어 있고, 모든 상태 및 멤버 정보를 유지하며 창 관리자에 붙어 있는 상태로 유지), 메로리가 부족한 경우 시스템이 중단시킬 수도 있습니다.
3. 정지됨(Stopped) : 액티비티가 다른 액티비티에 완전히 가려진 상태, 이때는 액티비티가 완전히 살아 있지만(Activity 객체가 메모리에 보관되어 있고, 모든 상태 및 멤버 정보를 유지하며 창 관리자에 붙어 있는 상태로 유지), 메로리가 부족한 경우 시스템이 중단시킬 수도 있습니다.

![](https://github.com/conquerex/OneHundredMillionSalary/blob/master/81_interview_virnect/image_v_1.png?raw=true)


1. onCreate() : 액티비티를 생성, 액티비티의 필수 구성 요소를 초기화, setContentView()를 호출해서 액티비티에 UI를 정의할 수 있음
2. onStart() : 액티비티가 보여지기 시작함
3. onResume() : 액티비티가 시작되고 사용자와 상호작용하기 직전에 호출
4. onPause() : 액티비티가 부분적으로 가려짐(예 - 반투명 다이얼로그). 사용자가 액티비티를 떠난다는 신호, 현재 사용자가 세션을 넘어서 정보가 지속되어야 한다면 변경 사항을 커밋하기에 가장 적절한 콜백입니다. (사용자가 돌아오지 않을 수도 있음)
5. onStop() : 액티비티가 더 이상 사용자에게 표시되지 않으면 호출됩니다. (예 - 다른 액티비티 실행)
6. onRestart() : 액티비티가 중단되었다가 다시 시작되기 전에 호출됩니다.
7. onDestroy() : 액티비티가 소멸되기 직전에 호출됩니다. 액티비티가 받는 마지막 호출입니다.


##### 액티비티의 수명

- Whole Lifecycle : onCreate() ~ onDestroy(), 액티비티 전체 생명 주기
- Visible Lifecycle : onStart() ~ onStop(), 가시적으로 보이는 액티비티 생명 주기
- Foreground Lifecycle : onResume() ~ onPause(), 포그라운드에 떠있는 액티비티 생명 주기


##### 액티비티 상태 저장

- 저장 생명 주기 : onPause() -> onSaveInstanceState() -> onStop() -> onDestroy()
- 복구 생명 주기 : onCreate() -> onStart() -> onRestoreInstanceState() -> onResume()


> 추천 링크
- https://jungwoon.github.io/android/2019/07/15/Activity/
- https://unikys.tistory.com/276

<br>

### 핸들러와 루퍼에 대해 설명하시오

다른 스레드에서 메인 스레드로 접근하기 위해 Looper와 Handler를 사용할 수 있으며, 안드로이드는 Java의 Thread를 좀 더 쉽게 사용할 수 있도록 래핑한 HandlerThread, 더 나아가 Thread나 Message Loop 등의 작동 원리를 크게 고려하지 않고도 사용이 가능한 AsyncTask 등의 클래스를 제공합니다.

왜 안드로이드는 메인 스레드에서만 UI 작업이 가능하도록 제한할까요? 메인 스레드가 아닌 스레드가 병렬적으로 실행되고 있을 때, 메인 스레드와 다른 스레드, 두 개 이상의 스레드가 동시에 같은 텍스트뷰에 setText()를 시도하는 경우를 생각하면 간단합니다. 둘 중 어느 스레드의 setText()가 적용될지 예측할 수 없고, 사용자는 둘 중 하나의 값만을 볼 수 있어 다른 한 스레드의 결과는 버려집니다. 이같이 **두 개 이상의 스레드를 사용할 때의 동기화 이슈를 차단하기 위해서 `Looper`와 `Handler`를 사용하게** 됩니다.

메인 스레드는 내부적으로 Looper를 가지며 그 안에는 Message Queue가 포함됩니다. Message Queue는 스레드가 다른 스레드나 혹은 자기 자신으로부터 전달받은 Message를 기본적으로 선입선출 형식으로 보관하는 Queue입니다. **`Looper`는 Message Queue에서 Message나 Runnable 객체를 차례로 꺼내 Handler가 처리하도록 전달**합니다. **`Handler`는 Looper로부터 받은 Message를 실행, 처리하거나 다른 스레드로부터 메시지를 받아서 Message Queue에 넣는 역할을 하는 스레드 간의 통신 장치입니다.**

![](https://github.com/conquerex/OneHundredMillionSalary/blob/master/81_interview_virnect/image_v_2.png?raw=true)

##### Handler

Handler는 스레드의 Message Queue와 연계하여 Message나 Runnable 객체를 받거나 처리하여 스레드 간의 통신을 할 수 있도록 합니다. 외부, 혹은 자기 스레드로부터 받은 메시지를 어떤 식으로 처리할 지는 handleMessage() 메서드를 구현하여 정합니다. sendMessage()나 post()로 특정 Handler에게 메시지를 전달할 수 있고, 재귀적인 호출도 가능하므로 딜레이를 이용한 타이머나 스케줄링 역할도 할 수 있어 편리합니다.

##### Looper와 Message Queue

Looper는 무한히 루프를 돌며 자신이 속한 스레드의 Message Queue에 들어온 Message나 Runnable 객체를 차례로 꺼내서 이를 처리할 Handler에 전달하는 역할을 합니다. 메인 스레드는 Looper가 기본적으로 생성돼 있지만, 새로 생성한 스레드는 기본적으로 Looper를 가지고 있지 않고, 단지 run 메서드만 실행한 후 종료하기 때문에 메시지를 받을 수 없습니다. 따라서 기본 스레드에서 메시지를 전달받으려면 prepare() 메서드를 통해 Looper를 생성하고, loop() 메서드를 통해 Looper가 무한히 루프를 돌며 Message Queue에 쌓인 Message나 Runnable 객체를 꺼내 Handler에 전달하도록 합니다.

##### 추가

Message가 int나 Object같이 스레드 간 통신할 내용을 담는다면, Runnable은 실행할 run() 메서드와 그 내부에서 실행될 코드를 담는다는 차이점이 있습니다.

<br>

### 메인스레드와 워커스레드

Android의 단일 스레드 모델에는 단순히 두 가지 규칙이 있습니다.

1. UI 스레드를 차단하지 마세요.
2. UI 스레드 외부에서 Android UI 도구 키트에 액세스하지 마세요.

2번의 외부는 백그라운드 쓰레드가 될텐데, 그렇다면 백그라운드에서 UI 변경을 하고 싶다면 도구 키트를 사용하는게 아닌 UI쓰레드에 접근을 해야한다. 그 방법도 개발문서에 적혀있다.

1. Activity.runOnUiThread(Runnable)
2. View.post(Runnable)
3. View.postDelayed(Runnable, long)

##### 메인스레드

- 안드로이드 이벤트 생성 및 처리를 담당
- 여러 이벤트를 그와 관련된 위젯으로 연동시키는 역할
- 다양한 뷰와 위젯을 표현하는 역할과 사용자로 하여금 그것들을 사용할 수 있게 해준다.
- 단일 스레드 모델 때문에, UI 스레드를 블록하지 않음으로써, 어플리케이션의 UI의 반응성을 최상으로 유지해야 한다.
- UI 스레드에서 모든 작업을 처리하는 모델의 단점은 네트워크 접속 또는 데이터베이스 쿼리와 같이 오래 걸리는 작업을 하는 동안 UI 관련된 작업은 처리되지 못한다.
- 오랜 시간 동안에 UI 관련 작업이 처리되지 못하면 ANR(applicatio not responding)이라는 에러가 발생하게 된다.

##### 워커스레드

- 여타 비동기 작업과 마찬가지로 UI 작업을 비동기적으로 처리한다면 반드시 동기화 문제에 마주치게 된다. 안드로이드는 이러한 문제 발생을 예방하기 위해서 병렬로 동작하는 메인 스레드와 워커 스레드 사이에 핸들러를 두고 UI 작업은 모두 메인 스레드로 전달하도록 한 것이다.
- 이런 문제를 해결하기 위해서 워커 스레드는 핸들러를 사용한다. 핸들러의 역할을 간단히 말하면 메시지를 전달하는 것이다. 워커 스레드에서 핸들러는 메인 스레드로 메시지를 전달한다. 그러면 메시지를 수신한 메인 스레드에서 적절한 작업을 처리하는 것이다.
- 메인 쓰레드는 쓰레드 세이프 하지 않습니다. 따라서 메인 쓰레드가 아닌 워커 쓰레드에서 UI를 조작하면 정상적으로 반영이 되지 않을 수 있고 비정상적인 동작을 초래할 수 있습니다.

> 참고 사이트
- http://anitoy.pe.kr/android-process-thread-concept/

<br>

### context를 설명하시오

Context  는 크게 두 가지 역할을 수행하는 Abstract 클래스 입니다.

- 어플리케이션에 관하여 시스템이 관리하고 있는 정보에 접근하기
- 안드로이드 시스템 서비스에서 제공하는 API 를 호출 할 수 있는 기능

##### 애플리케이션 컨텍스트(Application Context)

- 애플리케이션의 라이프사이클과 연결, getApplicationContext()
- 현재의 컨텍스트와 분리된 라이프사이클을 가진 컨텍스트가 필요할 때나 액티비티의 범위를 넘어서 컨텍스트를 전달할 떄에 사용
- 애플리케이션과 관련되어 있고 애플리케이션이 살아있는 동안 변경되지 않습니다. 그러므로 싱글톤
- 그 어떤 컨텍스트(Context)보다 오래 유지되는 컨텍스트(Context)가 필요할때에만 getApplicationContext()를 사용 (메모리 누수, GC가 액티비티를 대상으로한 수집을 못하므로)

##### 액티비티 컨텍스트(Activity Context)

- 액티비티에서 사용 가능하며 이 컨텍스트는 액티비티의 라이프사이클과 연결되어 있습니다.
- 컴포넌트의 라이프 사이클 안에서만 사용할 경우 Context를 쓰고 전역적으로 사용할 경우에는 ApplicationContext를 사용하자

> 참고 사이트
- https://shinjekim.github.io/android/2019/11/01/Android-context%EB%9E%80/
- https://black-jin0427.tistory.com/220
- https://www.charlezz.com/?p=1080
- https://shnoble.tistory.com/57

<br>

## 자바

### 앱스트랙트와 인터페이스의 차이, 어떻게 사용했는가

##### 공통점
- 추상클래스와 인터페이스는 선언만 있고 구현 내용이 없는 클래스다. (자바8부터 인터페이스에 default method 구현이 가능해졌지만 일반적으로 인터페이스는 구현이 없다.)
- 따라서 인터페이스와 추상클래스를 가지고 새로운 인스턴스(객체)를 생성할 수 없다.
- 추상클래스를 extends로 상속받아 구현한 자식클래스나 인터페이스를 implements 하고 구현한 자식클래스만이 객체를 생성할 수 있다.
- 결국 자식클래스가 무언가 반드시 구현하도록 위임해야할 때 사용해야 한다.
##### 추상클래스
- 추상클래스의 목적은 말 그대로 공통적인 기능을 하는 객체들의 추상화다.
- 기존의 클래스에서 공통된 부분을 추상화하여 상속하는 클래스에게 구현을 강제화
- 추상 클래스를 상속받은 경우는 일반 메소드가 아닌 추상 메소드만을 구현했다.
##### 인터페이스
- 인터페이스는 구현하는 모든 클래스에 대해 특정한 메서드가 반드시 존재하도록 강제하는 역할이다.
- 구현 객체의 같은 동작을 보장하기 위한 목적
- 클래스가 아니기 때문에 인터페이스는 다중 상속이 가능
- 인터페이스를 상속받은 경우는 모든 메소드를 구현

> 참고 사이트
- https://jeong-pro.tistory.com/82
- https://loustler.io/languages/oop_interface_and_abstract_class/
- https://mygumi.tistory.com/257
- https://cbw1030.tistory.com/47

<br>

### Synchronized에 대해 설명하시오.

synchronized 키워드를 사용하여 A 쓰레드의 작업이 끝날 때까지 대기해라!라고 명령을 내릴 수 있다. 더 나아가 synchronized 블럭 내에 있는 공유 자원을 점유하라!라고 이해를 하는 게 좀 더 정확하다.

CPU에서 명령을 수행하기 위해서는 메모리에 있는 데이터를 CPU로 가져와야한다. 하지만 메모리에 있는 데이터를 CPU로 가져오는 행위는 매우 느리므로 CPU는 캐시 메모리가 있다. (L1 캐시, L2 캐시 등등) 그리고 이 캐시 메모리를 쓰레드 로컬 변수라고 부른다.

하지만 쓰레드 로컬 변수이기 때문에 다른 쓰레드에서는 메모리에 접근을 해도 해당 쓰레드 변수의 값을 얻어올 수 없다.

여기서 synchronized 키워드가 가지는 진정한 의미가 나온다. synchronized 키워드를 사용한다는 것은 **해당 블럭 내에 있는 공유 자원 A’가 쓰레드 로컬 변수에서 램으로 써지기 까지 다른 쓰레드는 대기(block)하라라는 의미를 가진다. 쓰레드 로컬 변수가 램에 써진 순간 다른 쓰레드가 램에서 해당 값을 가져와서 작업할 수 있게** 된다.

<br>

## 코틀린

### 주로 코틀린을 사용하는지, 코틀린의 장단점

##### 장점

- 코틀린이 언어 수준에서 제공하는 Null Safety는 null을 처리하는 데 있어 안전한 방법을 제공합니다. 변수를 선언하는 시점부터, 그 변수의 특성을 고민하게 만들었다는 것. 컴파일 레벨에서 확인 가
- 자바와 호환
- 가변/불변 지원 (var, val, const-컴파일 레벨에 상수할당)
- 간결한 문법 - 세미콜론이 없다. new 키워드 없이 객체를 생성한다. 타입추론을 지원하므로 일반적인 타입을 적지 않는다.
- 람다 표현식 지원, 스트림 api를 지원

##### 단점

- 러닝 커브
- 자바만큼 레퍼런스가 많지 않다는 것 (당연하겠지만)
- 빌드 속도 (체감되지 않음)

> 참고 사이트
- https://soulduse.tistory.com/68
- https://wsym.tistory.com/entry/%EC%BD%94%ED%8B%80%EB%A6%B0-%ED%8A%B9%EC%A7%95-%EB%B0%8F-%EC%9E%A5%EB%8B%A8%EC%A0%90
- https://woowabros.github.io/experience/2017/07/18/introduction-to-kotlin-in-baeminfresh.html

<br>

### lateinit과 lazy

##### lateinit
- 필요할 때 초기화하고 사용할 수 있음. 초기화하지 않고 쓰면 Exception 발생
- lateinit은 꼭 변수를 부르기 전에 초기화 시켜야
- var(mutable)에서만 사용이 가능하다
- var이기 때문에 언제든 초기화를 변경할 수 있다.
- null을 통한 초기화를 할 수 없다.
- 초기화를 하기 전에는 변수에 접근할 수 없다
##### lazy
- 변수를 선언할 때 초기화 코드도 함께 정의. 변수가 사용될 때 초기화 코드 동작하여 변수가 초기화됨
- 호출 시점에 by lazy 정의에 의해서 초기화를 진행한다.
- val(immutable)에서만 사용이 가능하다.
- val이므로 값을 교체하는 건 불가능하다.
- 초기화를 위해서는 함수명이라도 한번 적어줘야 한다.
- lazy을 사용하는 경우 기본 Synchronized로 동작한다.

> 참고 사이트
- https://soulduse.tistory.com/68
- https://thdev.tech/kotlin/2018/03/25/Kotlin-lateinit-lazy/
- https://dnight.tistory.com/entry/lateinit-lazy-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90

<br>

### apply, with, let, also, run

![](https://github.com/conquerex/OneHundredMillionSalary/blob/master/81_interview_virnect/image_v_3.png?raw=true)

- apply
  - inline fun <T> T.apply(block: T.() -> Unit): T
  - 프로퍼티에 값을 할당할때 유용
- also
  - inline fun <T> T.also(block: (T) -> Unit): T
  - 자신을 호출한 객체를 사용하지 않거나, 값을 변경하지 않을 경우에 쓰이는 함수 (데이터를 할당하기 전에 유효성 검사)
- run
  - inline fun <T, R> T.run(block: T.() -> R): R
  - 함수를 호출한 객체를 이어지는 블록의 수신 객체로 전달
- let
  - inline fun <T, R> T.let(block: (T) -> R): R
  - null 검사를 하고 null 이 아닐때만 코드를 실행할때 유용합니다.
- with
  - inline fun <T, R> with(receiver: T, block: T.() -> R): R
  - 어떤 객체의 이름을 반복하지 않고 객체에 대해 다양한 연산을 수행하려고 할 때

> 참고 사이트
- https://soulduse.tistory.com/68
- https://jungwoon.github.io/kotlin/2019/07/24/Kotlin-apply-let-also-run/
- https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%9D%98-apply-with-let-also-run-%EC%9D%80-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-4a517292df29
https://black-jin0427.tistory.com/169

### 앨비스 연산자

코틀린은 null 대신 사용할 디폴트 값을 지정할 때 편리하게 사용할 수 있는 연산자

### ?, !!의 차이

### 코틀린과 자바를 동시에 사용해봤는지. 어떻게 사용했는지
