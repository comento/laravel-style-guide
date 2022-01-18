# Comento Laravel Style Guide
- 이 문서는 코멘토에서 사용하는 라라벨 스타일 가이드를 정리하기 위한 문서입니다.
- 스타일 가이드에 정답은 없으며 Laravel, PHP의 버전과 논의를 통해 지속적으로 변경됩니다.

## 목차
- [들어가기](#들어가기)
- [참조](#참조)

## 들어가기

### 환경
- PHP 7.4
- Laravel 7.x

### 기본 규칙
- 기본적인 규칙은 [PSR-12](https://www.php-fig.org/psr/psr-12) 를 적용합니다.

## 컨벤션

### Early returns
- 예측가능하고 제어가 가능한 예외사항은 `throw` 대신에 `early return`을 사용한다.
- 최대한 `else`의 사용을 피한다.
- 가급적 `if` 조건에는 부정 (`!`) 연산자를 사용하지 않는다.

### Json Response
- json response는 `response()->json()` 형태를 사용합니다.
- status code는 직관성을 위해 위해 `JsonResponse`를 사용합니다.

```php
return response()->json(
    [
        'message' => 'ok',
        'data' => $data,
    ],
    JsonResponse::HTTP_OK
);
```

## 참조

### 다른 곳에서 공유하는 스타일 가이드
- [Spatie Laravel & PHP Style Guide](https://github.com/spatie/guidelines.spatie.be/blob/master/content/code-style/laravel-php.md#typed-properties)
- [OP.GG 스타일 가이드](https://github.com/opgginc/styleguide/blob/master/laravel.md)
- [ESC Company 가이드](https://helloworld.holapet.com/php-coding-guidelines)

### 읽어보면 좋은 문서
- [PHP The Right Way](http://modernpug.github.io/php-the-right-way/)
- 공식 [Laravel 문서](https://laravel.com/docs/)
- 한글 [Laravel 문서](https://laravel.kr/docs/)