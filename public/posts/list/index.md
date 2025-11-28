# 


<div class="container">
    <div class="page home posts" style="background: transparent;">
        
        <!-- 页面标题 -->
        <h2 class="single-title">{{ .Title }}</h2>

        <!-- 关键魔法：直接调用 FixIt 主题写好的 summary 视图 -->
        {{ range .Paginator.Pages }}
            {{ .Render "summary" }}
        {{ end }}

        <!-- 分页器 -->
        {{ partial "paginator.html" . }}
    </div>
</div>


---

> 作者:   
> URL: http://localhost:1313/posts/list/  

