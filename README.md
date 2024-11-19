# Comento Laravel Style Guide
- 이 문서는 코멘토에서 사용하는 라라벨 스타일 가이드를 정리하기 위한 문서입니다.
- 스타일 가이드에 정답은 없으며 Laravel, PHP의 버전과 논의를 통해 지속적으로 변경됩니다.

## 목차
- [들어가기](#들어가기)
- [컨벤션](#컨벤션)
- [참조](#참조)

## 들어가기

### 환경
- PHP 8.3
- Laravel 10.x

### 기본 규칙
- 기본적인 규칙은 [PSR-12](https://www.php-fig.org/psr/psr-12) 를 적용합니다.

## 컨벤션

### Routing

- 라우트 정의는 컨트롤러와 메소드를 이용합니다.
- 컨트롤러를 입력할 때는 전체 경로를 입력하지 않고 `use`를 사용합니다.
- 컨트롤러의 이름이 동일하거나, 제품의 폴더 안에 들어가 있어 단순해진 이름으로 인해 오해의 소지가 있을 경우에는 `as` 를 사용해서 클래스의 성격 및 용도를 명확하게 해서 사용합니다.

```php
use App\Http\Controllers\UserController;
use App\Http\Controllers\Api\Jwt\V2\EduCamp\LiveClassController as EduLiveClassController;

Route::get('/user', [UserController::class, 'index']);
Route::post('ready/{eduCamp}', [EduLiveClassController::class, 'readyToLiveClass']);
```

- Route URI의 구조가 Controller 이름, Method가 되도록 구성합니다.

```php
Route::prefix('coupon')->group(function () {
    Route::post('/download/code', [CouponController::class, 'downloadCode']);
})

// 제품 단위가 클 경우 Controller를 세부 단위까지 분리해서 작성합니다.
Route::prefix('vod')->group(function () {
    Route::post('check/paid', [VodCheckController::class, 'paid']);
    // ...
  
    Route::post('note/list', [VodNoteController::class, 'list']);
    // ...
}
```

### Route URI Naming

- 다음의 일관성을 지킵니다.
  - 케밥 케이스(kebab-case)로 작성: 가독성을 위해 하이픈(-)을 사용합니다.
  - 밑줄 (_)은 사용하지 않습니다.
  - 소문자를 사용합니다.

### Route Structure

- 제품 단위가 커지면 파일을 분리합니다.
- 분리한 파일은 routes/resources 폴더에 저장합니다.
```php
// before
Route::prefix('vod')->middleware(['auth.jwt'])->group(function () {
    // ...
});

// after
Route::prefix('vod')->middleware(['auth.jwt'])->group(
    base_path('routes/resources/vod.php'),
);
```

### Validation

- Validation은 `Validator::make`를 사용합니다.
  - Form Request를 사용하지 않는 이유는 현재 잘 사용하기 위한 정책이 아직 마련되지 않았기 때문입니다.
  - 그리고 유지보수 측면으로도 컨트롤러에서 확인하는게 더 빠르기 때문입니다.
- `Valitator::make` 아래에는 `fails()` 를 이용해서 early return을 합니다.

```php
$validator = Validator::make($request->all(), [
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
]);

if ($validator->fails()) {

}
```

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

## Laravel Pint 적용하기

### 설치
```
composer require laravel/pint --dev
```

### Pint.json 파일 및 설정법
- [Laravel Pint 적용하기](https://www.notion.so/comento/Laravel-Pint-c34ad012fa1544caa1c09f4c2b9d12c0?pvs=4)

## 참조

### 다른 곳에서 공유하는 스타일 가이드
- [Spatie Laravel & PHP Style Guide](https://github.com/spatie/guidelines.spatie.be/blob/master/content/code-style/laravel-php.md#typed-properties)
- [OP.GG 스타일 가이드](https://github.com/opgginc/styleguide/blob/master/laravel.md)
- ~~[ESC Company 가이드](https://helloworld.holapet.com/php-coding-guidelines)~~

### 읽어보면 좋은 문서
- [PHP The Right Way](http://modernpug.github.io/php-the-right-way/)
- 공식 [Laravel 문서](https://laravel.com/docs/)
- 한글 [Laravel 문서](https://laravel.kr/docs/)
