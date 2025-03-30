# head-dump

## Description
Welcome to the challenge! In this challenge, you will explore a web application and find an endpoint that exposes a file containing a hidden flag.The application is a simple blog website where you can read articles about various topics, including an article about API Documentation. Your goal is to explore the application and find the endpoint that generates files holding the server’s memory, where a secret flag is hidden.The website is running [picoCTF News](http://verbal-sleep.picoctf.net:64167/).
## Solve
1. First we connect to the server with the given link.

2. Based on the name of the challenge, it seems like it might involve analyzing a heap dump from the site.

3. Let's try to use use heapdump as an endpoint in the URL: http://verbal-sleep.picoctf.net:64167/heapdump

4. A file .heapsnapshot is downloaded in our system, so let's see the content:
```sh
┌──(xrengariox㉿PC)-[~/Downloads]
└─$ file heapdump-1743312578447.heapsnapshot 
heapdump-1743312578447.heapsnapshot: ASCII text, with very long lines (826)

┌──(xrengariox㉿PC)-[~/Downloads]
└─$ cat heapdump-1743312578447.heapsnapshot | grep "pico"
picoCTF{Pat!3nt_15_Th3_K3y_439bb394}
```

5. And we got the flag:
```flag
picoCTF{Pat!3nt_15_Th3_K3y_439bb394}
```

## Additional Notes 

## References
