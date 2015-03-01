---
layout: post
title: You're up and running!
---

This post is a temporary placeholder for the real blog post on how to get a RouteData instance from an arbitrary URL. This post got a lot of traffic from StackOverflow and I’m in the process of changing my website around (a lot). I’m just putting the relevant code here for the benefit of folks who are coming from SO and will re-write this post shortly to contain a full description of the implementation.

{% highlight csharp %}
public class RouteInfo  
{  
    public RouteData RouteData { get; private set; }  
  
    public RouteInfo(RouteData data)  
    {  
        RouteData = data;  
    }  
  
    public RouteInfo(Uri uri, string applicationPath)  
    {  
        RouteData = RouteTable.Routes.GetRouteData(new InternalHttpContext(uri, applicationPath));  
    }  
  
    private class InternalHttpContext : HttpContextBase  
    {  
        private readonly HttpRequestBase _request;  
  
        public InternalHttpContext(Uri uri, string applicationPath)  
        {  
            _request = new InternalRequestContext(uri, applicationPath);  
        }  
  
        public override HttpRequestBase Request { get { return _request; } }  
    }  
  
    private class InternalRequestContext : HttpRequestBase  
    {  
        private readonly string _appRelativePath;  
        private readonly string _pathInfo;  
  
        public InternalRequestContext(Uri uri, string applicationPath)  
        {  
            _pathInfo = uri.Query;  
  
            if (String.IsNullOrEmpty(applicationPath) || !uri.AbsolutePath.StartsWith(applicationPath, StringComparison.OrdinalIgnoreCase))  
                _appRelativePath = uri.AbsolutePath.Substring(applicationPath.Length);  
            else  
                _appRelativePath = uri.AbsolutePath;  
        }  
  
        public override string AppRelativeCurrentExecutionFilePath { get { return String.Concat("~", _appRelativePath); } }  
        public override string PathInfo { get { return _pathInfo; } }  
    }  
}  
{% endhighlight %}

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
