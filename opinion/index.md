---
layout: home
---

<div class="index-content opinion">
    <div class="section">
        <ul class="artical-cate">
            <li><a href="/"><span>软件技术</span></a></li>
            <li class="on" style="text-align:center"><a href="/opinion"><span>摄影</span></a></li>
            <li style="text-align:right"><a href="/project"><span>推荐文章</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <ul class="artical-list">
        {% for post in site.categories.opinion %}
            <li>
                <h2>
                    <a href="{{ post.url }}">{{ post.title }}</a>
                </h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
