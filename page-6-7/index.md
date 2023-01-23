# [Mastering Spring 5.0] 6.7 국제화



## 국제화
> 국제화는 다양한 언어 및 문화권에 에서 사용할 수 있는 컨텐츠를 제공할 수 있도록 어플리케이션을 작성하는 것을 의미한다. **국제화(internationalization)** 를 **I18N**이나 **i18n**으로, **현지화 localization**를 **L10N**이나 **l10n** 등으로 표기하기도 한다. 스프링 부트는 국제화를 위한 지원 기능을 내장하고 있다.

## 스프링부트프로젝트에서 국제화 지원설정하기
### `Application.java` 에 `LocaleResolver` 추가

```java
@Bean
public SessionLocaleResolver localResolver() {
    SessionLocaleResolver sessionLocaleResolver = new SessionLocaleResolver();
    sessionLocaleResolver.setDefaultLocale(Locale.US);
    return sessionLocaleResolver;
}
    
@Bean
public ResourceBundleMessageSource messageSource (){
    ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
    messageSource.setBasename("i18n/message");
    messageSource.setUseCodeAsDefaultMessage(true);
    return messageSource;
}
```
+ `sessionLocaleResolver.setDefaultLocale(Locale.US)` : 기본 로케일을 `Locale.US` 로 설정한다.

+ `messageSource.setBasename("i18n/messages")` : 메세지 소스의 기본이름을 **message** 로 설정한다. 예를들어 message_kr.properties 지정할 수 있으며, `message_kr.properties` 를 사용할 수 없으면 `message.properties` 에서 검색된다.

+ `messageSource.setUseCodeAsDefaultMessage(true)` : 메세지를 찾을 수 없는 경우 코드는 기본 메세지를 반환한다.

### 국제화 메세지 파일 생성하기
```properties
# message.properties
welcome.message=Welcome in English

# message_fr.properties
welcome.message=Welcome in French
```
