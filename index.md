Hi, I'm Hyeok. I will share my thoughts and notes (mostly about codes).

# Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
