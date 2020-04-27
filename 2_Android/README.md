# Android 질문

기본적으로는 [신입 개발자 면접 질문 시리즈](https://www.notion.so/54d624628a634c879cc93d94f54cd2d1)에서 답변을 가지고 왔습니다.

<br>

### 번들과 인텐트의 차이를 설명하시오.
**Bundle**은 문자열로 된 키와 여러가지 타입의 값을 저장 하는 일종의 Map 클래스이다. HashMap 형태(Key,Value값을 put해서 보관)로 보관한다.  언제 보관할 수 있는가?

Shut down 후 다시 초기화 할 때. Shut down이 되는 상황은 다양한데 대표적으로 메모리 부족으로 Shut down되는 경우와 디바이스를 가로, 세로모드로 바꿀때 Shut down 된다. Shut down이 발생하면 데이터가 날라가기 때문에 이 시점에 Bundle에 값을 보관해야 한다.

protected void onSaveInstanceState(Bundle outState) 메서드를 override해서 처리해주면 된다. 여기서 값을 보관하고 protected void onRestoreInstanceState(Bundle savedInstanceState) 이 메서드를 override해서 get하여 값을 받아오면 된다.

**Intent**는 저장이 아닌 전달하는 수단으로의 객체이다. 인텐트 안에는 Bundle이 있다. 실제데이터는 인텐트안의 Bundle에 넣어지게 된다.

Intent, Bundle은 모두 Parcelable을 inherited한 것이므로 근본적으로 모두 공통된 기능을 수행할 수 있다.

<br>

### Intent, IntentFilter, PendingIntent를 설명하시오.

- 참고 사이트
  - https://woovictory.github.io/2019/01/02/Android-What-is-Intent/
  - https://developer.android.com/guide/components/intents-filters?hl=ko#ExampleExplicit

###### Intent
Intent는 메시징 객체로, 다른 앱 구성 요소로부터 작업을 요청하는 데 사용할 수 있습니다. 앱 구성 요소는 Android 앱의 필수적인 기본 구성 요소입니다. 각 구성 요소는 시스템이나 사용자가 앱에 들어올 수 있는 진입점입니다. 다른 구성 요소에 종속되는 구성 요소도 있습니다. 앱 구성 요소에는 네 가지 유형이 있습니다. (액티비티, 서비스 , 브로드 캐스트, 콘텐츠 제공자)

인텐트가 구성 요소 사이의 통신을 촉진하는 데는 여러가지 방식이 있지만 기본적으로 액티비티와 서비스 시작, 브로드캐스트 전달에 사용됩니다.

- 액티비티 시작
  - Intent를 startActivity()로 전달
- 서비스 시작
  - Android 5.0(API 레벨 21) 이상부터는 JobScheduler로 서비스를 시작할 수 있습니다.
  - JobScheduler에 대한 자세한 내용은  https://developer.android.com/reference/android/app/job/JobScheduler?hl=ko
- 브로드캐스트 전달
  - 브로드캐스트는 모든 앱이 수신할 수 있는 메시지입니다. 시스템은 시스템이 부팅될 때 또는 기기가 충전을 시작할 때 등 시스템 이벤트에 대한 다양한 브로드캐스트를 전달합니다.  Intent를 sendBroadcast() 또는 sendOrderedBroadcast()에 전달하면 다른 앱에 브로드캐스트를 전달할 수 있습니다.

인텐트에는 두가지 유형이 있습니다.

- 명시적 인텐트
  - 인텐트에 전달되는 클래스 객체 컴포넌트 이름을 정확히 지정하여 호출될 대상을 확실히 알 수 있는 경우. 예를들어 ,
  ```
  var intent = Intent(this@MainActivity, SecondActivity::class.java)
  ```

- 암시적 인텐트
  - 인텐트의 액션과 데이터를 지정하긴 했지만, 호출할 대상이 달라질 수 있는 경우에는 암시적인 인텐트를 사용한다. 설치된 애플리케이션들에 대한 정보를 알고있는 안드로이드 시스템이 인텐트를 이용해 요청한 정보를 처리할 수 있는 적절한 컴포넌트를 찾아본다. 그리고 나서 사용자에게 그 대상과 처리 결과를 보여주는 과정을 거친다.
  - 특정 컴포넌트에서 암시적 인텐트를 받기 위해서는 매니페스트 파일에서 요소와 함께 어플리케이션 컴포넌트 각각에 대해서 하나 이상의 IntentFilter를 선언해야한다. 각각의 컴포넌트는 action, data, category를 기반으로 해서 자신이 받길 원하는 인텐트의 유형을 명시해야한다.
  - 암시적 인텐트를 사용하는 대표적인 경우로 문서 편집기를 들 수 있다. 카카오톡으로 PDF 파일을 받아서 열기를 클릭하면 PDF를 편집하거나 보여줄 수 있는 많은 애플리케이션들이 자기가 그 PDF 파일을 열겠다고 손을 든다. 그러면 안드로이드 시스템에서는 PDF를 열 수 있는 애플리케이션을 선택하는 위젯을 띄워준다.

<br>

암시적 인텐트가 시스템을 통해 전달되어 다른 액티비티를 시작하는 방법.
1. 액티비티 A가 작업 설명이 있는 Intent를 생성하여 이를 startActivity()에 전달.
2. Android 시스템이 모든 앱에서 해당 인텐트와 일치하는 인텐트 필터를 검색 일치하는 항목을 찾으면,
3. 시스템이 해당 액티비티의 onCreate() 메서드를 호출하여 이를 Intent에 전달하고 , 일치하는 액티비티(Activity B)를 시작한다.

암시적 인텐트는 보통 액션과 데이터라는 속성으로 구성되어있다. 액션은 수행할 기능이며, 데이터는 액션이 수행될 대상 데이터을 의미한다.

```
var intent = Intent(Intent.ACTION_DIAL, Uri.parse(data))
```

예를 들어, 위의 코드에서 액션은 ACTION_DIAL 즉, 전화를 걸라는 액션. 데이터는 Uri.parse(data), 즉 액션이 수행할 data , 전화번호가 된다. 따라서 Uri로 파싱한 전화번호 data를 대상으로 전화다이얼을 걸어라는 뜻이고 이 뜻을 인텐트에 담아 안드로이드 시스템에게 전달하면 된다.

이 두가지 말고도 Category, Type, Component, Extras라는 속성을 가진다. 여기서 Component라는 속성을 지정할 경우 컴포넌트 클래스 이름을 명시적으로 지정하게 되는데 이 경우가 명시적 인텐트에 속하게 된다.

###### IntentFilter
앱이 수신할 수 있는 암시적 인텐트가 어느 것인지 알리려면, `<intent-filter>`요소를 사용하여 각 앱 구성 요소에 대해 하나 이상의 인텐트 필터를 매니페스트 파일에 선언한다. 각 인텐트 필터는 인텐트의 작업, 데이터 및 카테고리를 기반으로 하여 어느 유형의 인텐트를 수락하는지 지정한다. 시스템은 인텐트가 인텐트 필터 중 하나를 통과한 경우에만 암시적 인텐트를 앱 구성 요소에 전달한다.

앱 구성 요소는 자신이 수행할 수 있는 각각의 고유한 작업에 대하여 별도의 필터를 선언해야 한다. 예를 들어 이미지 갤러리 앱에 있는 어떤 액티비티에 두 개의 필터가 있을 수 있다. 한 필터는 이미지를 보고, 다른 필터는 이미지를 편집하기 위한 것이다. 액티비티가 시작되면 , Intent를 검사한 다음 Intent에 있는 정보를 근거로 어떻게 동작할 것인지를 결정.  

각 인텐트 필터는 앱의 매니페스트 파일에 있는 `<intent-filter>` 요소에서 정의하고, 이는 대응되는 앱 구성 요소에 중첩된다. (예 : `<activity>` 요소)

<intent-filter> 내부에서는 `<action>, <data>, <category>` 요소를 사용하여 허용할 인텐트 유형을 지정할 수 있다.

- `<action>`
  - name 특성에서 허용된 인텐트 작업을 선언한다. 이 값은 어떤 작업의 리터럴 문자열 값이어야 하며, 클래스 상수가 아니다.
- `<data>`
허용된 데이터 유형을 선언. 이때 데이터 URI( scheme, host, port, path) 와 MIME 유형의 여러가지 측면을 나타내는 하나 이상의 특성을 사용.
- `<category>`
name 특성에서 허용된 인텐트 카테고리를 선언. 이 값은 어떤 작업의 리터럴 문자열 값이어야 하며, 클래스 상수가 아니다.

예) ACTION_SEND 인텐트를 수신할 수 있는 인텐트 필터

```
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```

***IntentFilter를 동적으로 등록할 수 도 있습니다.*** IntentFilter를 매니페스트 파일에 등록하지 않고 코드에서 동적으로 등록할 수 있습니다.

예제) MMS 메시지 도착 이벤트를 받는 브로드캐스트 리시버를 동적 등록할 때

```
public class ChattingRoomActivity extends AppCompatActivity {
  private MMSReceiver mReceiver;
  protected void onCreate(Bundle savedInstancestate){
  	…
  	registMMSReceiver();
  }

  // 생략

  private void registMMSReceiver() {
      mReceiver = new MMSReceiver();

      /* 인텐트필터 생성 */    
      IntentFilter intentFilter = new IntentFilter();
      intentFilter.addAction(“android.provider.Telephony.WAP_PUSH_RECEIVED”);
      intentFilter.addDataType(“application/vnd.wap.mms-message”);

      /* 브로드캐스트 리시버에 인텐트 필터를 달아준다. */
      this.registerReceiver(mReceiver, intentFilter, Manifest.permission.BROADCAST_WAP_PUSH, null);
  }

  @Override
  protected void onDestroy(){
  	super.onDestroy();
  	this.unregisterReceiver(mReceiver);
  }
}
```

<br>

###### PendingIntent
PendingIntent 객체는 Intent 객체 주변을 감싸는 래퍼이다. 외부 애플리케이션에 권한을 허가하여 안에 들어 있는 Intent를 마치 본인 앱의 자체 프로세스에서 실행하는 것처럼 사용하게 하는 것이 목적.

예) 지정된 시간에 인텐트가 실행되도록 선언 ( Android 시스템의 AlarmManager가 Intent를 실행)

- 일반 인텐트와 차이점?
  - 컴포넌트에서 다른 컴포넌트에게 작업을 요청하는 인텐트를 사전에 생성시키고 만든다는 점과 “특정 시점”에 자신이 아닌 다른 컴포넌트들이 펜딩인텐트를 사용하여 다른 컴포넌트에게 작업을 요청시키는 데 사용된다는 점이 차이점이다.

***“A한테 이 B 인텐트를 C시점에 실행하라고 해. 지금 말고. “***

각 Intent 객체는 특정한 유형의 앱 구성 요소 ( Activity, Service 또는 BroadcastReceiver)가 처리하도록 설계되어 있으므로, PendingIntent도 같은 고려사항을 생각해서 생성해야 한다. PendingIntent를 생성할 때 원래 의도한 구성 요소 유형을 선언해야 한다.

- Activity를 시작하는 Intent의 경우, PendingIntent.getActivity()
- Service를 시작하는 Intent의 경우, PendingIntent.getService()
- BroadcastReceiver를 시작하는 Intent의 경우, PendingIntent.getBroadcast()

앱이 다른 앱에서 PendingIntent를 수신하지 않는 한, 위의 PendingIntent를 생성하는 메서드만 있으면 된다. 각 메서드는 현재 앱의 Context, 감싸고자 하는 Intent, 인텐트의 적절한 사용 방식을 나타내는 하나 이상의 플래그(예: 인텐트를 한번 이상 사용할 수 있는 지 여부) 등을 취한다.

여기에 적절한 예제와 자세한 설명이 있다. (https://techlog.gurucat.net/80)

<br>

### Parcelable vs Serializable 차이점

- 참고
  - https://medium.com/@limgyumin/parcelable-vs-serializable-%EC%A0%95%EB%A7%90-serializable%EC%9D%80-%EB%8A%90%EB%A6%B4%EA%B9%8C-bc2b9a7ba810
  - https://android.jlelse.eu/parcelable-vs-serializable-6a2556d51538

###### Serializable
Serializable 은 Android SDK 가 아닌 표준 Java 의 인터페이스이다. Serializable은 해당 클래스가 직렬화 대상이라고 알려주기만 할뿐 어떠한 메서드도 가지지 않는 단순한 "마커 인터페이스"이므로, 사용자는 매우 쉽게 사용할 수 있다.

사용방법이 쉽다는 것은 곧 시스템 적인 비용이 비싸다는 것.

Serializable은 내부에서 Reflection을 사용하여 직렬화를 처리한다. Reflection은 프로세스 동작 중에서 사용되며 처리 과정 중에 많은 추가 객체를 생성한다. 이 많은 쓰레기들은 가비지 컬렉터의 타겟이 되고 가비지 컬렉터의 과도한 동작으로 인하여 성능 저하 및 배터리 소모가 발생한다.

Reflection 이란? (https://www.charlezz.com/?p=756)

###### Parcelable
Parcelable은 직렬화를 위한 또다른 인터페이스이다. Serializable과 달리 표준 Java가 아닌 Android SDK의 인터페이스이다.

Parcelable은 Reflection을 사용하지 않도록 설계되었다. Serializable 과는 달리 직렬화 처리 방법을 사용자가 명시적으로 작성하기 때문에 자동으로 처리하기 위한 Reflection이 필요 없다.

마커 인터페이스인 Serializable과 달리 Parcelable은 구현해야 하는 필수 메서드를 포함하기 때문에 클래스에 보일러 플레이트 코드가 추가 된다. 이는 클래스를 이해하기 어렵고, 새로운 기능을 추가 하기 힘들게 만든다. 또한 코드의 추가로 클래스가 복잡해 져서 유지 보수가 어려워지는 원인이 된다.

###### Serializable vs Parcelable

실험 결과는 Parcelable이 Serializable 보다 10배 이상 빠르다는 것을 보여준다. 일부의 구글 엔지니어들도 이 결과에 대해 지지하고 있다.

***그러나!!*** Philipe Breault의 테스트 방법은 불공평하다고 주장하는 그룹이 있다.

Parcelable은 하나의 클래스 객체 만을 위한 특별한 사용자 정의 코드를 작성해야 한다. 사용자 정의 코드의 도움으로 Parcelable은 Serializable과는 달리 쓰레기를 생성할 이유가 없어지고 그 결과 성능이 좋아진다.

그러나 '기본 사용법에 의한 Serializable’은 Java의 자동 직렬화 프로세스를 사용한다. 이 과정은 분명 Parcelable과 같은 수고로움은 없지만 처리 과정에서 많은 쓰레기를 만든다. **따라서 , 더 나쁜 결과를 얻는다.**

그렇다면 다른 방법으로 Serializable을 사용한다면?

Serializable 에서 자동으로 처리되는 직렬화 프로세스는 사용자가 구현한 writeObject()와 readObject() 메서드로 대체될 수 있다. 사용자가 구현한 직렬화 메서드와 함께 Serializable 접근 방식을 사용하려면 다음의 메서드들을 반드시 구현해야 한다.

```
private void writeObject ( java.io.ObjectOutputStream out)
	throws IOException;

private void readObject ( java.io.ObjectInputStream in)
	throws IOException, ClassNotFoundException;

private void readObjectNoData ( )
	throws ObjectStreamException;
```

마치 Parcelable의 사용법 처럼 Serializable의 writeObject()와 readObject()메서드에는 해당 클래스에 맞는 처리 로직을 포함시킬 수 있다. 제대로 수행된다면, 기본 사용법에 의한 Serializable 방식에서 발생하는 쓰레기가 더이상 생성되지 않는다.

이제 Parcelable과 Serializable의 비교가 공정해 졌다. 테스트 결과는 놀랍게도 특정 클래스 처리 로직을 구현한 Serializable이 Parcelable 보다 쓰기 속도가 3배 이상, 읽기의 경우 1.6배 더 빠르다!!

https://github.com/GMLim/Android-Serialization-Test
위 링크에서 테스트 데이터가 포함된 프로젝트를 다운받아 테스트 해볼 수 있다.

<br>

### Fragment를 보통 static function 를 통해 생성하는 이유 (bundle)
프래그먼트를 싱글톤으로 사용해야하는 이유

- https://lanace.github.io/articles/why-do-not-use-arguments-in-fragment/
- http://blog.naver.com/horajjan/220327553797
- https://www.slideshare.net/iamhjoo/ss-44214966

<br>

### RecylcerView와  ListView의 차이에 대해 설명하시오

참고링크 : ListView vs RecyclerView

1. ListView(1.0 ~)
2. RecyclerView(5.0 ~)
3. 차이점


* ViewHolder패턴을 사용한 ListView와 RecyclerView의 성능차이는 없다.
* Adapter 클래스를 직접 구현하기 때문에 뷰 커스텀 작업이 유연하다.
* ListView는 머리글, 바닥글을 기본적으로 셋팅할수있고 Recyclerview는 구현해야한다.

<br>

### ViewHolder 패턴에 대해 설명하시오
ViewHolder란?
ListView / RecyclerView 는 inflate를 최소화 하기 위해서 뷰를 재활용 하는데, 이 때 각 뷰의 내용을 업데이트 하려면 findViewById 를 매번 호출 해야한다. 이로 인해 성능저하가 일어남에 따라 ItemView의 각 요소를 바로 엑세스 할 수 있도록 저장해두고 사용하기 위한  각 뷰를 보관하는 Holder 객체이다
※ inflate? : xml 로 쓰여있는 View의 정의를 실제 View 객체로 만드는 것을 말함.
ViewHolder Pattern

getTag(), setTag()로 뷰를 재사용하여, 데이터의 값(position)을 변경한다.

ConstraintLayout을 사용해야 하는 이유가 무엇인가요?
ConstraintLayout이란
: ConstraintLayout은 복잡한 레이아웃을 단순한 계층 구조를 이용하여 표현할 수 있는 ViewGroup이다. 형제 View들과 관계를 정의해서 레이아웃을 구성한다는 점이 RelativeLayout과 비슷하지만, 보다 유연하고 다양한 기능을 제공한다. (ex. 두 View를 위 아래로 컨테이너 중앙에 배치하기등) 참고
기존에 비해 어떤점이 더 좋은지
: RelativeLayout의 "상대적 위치 관계에 따른 배치" 특성에 LinearLayout의 "가중치(weight)가 가진 장점"을 적용하고, 체인(chain) 사용으로 다른 레이아웃 없이 "요소들을 그룹화"할 수 있다.
: 뷰 계층을 간단하게 구성하여 유지보수가 용이하며 성능면에서 우수하다.
