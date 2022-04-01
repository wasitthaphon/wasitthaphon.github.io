---
layout: post
title: "JSON Web Token"
date: 2022-04-01 08:18:00 +0700
categories: Jwt
---

## JSON Web Token (JWT)

JWT เป็น JSON object ที่ประกอบไปด้วยข้อมูลไว้ใช้สำหรับการ verify ตัว JWT เป็นลายเซ็นดิจิทัลที่สร้าง
โดยเซิฟเวอร์ที่เราร้องขอเพราะตัว JWT จะถูกสร้างได้นั้นต้องใช้ secret หรือ public/private key

## เนื้อหา

* TOC
{:toc}

## ใช้ JWT กับงานอะไร

1. Authentication: ใช้ JWT เพื่อนำไปใช้กับการเข้าถึงเซอร์วิส ทรัพยากร หรือ พาธต่าง ๆ ที่อนุญาต และ JWT นิยมนำไปใช้กับ Single Sign On ด้วยความที่ว่ามีโอเวอร์เฮดที่เล็ก และใช้งานง่ายในการทำงานระข้ามโดเมนกัน
2. Information Exchange: JWT เป็นทางเลือกที่ดีตัวหนึ่งที่ช่วยด้านความปลอดภัยในการถ่ายโอนข้อมูลระหว่างกันเนื่องจากใช้ตัว public/private คีย์เพื่อยืนยันว่าเป็นใคร

## Structure of JWT

JWT ประกอบไปด้วย 3 ส่วนที่คั่นกันด้วยจุด `.`

- Header
- Payload
- Signature

ดังนั้น ตัว JWT จะมีหน้าตา Header.Payload.Signature => xxxx.yyyy.zzz

### Header

Header ประกอบไปด้วย 2 ส่วน ได้แก่ ประเภทของโทเค็น และแฮ็ชอัลกอริทึมที่ใช้ เช่น `HMAC SHA256 หรือ RSA` และ encode ส่วน Header นี้ด้วย Base64Url

ตัวอย่างของ Header

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

Payload เป็นส่วนที่สอง โดยในตัว Payload นั้นจะเป็น JSON Object ของส่วนข้อมูลสำคัญที่ใช้อธิบายลักษณะหรือข้อมูลที่จำเป็นบางอย่าง (Claims) และ encode ด้วย Base64Url

Claim มี 3 ประเภท ได้แก่ reserved public และ private

- Reserved claims
  Reserved Claims เป็นกลุ่มของ claim ที่แนะนำว่าควรมีเพื่อการจัดการได้ claim ได้ง่าย ตัวอย่าง claim ของส่วนนี้ เช่น `iss(issuer)` `exp(expiration time)` `sub(subject)` `aud(audience)`
- Public claims
  Puclic claims เป็น claim ที่กำหนดเพิ่มเติม แต่ควรจะกำหนดตาม [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) เพื่อกันการชนกันของชื่อ claim
- Private claims
  Private claims เป็น claim ที่ custom ขึ้นมาเพื่อใช้เป็นข้อมูลที่ได้ตกลงระหว่างกันแล้ว

ตัวอย่าง Payload

```json
{
  "sub": "1234567890",
  "name": "Will smash",
  "admin": true
}
```

### Signature

การสร้าง Signature นั้น จะใช้ข้อมูลทั้งหมดนี้สร้าง Signature ขึ้นมา

- header ที่ encoded แล้ว
- payload ที่ encoded แล้ว
- secret
- hash algorithm ที่กำหนดใน header

สมมติว่าจะสร้าง Signature ด้วยอัลกอริทึม HMAC SHA256 การสร้าง Signature เป็นฟังก์ชันคล้ายแบบนี้ `HMACSHA256(base64UrlEncodde(header) + "." + base64UrlEncode(payload), secret)`

Signature ใช้ในการ verify ว่าคนส่ง JWT มานั้นไม่ได้เปลี่ยนแปลงข้อมูลใด ๆ

## JWT ทำงานอย่างไร

JWT จะถูกสร้างก็ต่อเมื่อ User ทำ authentication ผ่านเรียบร้อยแล้ว และตัวโทเค็นที่ได้ไปไม่ควรจะเก็บไว้นานเกินไป และ[ข้อมูลที่มีความสำคัญมากก็ไม่ควรเก็บลงบน storage ของเบราว์เซอร์](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html#local-storage)

แปะโทเค็นไปกับ `Authorization: Bearer <token>` เพื่อใช้เข้าถึงพาธที่มีการกำหนดสิทธิ์ในการเข้าถึง

และนี่คือ Stateless authentication กล่าวคือจะไม่เก็บ state ของ User ไว้กับ Server memory หน้าที่การรับผิดชอบจะให้ส่วนที่ทำงานเช็คความถูกต้องของ JWT บนเซิฟเวอร์เป็นตัวเช็คว่าผ่านหรือไม่ และนี่ก็เป็นการลดภาระที่ต้องเข้าไปเช็คข้อมูลกับฐานข้อมูล

![JWT Flow](/assets/images/jwt/jwt-flow.PNG)

## ทำไมต้องใช้ JWT

จากบทความได้เปรียบเทียบระหว่าง JSON Web Tokens(JWT), Simple Web Tokens(SWT) และ Security Assertion Markup Language Tokens(SAML)

แน่นอนว่าขนาดเมื่อ encoded ข้อมูลแล้วนั้น JSON มีขนาดที่เล็กกว่า XML และนี่ทำให้ JWT นั้นกระชับกว่า SAML ทำให้ JWT เป็นตัวเลือกที่ดีในการส่งข้อมูลไปกับโปรโตคอล HTTP

SWT ใช้วิธีการทำลายเซ็นแบบสมมาตร (Symetric signed) ด้วยแชร์ secret และใช้ Hash algorithm HMAC

JWT และ SAML ใช้ public/private key ในรูปแบบของ X.509 certificate

JSON เข้ากันได้ง่านกับหลายภาษาโปรแกรมเพราะสามารถ map ไปยัง object ได้ ซึ่งต่างจาก XML หลายภาษาโปรแกรมไม่มีการ map แบบ document-to-object

Resource - [Get Started with JSON Web Tokens](https://auth0.com/learn/json-web-tokens/)
