# NoSQL with MongoDB

## 1. MongoDB

MongoDB

- document 기반 NoSQL 데이터베이스
- BSON(Binary JSON) 기반의 Document로 데이터 관리
- JSON보다 대용량 데이터 처리 시 유용
- BSON: 이진 형식으로 데이터를 빠르게 스캔하고 처리
- JSON 데이터를 0과 1 형식으로 변환해 내부에서 저장하면 성능상 이점이 있음

MongoDB 데이터 구조

- Database
- Collection (Table)
- Document (Record)

## 2. 기본 명령

```
show dbs;
use dbName;
show collections;
db.collectionName.funcName();
db.status();
db.dropdatabase();
db.collectionName.drop();
db.createCollection(name, options);
db.collectionName.renameCollection(newCollectionName);
```

```
db.createCollection(name, options);
db.collectionName.isCapped();
db.createCollection('log', { capped: true, size: 5242880, max: 5000 });
```

- `capped`: true로 설정할 시 고정된 크기를 가진 capped 컬렉션을 생성. 그 크기가 꽉 차면 가장 오래된 데이터부터 자동으로 삭제 (기본값은 false)
- `autoIndexId`: true로 설정할 시 `_id` 필드에 대한 인덱스를 자동으로 생성 (기본값은 false)
- `size`: capped 컬렉션의 최대 바이트 크기를 지정 (capped가 true일 때만 사용)
- `max`: capped 컬렉션에 저장할 수 있는 문서의 최대 개수를 지정

## 3. 주요 데이터 타입

- String
- Integer
- Boolean
- Double
- Arrays
- Object
- Null
- ObjectId
- Date

## 4. CRUD

### 1. Insert

- `insertOne()`
- `insertMany()`

```
db.users.insertOne({ name: '', age: 10, addr: '' });
db.users.insertMany([
{ name: '', age: 10, addr: '' },
{ name: '', age: 20, addr: '' },
]);
```

### 2. Find

- `findOne()`
- `find()`

```
db.users.find(
  { age: { $gt:18 } },
  { name: 1, address: 1 }
).limit(5);
```

비교 문법

- `$eq`: =
- `$ne`: !=
- `$gt`: >
- `$gte`: >=
- `$lt`: <
- `$lte`: <=
- `$in`: 값이 배열에 포함됨
- `$nin`: 값이 배열에 포함되지 않음

논리 연산 문법

- `$or`
- `$and`
- `$not`

```
db.users.find({ age: 10, addr: 'seoul' });
db.users.find({ $and: [{ age: 10 }, { addr: 'seoul' }] });
```

정규표현식

- `$regex`

```
db.users.find({ name: /Kim/ });
db.users.find({ name: { $regex: /Kim/ } });
```

정렬

- `sort()`

```
db.users.find().sort({ age: 1 });
db.users.find().sort({ age: -1 });
```

카운트

- `count()`

```
db.users.find().count();
```

필드 존재 여부로 카운트

- `$exists`

```
db.users.find({ addr: { $exists: true } });
```

중복 제거

- `distinct()`

```
db.users.distinct('addr');
```

제한

- `limit()`

```
db.users.find().limit(5);
```

조건

```
db.users.find({ addr: { $all: ['seoul', 'busan'] } }); // 모두 맞아야 조회
db.users.find({ addr: { $in: ['seoul', 'busan'] } }); // 하나라도 맞으면 조회
db.users.find({ addr: { $nin: ['seoul', 'busan'] } }); // 어떤 것도 안 맞으면 조회
```

### 3. Update

- `updateOne()`
- `updateMany()`

```
db.users.updateMany(
  { age: { $gt: 10 } },
  { $set: { addr: 'busan' } }
);
db.users.updateMany(
  { addr: { $eq: 'seoul' } },
  { $inc: { age: 2 } }
);
```

### 4. Delete

- `deleteOne()`
- `deleteMany()`

```
db.users.deleteOne();
```
