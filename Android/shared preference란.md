# Shared preference
## SharedPreferences란?
    간단한 값을 저장할 때 주로 사용하는 초기 설정 값이나 자동 로그인 여부 등 간단한 작업을 저장할 때 DB를 사용하면 복잡하기 때문에 SharedPreferences를 사용하면 편리하다.

**SharedPreferences는 어플리케이션에 파일 형태로 데이터를 저장한다. 데이터는 (key, value) 형태로 data/data/패키지명/shared_prefs 폴더 안에 xml 파일로 저장된다. 해당 파일은 어플리케잇녀 삭제되기 전까지 보존된다.**

## SharedPreferences의 장점
    int, float, String, boolean 등 간단한 데이터를 저장하고 불러올 수 있다. 앱을 꺼도 데이터가 유지된다는 점에서 간편한 데이터베이스 역할을 할 수 있다.


## SharedPreferences 사용법 
