---
title: "[Spring] maven과 gradle의 특징과 차이점"
excerpt: ""

categories: Spring
tags: [Spring, maven, gradle]

date: 2022-04-06
last_modified_at: 2022-04-06
---

> 자바 스프링 부트에 대해 공부하다보니 빌드 관리 도구인 maven과 gradle을 배우게 됐다. 겉으로 보기에 maven이 코드의 가독성이 떨어지고 더 길다는 단점이 있어 보였지만 이 이유만으로 gradle이 더 좋은 빌드 관리 도구일까라는 궁금증이 들어 이번에 maven과 gradle의 특징과 차이점을 정리해보려고 한다.


## 빌드 관리 도구
먼저 빌드 관리 도구란 무엇인지 알 필요가 있다.

> 빌드 관리 도구란 프로젝트에서 작성한 소스 코드와 프로젝트 내의 xml, properties, yml, jar 파일 등을 JVM, WAS가 인식할 수 있도록 패키징 해주는 빌드 자동화 도구

프로젝트의 생성, 테스트, 배포 작업을 위한 프로그램이다.


빌드 도구 설정 파일(pom.xml, build.gradle)에 라이브러리의 종류, 버전, 종속성 등을 명시하면 필요한 라이브러리를 자동으로 다운받아 사용할 수 있도록 돕는다.


## Maven(메이븐)
기존에 존재하던 Ant 빌드 파일의 단점을 보완해 빌드 프로세스를 단순화할 수 있도록 2004년에 출시되었다.

[공식 홈페이지](https://maven.apache.org/what-is-maven.html)를 보면 Maven 4가지 목표가 있다.
1. <B>Making the build process easy(빌드 프로세스를 쉽게 만들기)
2. <B>Providing a uniform build system(정형화된 빌드 시스템 제공)</B>: pom.xml 파일을 이용해 빌드 시스템 설정
3. <B>Providing quality project information(퀄리티 있는 프로젝트 정보 제공)
4. <B>Providing guidelines for best practices development(최고의 개발 가이드라인 제공)</B>: 실제로 공식 사이트에 들어가보면 다양한 가이드라인이 나와있다.

---
공식 사이트에서 소개하는 목표는 이렇고 실제로 Maven을 사용하면서 편리한 점은 라이브러리를 pom.xml에 명시하기만 하면 자동으로 관련된 라이브러리들까지 연동되어 관리가 되어진다는 점이다.  
즉, 원하는 모듈(spring-boot-starter-data-jpa)을 pom.xml에 넣어주면 자동으로 mvn-repository에서 다운로드 후 빌드까지 해준다.


pom.xml에서 POM이란 Project Object Model의 줄임말로 프로젝트의 오브젝트 모델 정보를 담고 있다는 의미이다.  
pom.xml 파일에는 주요하게 다루는 기능들은 다음과 같다.
- 프로젝트 정보: 프로젝트명, 라이센스 등
- 빌드 설정: 리소스, 플러그인 등 빌드 관련 설정
- 빌드 환경: 사용자 환경 별로 달라질 수 있는 프로파일 정보
- POM 연관 정보: 상위 프로젝트, 하위 모듈 등


Maven과 관련된 또 하나의 설정 파일은 settings.xml이 있다.  
해당 파일은 Maven tool 자체에 설정을 바꾼다.  
Maven tool 자체에 대한 설정을 바꾸는 경우는 거의 없기 때문에는 실제로 잘 사용하지 않는다고 한다.


### pom.xml 파일 분석
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.cos</groupId>
	<artifactId>blog</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>blog</name>
	<description>My First Blog Project</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```
위 코드는 실제로 스프링 부트 연습 프로젝트에서 쓰고 있는 pom.xml 파일의 일부분이다.
위에서부터 차례대로 살펴보자.
- <B>\<modelVersion\></B>: pom.xml의 모델 버전을 나타난다.
- <B>\<parent\></B>: pom.xml은 상속을 받을 수 있는데, 부모 pom.xml의 정보를 나타낸다. 기본적으로 super pom.xml을 상속받고 있다.
- <B>\<groupId\></B>: 프로젝트 생성 조직 or 그룹명을 나타낸다. 보통 도메인의 역순으로 표시한다.
- <B>\<version\></B>: 프로젝트의 버전을 나타낸다.
- <B>\<name\></B>: 프로젝트명을 나타낸다.

---
- <B>\<properties\></B>: pom.xml에서 공통적으로 사용되는 설정값(상수)을 지정해놓을 수 있다. 다른 위치에서는 `${java.version}`로 표시해 사용할 수 있다.
- <B>\<dependencies\></B>: 의존성 라이브러리를 설정할 수 있다. groupId, artifactId 정보가 필요하고 버전을 지정할 수도 있다. 스프링 부트에서는 기본적으로 해당 스프링 부트 버전에 적합한 라이브러리 버전을 자동으로 세팅해준다.
- <B>\<build\></B>: 빌드와 관련된 정보를 설정할 수 있다. 아래는 설정할 수 있는 값들이다.
    - <B>\<finalName\></B>: 빌드 결과물의 이름을 설정할 수 있다.
    - <B>\<resources\></B>: 리소스 파일의 위치를 지정할 수 있다. 지정하지 않을 시 기본적으로 'src/main/resources'이다.
    - <B>\<testResources\></B>: 테스트 리소스 파일의 위치를 지정할 수 있다. 지정하지 않을 시 기본적으로 'src/test/resources'이다.
    - <B>\<outputDirectory\></B>: 컴파일 결과물의 위치를 지정할 수 있다. 지정하지 않을 시 'target/classes'이다.
    - <B>\<testOutputDirectory\></B>: 컴파일 결과물의 위치를 지정할 수 있다. 지정하지 않을 시 'target/test-classes'이다.
    - <B>\<repository\></B>: 빌드 시 접근할 저장소의 주소를 지정할 수 있다. 지정하지 않을 시 기본적으로 [Maven 중앙 저장소](https://repo1.maven.org/maven2/)이다.
    - <B>\<plugin\></B>: Maven의 플러그인을 설정할 수 있다. 가장 중요한 기능이라고 할 수 있다.
        - Maven은 플러그인 기반으로 동작한다. 아래 나올 Maven의 phase도 플러그인 기반으로 작동한다.
        - 들어가는 옵션을 제각각이지만 플러그인 형식에 대한 안내가 다양한 곳에 나와있으니 참고해서 작성하면 된다.


### Maven Lifecycle
---
Maven에서는 빌드 순서가 정해져 있고 각각의 단계를 phase라고 한다. 각각의 phase는 서로 의존 관계를 가지고 있기 때문에 이전의 phase가 완료되지 않으면 다음 phase를 수행할 수 없다.


또한 plugin에서 실행할 수 있는 각각의 작업을 골(goal)이라고 한다. 하나의 페이즈는 하나의 골과 연결되고 하나의 플러그인은 여러 골을 가질 수 있다.


아래는 Maven의 라이프사이클이다.
- Clean: 이전 빌드에서 생성된 파일들을 삭제하는 단계
- Validate: 프로젝트가 올바른지 확인학고 필요한 모든 정보를 사용할 수 있는 지 확인하는 단계
- Compile: 프로젝트의 소스코드를 컴파일하는 단계
- Test: 유닛(단위) 테스트를 수행하는 단계(테스트 실패 시 빌드 실패로 처리, 스킵 가능)
- Package: 실제 컴파일된 소스 코드와 리소스들을 jar등의 배포를 위한 패키지로 만드는 단계
- Verify: 통합테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계
- Install: 패키지를 로컬 저장소에 설치하는 단계
- Site: 프로젝트 문서를 생성하는 단계
- Deploy: 만들어진 Package를 원격 저장소에 release하는 단계

위 phase 외에도 더 많은 phase가 존재하고 maven의 lifecycle은 크게 clean(빌드 시 생성되었던 산출물 제거), build(=Default, 프로젝트 배포 절차, 패키지 타입 별로 다르게 정의), site(프로젝트 문서화 절차)의 단계로 구분된다.  
모든 phase가 한 번에 수행되어야 하는 것은 아니고 특정 phase까지만 수행도 가능하다.


## Gradle(그래들, 그레이들)
기존에 존재하던 Ant 빌드 파일의 단점을 보완해 2012년에 출시되었고 안드로이드 앱을 만들 때 필요한 공식 빌드 관리 시스템으로 선정되었다. 

[공식 홈페이지](https://docs.gradle.org/current/userguide/what_is_gradle.html)를 보면 Gradle에 대해 알아야 할 5가지 사항을 명시하고 있다.
1. <B>Gradle is a general-purpose build tool(범용적인 빌드 도구)</B>: Gradle은 어떤 소프트웨어에서도 빌드가 가능하도록 돕는다.
2. <B>The core model is based on tasks(핵심 모델은 작업에 기반하고 있음)</B>: Gradle에서는 거의 모든 빌드 프로세스를 DAG라는 그래프로 모델링하는데, 이 부분이 Gradle을 유연하게 만들어주는 이유라고 한다(읽어봤는지 솔직히 이해하지 못했다..ㅋㅋ)
3. <B>Gradle has several fixed build phases(몇 가지 고정된 필드 phase를 갖고 있음)</B>: Initialization(초기화) - Configuration(구성) - Execution(실행) 단계로 이루어져있고, 이 과정은 Gradle의 빌드 Lifecycle을 형성한다.
4. <B>Gradle is extensible in more ways than one(여러 가지의 확장 가능성)</B>
5. <B>Build scripts operate against an API(API에 기반해 빌드 스크립트 동작)</B>

---
위 특징들 외에 추가적인 특징은 아래와 같다.
- Maven을 사용할 수 있는 변환 가능 프레임워크
- 멀티 프로젝트 높은 사용성: 설정 주입 방식을 사용
- Apache Ivy에 기반한 강력한 의존성 관리
- Maven repository와 Ivy repository 지원
- Groovy 문법 사용: 동적인 빌드 가능

Gradle의 장점을 요약하면 높은 호환성과 가시성, 빠른 빌드 속도이라고 할 수 있다.

```groovy
plugins {
    id 'org.springframework.boot' version '2.4.1'
    id 'io.spring.dependency-management' version '1.0.10.RELEASE'
    id 'java'
}

group 'com.test'
version '1.0.4-SNAPSHOT-'+new Date().format("yyyyMMddHHmmss")
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

test {
    useJUnitPlatform()
}

dependencies {
    implementation('org.springframework.boot:spring-boot-starter-web')
    implementation('org.springframework.boot:spring-boot-starter-mustache')

    implementation('org.projectlombok:lombok')
    annotationProcessor('org.projectlombok:lombok')
    testImplementation('org.projectlombok:lombok')
    testAnnotationProcessor('org.projectlombok:lombok')

    implementation('org.springframework.boot:spring-boot-starter-data-jpa')
    implementation("org.mariadb.jdbc:mariadb-java-client")
    implementation('com.h2database:h2')

    implementation('org.springframework.boot:spring-boot-starter-oauth2-client')

    testImplementation('org.springframework.boot:spring-boot-starter-test')
}
```
위에는 실제로 스프링 부트 프로젝트에서 사용하고 있는 `build.gradle` 파일 예시이다.


이전에 봤던 Maven의 여러 태그가 들어갔던 것에 비해 훨씬 가독성이 높다.  
프로젝트가 커지면 커질 수록 이런 장점은 점점 더 드러나지 않을까 싶다.


## Maven vs Gradle
Gradle이 Maven에 비해 뒤에 출시되었기 때문에 더 많은 장점을 갖고 있는 것 같다.  
Gradle 공식 사이트에서는 자신있게 Maven과 Gradle을 비교하는 [공식 문서](https://gradle.org/maven-vs-gradle/)가 있는 것을 보면 자신감이 넘치는 것 같다.


Gradle이 Maven 보다 좋은 점은 구체적으로 무엇이 있을까?  
아래에서 하나씩 살펴보자.
- Gradle은 groovy 문법을 사용하기 때문에 동적인 빌드가 가능하다. 설정 주입 시 프로젝트 별로 주입되는 설정을 다르게 할 수 있다. 반면에 Maven에서는 동적인 요소를 XML로 정의하기에는 어려운 부분이 많다. 불가능한 것은 아니지만 설정 내용이 복잡하고 상속 구조를 통한 멀티 모듈을 구현해야 한다는 점에서 상대적으로 비효율적이다.
- 빌드 속도는 Gradle이 기본적으로 2배, 최대 100배까지 빠르다고 한다. 이유는 총 3가지가 있다.
    - 작업의 입력과 출력을 추적해 필요한 것만 실행한다. 가능한 경우 변경된 파일만 처리해 불필요한 작업을 최소화한다.
    - build cache를 사용할 경우 다른 Gradle 빌드의 빌드 출력을 재사용한다. 참고로 다른 글들을 찾아보면 대부분 cache를 사용하기 때문에 빠르다고 하던데 아래 그래프를 보면 cache를 사용하지 않더라도 빌드 속도가 더 빠름을 알 수 있다. cache를 사용할 경우 엄청난 속도로 빌드가 된다.
![comparison](\assets\images\posts\2022-04-06\gradleVSmaven.png){: .align-center}
    - 프로세스가 빌드 정보를 hot한 상태로 오랫동안 유지(long-lived)된다.

---
그렇다면 Gradle 보다 더 나은 Maven의 장점은 없을까?


공식 문서를 찾아보니 IDE를 통해 작업할 때 Maven은 Gradle에 비해 많은 기능(자동 완성 등)이 지원된다고 한다.  
초기에는 Gradle을 IDE에서 사용하기 불편했을 것 같다.  
공식 문서가 언제 쓰여졌는지 확인하기 어려워서 정확히는 모르겠지만 Gradle이 나온지 10년이나 됐기 때문에 이 부분은 더 이상 큰 장점이 아닌 것 같다.


또 다른 장점으로는 Maven의 커뮤니티가 아직 더 크다는 것이다. 
Gradle도 점점 커뮤니티가 커지는 중이지만 아직까지는 Maven의 점유율이 더 높고 참고할 문서의 양도 더 많기 때문에 이는 큰 장점이라고 할 수 있다.


## 결론
Gradle의 점유율이 점점 올라가는만큼 Gradle을 제대로 아는 것은 필수이다.


하지만 기존에 Maven으로 개발된 프로젝트나 현재 개발되고 있는 많은 프로젝트들도 Maven으로 개발되고 있기 때문에 Maven에 대해 제대로 아는 것도 필수이다.


또한 언젠가 레거시를 대체할 기회를 얻게  된다면 둘 다 알아야 가능하니 둘 다 잘 배워두자!



## 추후 공부해볼 부분
maven과 gardle에 대해서 공부해봤는데 아무래도 Ant에 대해서는 전혀 알지 못 하고 있기 때문에 이들의 등장 배경을 보고 '아 그렇구나..' 정도로만 생각하게 됐다.

또한 막상 maven의 플러그인이 굉장히 중요하다고 썼지만 어떤 방식으로 동작하는지 정확히는 알지 못한다.


위에 적은 부분들을 공부해봐야겠다.


## 참고 자료
- [https://maven.apache.org/](https://maven.apache.org/what-is-maven.html)
- [https://docs.gradle.org/](https://docs.gradle.org/current/userguide/what_is_gradle.html)
- [https://jeong-pro.tistory.com/](https://jeong-pro.tistory.com/168?category=793347)
- [https://dev-coco.tistory.com/](https://dev-coco.tistory.com/65)
- [https://jisooo.tistory.com/](https://jisooo.tistory.com/entry/Spring-%EB%B9%8C%EB%93%9C-%EA%B4%80%EB%A6%AC-%EB%8F%84%EA%B5%AC-Maven%EA%B3%BC-Gradle-%EB%B9%84%EA%B5%90%ED%95%98%EA%B8%B0)
- [https://hyojun123.github.io/](https://hyojun123.github.io/2019/04/18/gradleAndMaven/)