---
layout: post
title:  "Using forms in Netlify"
date:   2019-07-01 15:44:00 -0500
permalink: /forms-in-netlify/
---
The features that Netlify provides always surprise me. A cool feature Netlify provides is support for custom forms. You just include Netlify provided form markup in your page and you are done. Whenever someone on your website submits information using this form, Netlify will automatically provide the information they submitted on the Netlify dashboard. What's more Netlify also provides some advanced form features like reCAPTCHA, file upload, exporting form notifications to csv, integration with Slack and Zapier etc.

I have used this feature to create a contact form for my [contact](/contact) page. Here is the recipe to do this.

### Include form markup in your page.

Include the Netlify provided form markup in your desired page. I have used this code

```html
<form name="contact" method="POST" data-netlify="true">
  <p>
    <label style="font-family: monospace;">Name<br><input style="width: 100%;" type="text" name="name" /></label>   
  </p>
  <p>
    <label style="font-family: monospace;">Email<br><input style="width: 100%;" type="email" name="email" /></label>
  </p>
  <p>
    <label style="font-family: monospace;">Message<br><textarea style="width: 100%;" rows="15" name="message"></textarea></label>
  </p>
  <div data-netlify-recaptcha="true"></div>
  <br>
  <p>
    <button class="send-button" type="submit">Send</button>
  </p>
</form>
```

Note the use of data-netlify="true" in the form tag. This tells Netlify to show form submissions on the Netlify dashboard. Also all the fields should be provided with the name attribute.

Finally I am including reCAPTCHA support for spam protection using `<div data-netlify-recaptcha="true"></div>`.

### Connect to Zapier

Whenever a user submits a form, you may want to be notified using email. Netlify provides an [integration](https://zapier.com/app/editor/template/29683?utm_campaign=Widget&embedded=true&referrer=https%3A%2F%2Fwww.netlify.com%2Fdocs%2Fform-handling%2F&utm_source=widget&utm_medium=embed&selected_apis=NetlifyDevAPI%2CGoogleMailV2API&create=true) with Zapier for this purpose. You will need to configure this integration to use the desired 'to' address. You can also configure this integration in such a way that it will use the form name, email and message that the user has submitted.

### Miscellaneous

You can also use AJAX form submissions. Netlify also allows the form to be used in libraries like React and Vue

The documentation for Netlify Forms can be found [here](https://www.netlify.com/docs/form-handling/).