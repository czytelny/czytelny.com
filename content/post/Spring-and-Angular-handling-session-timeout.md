+++
date = "2015-12-07T19:30:04+01:00"
tags = ["AngularJS", "Spring", "Java", "Javascript"]
title = "Spring and Angular - handling session timeout"

+++

*"This should be trivial one"* - that was my first thought when I encountered issue with session timeout in a Spring based webapp. By default Spring Security send `redirect` in response to a request which session have expiried. However in webapp using [ui-router](`https://github.com/angular-ui/ui-router`) in Angular you can experience unwanted artefacts such as login page partially nested inside other view...
In other words - it doesn't work as expected.

My first thought was to use Angular's interceptor to catch somehow `redirect` which Spring send to me. But because it's handled by a browser so I can't easily manipulate this as it's outside Angulars scope.

Digging in a web lead me to following steps:
## Add a custom entry point
Which extends `LoginUrlAuthenticationEntryPoint` to overwrite default behaviour and send regular error code `UNAUTHORIZED (401)`.

```java
@Configuration
public class CustomEntryPoint extends LoginUrlAuthenticationEntryPoint {
    private static final String XML_HTTP_REQUEST = "XMLHttpRequest";
    private static final String X_REQUESTED_WITH = "X-Requested-With";
    private static final String LOGIN_URL = "/login.jsp";

    public CustomEntryPoint() {
        super(LOGIN_URL);
    }

    public CustomEntryPoint(String loginFormUrl) {
        super(loginFormUrl);
    }

    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception)
            throws IOException, ServletException {

        if (isAjax(request) && isFromRunningApp(request)) {
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
        } else {
            super.commence(request, response, exception);
        }
    }

    private boolean isAjax(HttpServletRequest request) {
        return XML_HTTP_REQUEST.equals(request.getHeader(X_REQUESTED_WITH));
    }

    private boolean isFromRunningApp(HttpServletRequest request) {
        return (request.getRequestedSessionId() != null || !request.isRequestedSessionIdValid());
    }
}
```

## Change spring security settings
To use this newly created custom entry point

```xml
    <http auto-config="true" use-expressions="true" entry-point-ref="customEntryPoint">
    	(...)
    </http>
```

## Modify angular app
To attach specific header `X-Requested-With` to `XMLHttpRequest` so I can easily distinguish an AJAX request. In some old version this was by default, now I need to easilt set it up by in a config:

```javascript
myAppModule.config(['$httpProvider', function($httpProvider) {
    $httpProvider.defaults.headers.common["X-Requested-With"] = 'XMLHttpRequest';
}]);
```


### What's left ?
Add standard interceptor which handle 401. It's piece of cake so I won't write it here :)
