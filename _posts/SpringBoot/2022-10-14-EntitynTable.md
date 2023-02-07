---
layout: single
title: "Entity와 Table의 차이점"

categories:
 - Springboot
---

## 스프링 JPA이란?
### Java Persistence API (자바 ORM 기술에 대한 API 표준 명세)
한마디로 ORM을 사용하기 위해 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스이다. <br>
ORM에 대한 자바 API 규격이며 Hibernate, OpenJPA 등이 JPA를 구현한 구현체 이다. (ORM을 사용하기 위한 인터페이스를 모아둔 것) <br>
이런 형태를 통해 우린 JPA을 이용하여 데이터베이스를 접근한다. <br> <br>
스프링부트 JPA 에서 Entity에 해당하는 파일에 **@Entity, @Table** 어노테이션을 사용할 수 있는데 <br>
일단 데이터베이스에 접근하기 위해서 @Entity는 필수로 들어가야 한다. <br> <br>

### Entity
@Entity만 사용했을 경우 DB와 연결 시 테이블명은 클래스명과 동일하게 설정된다. <br>
예를 들어 @Entity 어노테이션을 사용한 상태에서 클래스명이 CrudEntity일 경우 DB에서 CrudEntity 테이블로 연결된다. <br> 
@Entity(name = "엔티티명") 으로 테이블명을 지정 EntityManager 등을 이용해 쿼리를 사용할 경우 createQuery("select .. from 엔티티명") 이렇게 사용할 수 있게 된다 <br> <br>

### Table
@Table의 경우에는 외부에서 호출하는 용도가 아닌 **실제 DB에 붙을 테이블명 어노테이션**을 말한다. <br>
@Entity와 조합을 해서 사용해보면 아래 이미지와 같이 @Entity 설정 후 @Table(name = "테이블명") 으로 해준다. <br>

![image](https://user-images.githubusercontent.com/81789003/195852815-d9d77528-afcf-4370-89d4-fa9f45dea5e4.png)

이후 createQuery(select m from 엔티티클래스명) 으로 호출을 해 주면 <br>
호출은 엔티티클래스명으로 하지만 실제 DB에는 테이블명의 테이블로 붙게 된다 <br>
