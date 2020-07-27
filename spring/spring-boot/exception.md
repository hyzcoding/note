## 自定义异常

```java
@Getter
public class CustomException extends RuntimeException {
    private int code;
    private String message;

    public CustomException(int code, String message) {
        this.code = code;
        this.message = message;
    }

}
```

## 异常捕获

```java
@ControllerAdvice
@Slf4j
public class GlobalExceptionHandler {
    @ResponseBody
    @ExceptionHandler(CustomException.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public JSONObject handleCustomException(CustomException e) {
        JSONObject response = new JSONObject();
        return response;
    }
}
```

## 默认异常

| Spring异常                              |          HTTP状态码          |
| --------------------------------------- | :--------------------------: |
| BindException                           |      400 - Bad Request       |
| ConversionNotSupportedException         | 500 - Internal Server Error  |
| HttpMediaTypeNotAcceptableException     |     406 - Not Acceptable     |
| HttpMediaTypeNotSupportedException      | 415 - Unsupported Media Type |
| HttpMessageNotReadableException         |      400 - Bad Request       |
| HttpMessageNotWritableException         | 500 - Internal Server Error  |
| HttpRequestMethodNotSupportedException  |   405 - Method Not Allowed   |
| MethodArgumentNotValidException         |      400 - Bad Request       |
| MissingServletRequestParameterException |      400 - Bad Request       |
| MissingServletRequesPartException       |      400 - Bad Request       |
| NoSuchRequestHandlingMethodException    |       404 - Not Found        |
| TypeMismatchException                   |      400 - Bad Request       |

