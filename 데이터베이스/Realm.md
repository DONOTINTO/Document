# 목차
1. [Realm 이란](#realm-이란)
2. [Realm 특징](#realm-특징)
3. [Realm 사용법](#realm-사용법)
    - [객체모델생성](#객체-모델-생성)
    - [Realm 생성 및 위치 확인](#realm-객체-생성-및-위치-확인)
    - [CRUD](#crud)
4. [Realm Relationship](#relationship)
5. [Migration](#migration)



## Realm 이란?

realm은 오픈 소스 데이터베이스 관리시스템(DBMS)으로, 모바일에 최적화되어있는 라이브러리다.

## Realm 특징
1. IOS에서만 사용할 수 있는 UserDefaults와 CoreData와 다르게 IOS, Android, React Native등 다양한 플랫폼에서 사용이 가능하다.

2. [ORM](#ormobject-reational-mapping)가 아닌 데이터 [컨테이너](#데이터-컨테이너) 모델을 사용하여, 데이터 객체는 Realm 객체로 저장된다.
Realm은 객체 형태로 데이터를 저장하고 읽기 때문에 별도의 가공 과정이 필요하지 않다.

## Realm 사용법

### 객체 모델 생성
```swift
import RealmSwift

class Person: Object {
    @Persisted(primaryKey: true) var id: ObjectId
    @Persisted var name: String
    @Persisted var gender: Bool
    
    convenience init(name: String, gender: Bool) {
        self.init()
        
        self.name = name
        self.gender = gender
    }
}

```

### Realm 객체 생성 및 위치 확인

```swift
let realm = try! Realm()
print("Realm is located at:", realm.configuration.fileURL!)
```

### CRUD 

```swift
// Create
func write<T: Object>(_ item: T) {
    do {
        try realm.write {
            realm.add(item)
        }
    } catch {
        print(error.localizedDescription)
    }
}

// Read
func fetch<T: Object>(_ type: T.Type) -> Results<T> {
    return realm.objects(type)
}

// Read - Object ID로 Object 가져오기
func fetchByObjectID<T: Object>(type: T.Type ,_ id: ObjectId) -> T? {
    return realm.object(ofType: type, forPrimaryKey: id)
}

// Update - Object ID로 value 값 변경하기
func updateByID<T: Object>(_ type: T.Type, id: ObjectId, value: Any) {
    do {
        try realm.write {
            realm.create(type,
                         value: [
                            "id": id,
                            "value": value
                         ],
                         update: .modified)
        }
    } catch {
        print(error.localizedDescription)
    }
}

// Delete
func delete<T: Object>(_ item: T) {
    do {
        try realm.write {
            realm.delete(item)
        }
    } catch {
        print(error.localizedDescription)
    }
}
```

## Relationship

1. To - One Relationship (1:1 관계)

하나의 프로퍼티에 다른 객체의 인스턴스를 하나만 매핑한다.
프로퍼티는 다른 객체의 타입을 옵셔널로 선언해야한다.

2. To - Many Relationship (1:다 관계)

하나의 프로퍼티에 다른 객체의 인스턴스를 여러개 매핑한다.   
List 태그를 사용한다.

3. Inverse Relationship (역관계) 

역관계 속성은 자동 back link 관계로, Realm은 to-many 또는 to-one 관계 속성에서 객체가 추가되거나 제거될 때마다 자동으로 업데이트한다.

4. Embedded Object (임베디드)

임베디드 객체는 독립적인 객체로 존재할 수 없으며, 부모 객체의 수명 주기를 상속한다.
Realm 은 부모 객체가 삭제되거나 새로운 인스턴스로 인해 덮어지면 내장된 객체를 자동으로 삭제한다.
To - One Relationship과 같이 옵셔널로 선언해야한다.

```swift
class Person: Object {
    @Persisted(primaryKey: true) var id: ObjectId
    @Persisted var name: String
    @Persisted var gender: Bool
    
    // MARK: to - many relationship
    @Persisted var pets: List<Pet>
    
    // MARK: EmbeddedObject

    @Persisted var address: Address?
    
    convenience init(name: String, gender: Bool) {
        self.init()
        
        self.name = name
        self.gender = gender
    }
    
}

class Pet: Object {
    @Persisted(primaryKey: true) var id: ObjectId
    @Persisted var type: String
    
    // MARK: Inverse Relationship
    @Persisted(originProperty: "pets") var person: LinkingObjects<Person>
    
    convenience init(type: String) {
        self.init()
        
        self.type = type
    }
}

// MARK: EmbeddedObject
class Address: EmbeddedObject {
    @Persisted var street: String?
    @Persisted var city: String?
    @Persisted var country: String?
    @Persisted var postalCode: String?
}
```

## Migration

object의 schema(구조)가 변경될 때, schema 버전을 증가시켜주어야 한다.

schema 버전의 기본값은 0이다.

schema 버전만 늘린다면, 옵셔널 프로퍼티가 추가되거나 프로퍼티가 삭제되는건 realm에서 자동으로 변경해준다.

조금 더 복잡한 스키마 업데이트는 수동으로 로직을 설정해줘야한다.

- 기본값이 필요한 프로퍼티의 추가
- 컬럼 합치기
- 컬럼명 변경
- 컬럼 타입 변경
- 일반 객체에서 임베디드 객체로 변경 

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        let configuration = Realm.Configuration(schemaVersion: 2) {
            migration, oldSchemaVersion in
            
            // 1: 옵셔널 프로퍼티 추가 / 프로퍼티 삭제의 경우 별도 코드를 작성 X
            if oldSchemaVersion < 1 {
                print("schema: 0 -> 1")
            }
            
            // 복잡한 스키마 변경의 경우 로직 설정
            if oldSchemaVersion < 2 {
                migration.enumerateObjects(ofType: Todo.className()) { oldObject, newObject in
                    
                    guard let newObject else { return }
                    
                    newObject["count"] = 100
                }
            }
        }
        
        Realm.Configuration.defaultConfiguration = configuration
        
        return true
    }
```

### deleteRealmIfMigrationNeeded
디버깅 시 마이그레이션 대신 위 deleteRealmIfMigrationNeeded를 사용하면 자동으로 기존 스키마를 삭제한다.

릴리즈 버전에서는 사용하면 안된다.

### Linear Migrations
위 예시처럼 마이그레이션 버전별로 진행해야한다.
건너 뛸 경우 스키마 업데이트가 제대로 진행되지 않을 수 있다.

### Schema Version 확인
```swift
do {
    let version = try schemaVersionAtURL(realm.configuration.fileURL!)
    print(version)
    } catch {
        print(error)
    }
```


## 용어

###### ORM(Object Reational Mapping)
 - 객체와 관계형 데이터베이스를 매핑해줌

###### 데이터 컨테이너
 - OS상에 논리적인 구획(컨테이너)을 만들고, 별도의 서버인 것 처럼 사용할 수 있음