---
title: Spring Boot拦截器配置
date: 2021-10-16 15:30:08
tags: [后端]
---

1、启用拦截器配置

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.easiweb.core.interceptor.RequestInterceptor;

@Configuration
public class InterceptorConfig implements WebMvcConfigurer {

	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(new RequestInterceptor()).addPathPatterns("/**");
	}
}
```

2、在拦截器里做请求校验

```java
import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import com.easiweb.core.utils.Result;
import com.fasterxml.jackson.databind.ObjectMapper;

@Component
public class RequestInterceptor implements HandlerInterceptor {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		String token = request.getHeader("token");

		if (token == null) { // 根据业务编写逻辑
			getResponse(response, Result.error("token为空"));
			return false;
		} else {
			return true;
		}
	}

	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {

	}

	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {

	}

	/**
	 * 构造响应消息体
	 *
	 * @param result
	 * @param response
	 * @throws IOException
	 */
	private void getResponse(HttpServletResponse response, Result result) throws IOException {
		response.setHeader("Access-Control-Allow-Origin", "*");
		response.setHeader("Cache-Control", "no-cache");
		response.setCharacterEncoding("UTF-8");
		response.setContentType("application/json");
		ObjectMapper objectMapper = new ObjectMapper();
		response.getWriter().println(objectMapper.writeValueAsString(result));
		response.getWriter().flush();
	}

}
```