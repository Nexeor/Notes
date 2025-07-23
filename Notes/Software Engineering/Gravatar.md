2025-06-11 14:30

Status: #Leaf

Tags: #WebDev #API 

# Gravatar
Gravatar, or **Globally Recognized Avatar**, is a service that associates users with a consistent avatar image. It allows the user to have an avatar that is consistent across many websites. 
1) Create an account on Gravatar and upload an image
2) Link the image to an email address
3) Gravatar stores a hashed version (MD5 hash) of our email
4) Sites request Gravatar for the image using our hashed email 

We can also use Gravatar to request a users avatar if we have their email. 
- Request: `https://www.gravatar.com/avatar/<hash>` where `<hash>` is the MD5 hash of the user's email 
- Arg `s`: Choose the size of the returned image (in pixels)
	- By default, Gravatar returns a 80x80 image
- Arg `d`: Choose what image to display for unregistered users
	- `d=indenticon` provides a unique geometric design
	- ![[Pasted image 20250611144151.png]]

We can request the avatar for a user with email `john@example.com`:
```python
>>> from hashlib import md5
>>> 'https://www.gravatar.com/avatar/' + md5(b'john@example.com').hexdigest()
'https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6'
```
This is the same request but returns a 60x60 image and `d=identicon`
```python
>>> from hashlib import md5
>>> 'https://www.gravatar.com/avatar/' + md5(b'john@example.com').hexdigest()
'https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?d=identicon&s=60'
```
## References
