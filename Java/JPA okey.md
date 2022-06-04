# JPA

## ORM

자바 객체와 레코드를 매핑시켜주는 것

## JPA Repository

Spring Data JPA는 `**JpaRepository**` 를 통해 데이터를 쉽게 조작할 수 있도록 한다. 

```java
@Entity
class Student {
	@ID
	@GeneratedValue
	private Long Id;
}
```

```java
interface StudentRepository extends JpaRepository<Student, Long> {
}
```

### Jpa Repository 제공 메서드

- findAll() : 테이블에 저장된 모든 데이터를 조회
- findAll(Sort sort) : 주어진 정렬 기준을 바탕으로 데이터 전체 조회
- findAllById(Iterable<ID> ids) : ID 리스트를 조건을 걸어 데이터 조회
- saveAll(Iterable<S> entity) : 리스트에 저장된 엔티티들을 테이블에 전부 저장
- saveAndFlush(S entity) : 저장한 데이터를 JPA 컨텍스트에서 가지고 있지 말고 바로 DB 에 반영해서 적용
- deleteAllInBatch() : 테이블에 있는 데이터를 모두 삭제
- deleteInBatch(Iterable<T> entities) : 리스트에 저장된 엔티티들을 테이블에서 삭제
- getOne(Id id) : 아이디를 이용하여 데이터를 조회

### PagingAndSortingRepository

JpaRepository 는 PagingAndSortingRepository 를 상속받기 때문에 해당 메서드도 사용 가능하다.

- findAll(Sort sort) : 정렬 기준으로 데이터 조회
- findAll(Pageable pageable) : 페이징을 활용하여 데이터 조회

### 기타 함수들

- count() : 레코드 갯수 확인
- existById(Long Id) : Id가 테이블에 존재하는지 확인하는 함수. 결과는 boolean 타입이다.
- delete(엔티티) : 해당 엔티티를 삭제

### data.sql

resources 폴더 하위에 data.sql 파일을 두면 JPA 가 한번 실행시켜 준다는 의미다.

### Spring Data JPA 설정 (application.yml)

```xml
spring:
	h2:
		console:
			enabled: true
	jpa:
		show-sql: ture // jpa 가 생성해주는 쿼리를 보겠다는 의미
		properties:
			hibernate: 
				format_sql: true // 쿼리문을 보기좋게 변환
```

### findById(Optional 리턴) vs getOne(Entity 리턴)

```java
@Test
void getOneTest() {
	Student student = studentRepository.getOne(1L);
	sout(student)
}
```

getOneTest 메서드를 실행시키면 LazyInitializationException 이 발생하고 no Session이란 문구를 볼 수 있다. @Transactional 을 붙이게 되면 세션을 유지하게 되고 저장된 객체를 출력할 수 있다.

```java
@Test
void findByIdTest() {
	Optional<Student> student = studentRepository.findById(1L);
	sout(student);
}
```

findById는 엔티티가 아닌 Optional을 리턴한다.

### flush() 와 saveAndFlush()

```java
void flushTest() {
	studentRepository.save(new Student("sayho", "sayho"));
	studentRepository.flush();
	studentRepository.findAll().forEach{
		sout() 
	}
}
```

```java
void saveAndFlushTest() {
	studentRepository.saveAndFlush(new Student("hozumi", "hozumi"));
	
}
```

### JPA Paging

- Pageable 의 구현체는 PageRequest다. PageRequest의 of 메서드를 활용하여 페이징처리된 데이터를 가져올 수 있다.

```java
Page<Student> students = studentRepository.findAll(PageRequest.of(1, 3));
```

 

# JPA 영속성 컨텍스트

### 영속성 컨텍스트

- 영속성 컨텍스트는 **`엔티티를 영구 저장하는 환경`** 으로 ****애플리케이션과 데이터베이스 사이에서 객체를 보관하는 논리적인 개념이다.
- `**EntityManager**`을 통해 영속성 컨텍스트에 접근하고 관리할 수 있다.
- 영속성 컨텍스트는 `1차 캐시`와 `쓰기 지연 SQL 저장소`를 가지고 있다.

```java
	@Transactional
	@Override
	public <S extends T> S save(S entity) {

		Assert.notNull(entity, "Entity must not be null.");

		if (entityInformation.isNew(entity)) {
			em.persist(entity);
			return entity;
		} else {
			return em.merge(entity);
		}
	}
```

새로운 엔티티가 삽입되면 persist 기존에 있는 엔티티라면 merge 메서드가 실행된다. 데이터를 추가할 때, 수정할 때 모두 save 메서드가 실행된다.

save가 되었다 해서 데이터베이스에 데이터가 삽입되거나 수정되는 것이 아니다. 영속성 컨텍스트의 변경내역을 데이터베이스에 완전히 반영하기 위해서는 flush를 해줘야한다.

### Flush 발생 조건

- 엔티티의 변경 감지
- 변경된 엔티티 쓰기 지연 SQL 저장소에 UPDATE 쿼리 저장
- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송 (등록, 수정, 삭제 쿼리)

### 영속성 컨텍스트 Flush 방법

- EntityManager을 flush 메서드를 활용해서 직접 flush
- 트랜잭션 커밋하면 영속성 컨텍스트는 flush 된다. `@Transactional` 을 사용하게 되면 커밋됨
- JPQL 쿼리 실행을 하게되면 JPQL 수행 바로 전에 자동으로 flush를 발생시켜 DB와 영속성 컨텍스트간의 동기화를 수행한다.

### 특징 1. JPA 쓰기지연

쓰기 지연은 엔티티를 저장하기 위해 persist() 를 호출해도 데이터베이스에 쿼리가 바로 전송 되는 게 아닌 트랜잭션이 커밋되는 시점(flush()가 호출되는 시점)에 쿼리가 전송되는 것을 의미한다.

### 특징 2. 변경 감지

엔티티를 수정할 때 엔티티를 조회해서 변경된 데이터만 변경한다. Entity 가 적재되는 순간을 기록하는 스냅샷 이라는 공간이 있고, 저장된 스냅샷과 트랜잭션이 종료 되었을 때 엔티티와 스냅샷을 비교하고 두개의 차이가 있으면 변경된 정보를 update 쿼리로 만들어서 쓰기 지연 SQL 저장소에 적재하게 된다.