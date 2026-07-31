---
title: "Contact"
url: "/contact/"
---

<form name="contact" method="POST" data-netlify="true" netlify-honeypot="bot-field">
  <input type="hidden" name="form-name" value="contact">
  <p hidden>
    <label>Don't fill this out: <input name="bot-field"></label>
  </p>
  <p>
    <label>Your name
      <input type="text" name="name" required>
    </label>
  </p>
  <p>
    <label>Your email
      <input type="email" name="email" required>
    </label>
  </p>
  <p>
    <label>Your organization
      <input type="text" name="organization" required>
    </label>
  </p>
  <p>
    <label>Tell us about your use of the A2O and its data
      <textarea name="message" rows="8"></textarea>
    </label>
  </p>
  <p>
    <label>
      <input type="checkbox" name="consent" value="yes">
      I consent to a follow up email to gather necessary information
    </label>
  </p>
  <p>
    <button type="submit">Submit</button>
  </p>
</form>