# WebMvcConfigurer

**常用的方法：**

```java
 /* 拦截器配置 */
void addInterceptors(InterceptorRegistry var1);
/* 视图跳转控制器 */
void addViewControllers(ViewControllerRegistry registry);
/**
*静态资源处理
**/
void addResourceHandlers(ResourceHandlerRegistry registry);
/* 默认静态资源处理器 */
void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer);
/**
* 这里配置视图解析器
 **/
void configureViewResolvers(ViewResolverRegistry registry);
/* 配置内容裁决的一些选项*/
void configureContentNegotiation(ContentNegotiationConfigurer configurer);
/** 解决跨域问题 **/
public void addCorsMappings(CorsRegistry registry) ;
/** 参数解析 **/
void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers)

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        super.addInterceptors(registry);
        registry.addInterceptor(new TestInterceptor()).addPathPatterns("/**").excludePathPatterns("/emp/toLogin","/emp/login","/js/**","/css/**","/images/**");
    }

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/toLogin").setViewName("login");
    }


    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**","/**/**").addResourceLocations("classpath:/static/");
    }


    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
            configurer.enable();
            configurer.enable("defaultServletName");
    }
    /**
     * 配置请求视图映射
     * @return
     */
    @Bean
    public InternalResourceViewResolver resourceViewResolver()
    {
        InternalResourceViewResolver internalResourceViewResolver = new InternalResourceViewResolver();
        //请求视图文件的前缀地址
        internalResourceViewResolver.setPrefix("/WEB-INF/jsp/");
        //请求视图文件的后缀
        internalResourceViewResolver.setSuffix(".jsp");
        return internalResourceViewResolver;
    }

    /**
     * 视图配置
     * @param registry
     */
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        super.configureViewResolvers(registry);
        registry.viewResolver(resourceViewResolver());
        /*registry.jsp("/WEB-INF/jsp/",".jsp");*/
    }

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        super.addCorsMappings(registry);
        registry.addMapping("/cors/**")
                .allowedHeaders("*")
                .allowedMethods("POST","GET")
                .allowedOrigins("*");
    }

    /**
    * 消息内容转换配置
     * 配置fastJson返回json转换
     * @param converters
     */
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        //调用父类的配置
        super.configureMessageConverters(converters);
        //创建fastJson消息转换器
        FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();
        //创建配置类
        FastJsonConfig fastJsonConfig = new FastJsonConfig();
        //修改配置返回内容的过滤
        fastJsonConfig.setSerializerFeatures(
                SerializerFeature.DisableCircularReferenceDetect,
                SerializerFeature.WriteMapNullValue,
                SerializerFeature.WriteNullStringAsEmpty
        );
        fastConverter.setFastJsonConfig(fastJsonConfig);
        //将fastjson添加到视图消息转换器列表内
        converters.add(fastConverter);

    }
	 /**
	 * 添加参数注解解析
	 */
	  @Override
            public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
                resolvers.add(new QueryArgumentResolver());
            }

class QueryArgumentResolver implements HandlerMethodArgumentResolver{
     @Override
    public boolean supportsParameter(@NonNull MethodParameter parameter) {
        return parameter.hasParameterAnnotation(QueryRequestParam.class);
    }
      @Override
    public Object resolveArgument(@NonNull MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
// 从请求参数中取出实际类型，这里还可以做写额外操作，先简单的实现功能
       
         HttpServletRequest httpServletRequest = webRequest.getNativeRequest(HttpServletRequest.class);
        // 创建实例
    }
}
```



[^SpringBoot---WebMvcConfigurer详解]: https://blog.csdn.net/zhangpower1993/article/details/89016503

