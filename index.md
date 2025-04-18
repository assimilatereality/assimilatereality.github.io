
![Image of fast.ai logo](images/chess.jpeg)

# Effecting Affordances

{% for post in site.posts %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p class="post-date">{{ post.date | date: "%B %d, %Y" }}</p>
  <p>{{ post.excerpt }}</p>
  <a href="{{ post.url }}">Read more...</a>
  <hr>
{% endfor %}
