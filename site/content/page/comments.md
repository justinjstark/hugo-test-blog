---
title: Comments
---
<form method="POST" action="https://api.staticman.net/v2/entry/justinjstark/blog-comments/master/comments">
  <input name="options[redirect]" type="hidden" value="https://justinjstark.com">
  <!-- e.g. "2016-01-02-this-is-a-post" -->
  <input name="options[slug]" type="hidden" value="testslug">
  <label><input name="fields[name]" type="text">Name</label>
  <label><input name="fields[email]" type="email">E-mail</label>
  <label><textarea name="fields[message]"></textarea>Message</label>
  
  <button type="submit">Go!</button>
</form>
