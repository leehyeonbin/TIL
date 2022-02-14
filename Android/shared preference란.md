# Shared preference
## SharedPreferences란?
    간단한 값을 저장할 때 주로 사용하는 초기 설정 값이나 자동 로그인 여부 등 간단한 작업을 저장할 때 DB를 사용하면 복잡하기 때문에 SharedPreferences를 사용하면 편리하다.

    저장하려는 키-값 컬렉션이 비교적 작은 경우 SharedPreferences API를 사용해야 한다. SharedPreferences 객체는 키-값 쌍이 포함된 파일을 가리키며 키-값 쌍으로 읽고 쓸 수 있는 간단한 메서드를 제공한다. 각 SharedPreferences 파일은 프레임워크에서 관리하며 비공개이거 공유일 수 있다.


**SharedPreferences는 어플리케이션에 파일 형태로 데이터를 저장한다. 데이터는 (key, value) 형태로 data/data/패키지명/shared_prefs 폴더 안에 xml 파일로 저장된다. 해당 파일은 어플리케잇녀 삭제되기 전까지 보존된다.**

## SharedPreferences의 장점
    int, float, String, boolean 등 간단한 데이터를 저장하고 불러올 수 있다. 앱을 꺼도 데이터가 유지된다는 점에서 간편한 데이터베이스 역할을 할 수 있다.

    
## SharedPreferences 사용하기

**(1) companion object 사용하기**

SharedPreferences는 앱의 어디서든 전역적으로 사용하기 때문에 싱글톤 패톤을 사용해서 어디서든 접근 가능하게 만드는 것이 좋다.

SharedPreferences 클래스는 앱에 있는 다른 액티비티보다 먼저 생성되어야 다른 곳에 데이터를 넘겨줄 수 있다. 이러기 위해서는 Application에 해당하는 클래스를 생성한 뒤, 전역 변수로 SharedPrefereneces를 가지고 있어야한다. Application()을 상속 받는 MyApplication 클래스를 생성하여, onCreate()보다 먼저 prefs를 초기화 해준다.


 ```kotlin
 MyApplication.kt
class myApplication : Application() {
    companion object {
        lateinit var prefs: PreferenceUtill
    }

    override fun onCreate() {
        prefs = PreferenceUtil(applicationContext)
        super.onCreate()
    }
}

 ```
MyApplication 클래스를 생성했다면, Manifest에 등록해줘야 한다.
 ```xml
 AndroidManifest.xml
 <applicatoin
    android:name="com.example.test.MyApplication">
    ...
    </application>
 ```

 다음은 SharedPreferences를 변수로 가지고 있는 PreferenceUtil 클래스를 생성한다. getString()과 setString() 메소드를 만들어서 SharedPreferences에 접근하여 데이터를 저장하거나 가져올 것이다.

```kotlin
PreferenceUtil.kt

class PreferenceUtil(context : Context) {
    private val prefs: SharedPreferences =
        context.getSharedPreferences("prefs_name", Context.MODE_PRIVATE)

    fun getString(key : String, defValue : String) :  String {
        return prefs.getString(key, defValue).toString()
    }

    fun setString(key : String, str : String) {
        prefs.edit().putString((key, str).apply()
    }
}
```
Mode의 종류는 
* MODE_PRIVATE : 생성한 application에서만 사용 가능
* MODE_WORLD_READABLE : 다른 application 에서 사용 가능 -> 읽기만 가능
* MODE_WORLD_WRITABLE : 다른 application 에서 사용 가능 -> 기록도 가능
* MODE_MULTI_PROCESS : 이미 호출되어 사용중인지 체크
* MODE_APPEND : 기존 preference에 신규로 추가

~~그냥 습관적으로 MODE_PRIVATE 해도 사실 상관없다.~~

데이터를 저장하거나 가져올 때는 아래의 예시코드를 사용해 접근하면 된다.
```kotlin
// 데이터 조회
MyApplication.prefs.getString("email","no email")

// 데이터 저장
MyApplication.prefs.setString("email","absd@gmail.com")
```

**(2) object 사용하기**

```kotlin
object SharedPreferenceManager {
    private const val PREF_TOKEN = "token"
    private const vla PREF_NAME = "name"
    private const val PREF_MODE = "Context.MODE_PRIVATE"
    private lateinit var sharedPreferences : SharedPreferences

    fun init(context : Context) {
        sharedPreferences = context.getSharedPreferences(PREF_NAME, PREF_MODE)
    }

    private inline fun SharedPreferences.edit(
        operation : 
    (SharedPreferences.Editor) -> Unit) {
        val editor = edit()
        operation(edit())
        edit().apply() 
    }

    var token : String
        get() = sharedPreferences.getString(PREF_TOKEN, "").toString()
        set(value) = sharedPreferences.edit {
            it.putString(PREF_TOKEN, value)
        }
}
```

```kotlin
class SharedFreferenceActivity : AppCompayActivity() {
    override fun onCreate(savedInstanceState : Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_shared_preference)

        // SharedPreference
        val sharedPreference = getSharedPreference("sp1", Context.MODE_PRIVATE)

        val editor : SharedPreference.Editor = sharedPreference.edit()
        editor.putString("hello", "안녕하세요")
        editor.commit()

        // sp1 -> SharedPreference
        //        (Key, Value) -> ("Hello", "안녕하세요")
        // sp2 -> SharedPreference
        //        (Key, Value) -> ("Hello", "안녕하세요11")

        btn. setOnClickListener {
            // SharedPreference 에 값을 불러 오는 방법
            val sharedPreference = getSharedPreferences("sp1", Context.MODE_PRIVATE)
        }
    }
}
```



```kotlin
val value = sharedPreference.getString("hello", "데이터 없음")
//                                    -> Key,    -> defaultValue
Log.d("key-value", "Value : " + value)
val value = sharedPreference.getString("hello1" "데이터 없음")
Log.d("key-value", "Value : " + value)
```

``` 
콘솔창


Value : 안녕하세요
Value : 데이터 없음
```


출처 : 
* [Kotlin에서 SharedPreferences 싱글톤으로 사용하기](https://velog.io/@yeji/Kotlin%EC%97%90%EC%84%9C-SharedPreferences-%EC%8B%B1%EA%B8%80%ED%86%A4%EC%9C%BC%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0) 

