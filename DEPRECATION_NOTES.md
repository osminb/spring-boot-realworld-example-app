# Spring Boot 2.7.18 Upgrade - Deprecation Warnings

This document lists deprecation warnings discovered during the Spring Boot upgrade from 2.6.3 to 2.7.18, which need to be addressed in the future Spring Boot 3.x migration.

## Major Deprecations

### 1. WebSecurityConfigurerAdapter (Critical for Spring Boot 3.x)

**Location:** `src/main/java/io/spring/api/security/WebSecurityConfig.java:23`

**Issue:** The class extends `WebSecurityConfigurerAdapter` which is deprecated in Spring Security 5.7+ and removed in Spring Security 6.0 (Spring Boot 3.x).

**Current Code:**
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // Security configuration
    }
}
```

**Future Fix for Spring Boot 3.x:**
Replace with SecurityFilterChain bean approach:
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        // Move existing configure() logic here
        return http.build();
    }
}
```

## Compilation Warnings

During compilation with Spring Boot 2.7.18, the following warnings were observed:

1. **Deprecated API Usage:** Some input files use or override deprecated APIs
2. **Unchecked Operations:** GraphQLCustomizeExceptionHandler.java uses unchecked or unsafe operations

## Dependencies Updated

- **Spring Boot:** 2.6.3 → 2.7.18
- **MyBatis Spring Boot Starter:** 2.2.2 → 2.3.2 (both main and test dependencies)

## Dependencies Kept (Verified Compatible)

- **Netflix DGS GraphQL:** 4.9.21 (tested and working with Spring Boot 2.7.18)
- **JJWT:** 0.11.2 (compatible with Spring Boot 2.7)
- **REST Assured:** 4.5.1 (compatible with Spring Boot 2.7)

## Testing Results

✅ **Compilation:** Successful with `./gradlew compileJava`
✅ **Tests:** All 66 tests pass with `./gradlew test`
✅ **Application Startup:** Successful with `./gradlew bootRun`
✅ **REST API:** `/tags` endpoint returns `{"tags":[]}`
✅ **GraphQL API:** Query `{ tags }` returns `{"data":{"tags":[]}}`

## Next Steps for Spring Boot 3.x Migration

1. Replace WebSecurityConfigurerAdapter with SecurityFilterChain
2. Update to Spring Security 6.0+ compatible configuration
3. Address javax to jakarta namespace migration
4. Update Netflix DGS to Spring Boot 3.x compatible version
5. Review and update all deprecated API usages
6. Test thoroughly with Spring Boot 3.x

## Notes

- This upgrade maintains Java 11 compatibility
- All existing functionality preserved
- No breaking changes introduced
- Application ready for production use with Spring Boot 2.7.18
