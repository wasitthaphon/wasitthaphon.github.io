---
layout: post
title: "Git 101"
date: 2022-03-14 15:06:00 +0700
categories: Git
---

## Git คืออะไร ?

Git เป็นโปรโตคอล และซอฟต์แวร์สำหรับการจัดการเวอร์ชันของไฟล์ทั้งตัวไฟล์ Text และไฟล์ Binary

## เนื้อหา

* TOC
{:toc}

## ทำไมต้องเป็น Git ?

Git เข้ามามีบทบาทในการทำเวอร์ชันให้กับไฟล์ในแต่ละการอัปเดตได้ และไม่จำเป็นต้องมีเซิฟเวอร์

จุดเด่นที่ทำให้ Git มีการใช้งานที่แพร่หลายมาจากฟีเจอร์เด่น ๆ เช่น Branch, Commit และ เป็นการควบคุมแบบไม่จำเป็นต้องพึ่งศูนย์กลาง เป็นต้น

### Branch

Branch เป็นการแยกสาขาการทำงาน เราแยกสาขาการทำงานออกมาเพื่อความเป็นระเบียบเพราะ Branch สามารถใช้อธิบายชุดงานได้ว่าเรากำลังทำอะไรอยู่

Branch เป็นตัวชี้ของ snapshot ในจุดของการเปลี่ยนแปลงล่าสุดของแต่ละ Branch ของมันเอง

### Commit

เราสามารถบันทึกการทำงานได้ด้วย Commit และตัว Commit เองเป็นเสมือน Pointer ที่คอยชี้ว่าแต่ละจุดมีการเปลี่ยนแปลงอะไรบ้าง

### Distributed source control

กรณีทำงานร่วมกันหลายคน ผู้ใช้ไม่จำเป็นต้องเชื่อมต่อเซิฟเวอร์กลางเพื่ออัปเดตไฟล์กับตัวที่อยู่บนเซิฟเวอร์ตลอดเวลา 

ผู้ใช้อัปเดตไฟล์บน Local repository ของตนเอง และหากมีอัปเดตที่ต้องการให้ทุกคนอัปเดตด้วย จึงค่อยอัปเดตขึ้นไปยังตัวเซิฟเวอร์

## คำสั่ง Git

อ้างอิงจาก [Git Cheatsheet](https://ndpsoftware.com/git-cheatsheet.html#loc=index;) จะเห็นว่าคำสั่งของ Git แบ่งได้อยู่ 5 กลุ่ม ได้แก่

1. Stash
2. Workspace
3. Index
4. Local repository
5. Upstream repository

### Stash

Stash เป็นพื้นที่ใช้เก็บสะสมไฟล์ที่เราแก้ไขยังไม่เสร็จ แต่เราต้องการ switch ไปทำงานที่ branch อื่น

#### คำสั่งที่ใช้กับ Stash

| คำสั่ง                                             | คำอธิบาย                                                                                                      |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| stash push [&lt;msg&gt;]                        | ย้ายการเปลี่ยนแปลงทั้งหมดเข้าไปสะสมไว้ในที่พัก จากนั้น git จะ revert (reset --hard) ส่วน &lt;msg&gt; เป็นคำอธิบายให้กับ stash |
| stash pop                                       | นำเอาการเปลี่ยนแปลงตัวที่ stash บนสุดออกมาและลบ stash ตัวนั้นทิ้ง                                                       |
| stash apply [&lt;stash&gt;]                     | เลือกเอา stash ออกมา                                                                                         |
| stash list                                      | แสดงรายการที่ทำ stash                                                                                          |
| stash show [&lt;stash&gt;]                      | แสดง diff ของ stash                                                                                         |
| stash drop [&lt;stash&gt;]                      | ลบ stash 1 รายการ                                                                                           |
| stash clear                                     | ลบ stash ทั้งหมด                                                                                              |
| stash branch &lt;branchname&gt; [&lt;stash&gt;] | สร้าง Branch ตามที่ระบุจากนั้น check out ไปยัง Branch นั้นพร้อมกับ stash                                               |

### Workspace

ที่ที่ใช้ทำงาน

#### คำสั่งที่ใช้กับ Workspace

| คำสั่ง                                        | คำอธิบาย                                                                           |
| ------------------------------------------ | -------------------------------------------------------------------------------- |
| status                                     | แสดงสถานะของไฟล์ที่มีการเปลี่ยนแปลงทั้งหมดไม่ว่าจะเป็น เพิ่ม ลบ แก้ไข เปลี่ยนชื่อ                  |
| diff                                       | แสดงความแตกต่างที่ยังไม่ได้ถูกนำเข้าไปใน Index                                            |
| diff &lt;commit หรือ branch&gt;             | แสดงความแตกต่างตาม Commit id หรือตาม Branch                                        |
| add &lt;file... หรือ dir...&gt;             | เพิ่มไปยัง Index                                                                    |
| add -u                                     | เพิ่มไฟล์ที่ไม่ใช่ไฟล์ใหม่เข้าไปยัง Index                                                   |
| rm &lt;file(s)...&gt;                      | ลบไฟล์ออกจาก Workspace และ Index                                                  |
| mv &lt;file(s)...&gt;                      | ย้ายไฟล์                                                                           |
| commit -a [-m 'msg']                       | บันทึกการเปลี่ยนแปลงตามไฟล์ที่บันทึกใน Index และลบไฟล์ใน Index ถ้าไฟล์นั้นถูกลบออกจาก Workspace |
| checkout &lt;file(s)... or dir&gt;         | อัปเดตไฟล์ใน Workspace ตามที่มีใน Branch ที่ Checkout                                   |
| reset --hard                               | ย้อนกลับไปยัง Workspace และ Index ตาม Local repository                              |
| reset --hard &lt;remote&gt;/&lt;branch&gt; | ย้อน Local repository กลับไปให้เหมือนกับ Remote branch                                |
| switch &lt;branch&gt;                      | สลับไปยัง Branch ที่ระบุ                                                              |
| checkout -b &lt;name of new branch&gt;     | สร้าง Branch และสลับไป Branch ที่ระบุ                                                 |
| merge &lt;commit หรือ branch&gt;            | รวม Source จาก Branch ที่ระบุมายัง Branch ที่กำลังทำงานอยู่                                 |
| rebase &lt;upstream branch&gt;             | ทำงานที่มีผลลัพธ์คล้าย Merge แต่ Rebase สามารถปรับแต่งได้                                   |
| cherry-pick &lt;commit&gt;                 | เอาการเปลี่ยนแปลงจาก Commit ที่ระบุมาทำงานรวมกันบน Branch ปัจจุบันแล้ว Commit               |
| revert &lt;commit&gt;                      | ทำงานคล้าย Undo แต่เป็นการทำงานตรงกันข้ามกับประวัติที่ Commit พร้อมทั้งสร้าง Commit ให้ใหม่        |
| clone &lt;repo&gt;                         | ดาวน์โหลด Repository                                                              |
| pull &lt;remote&gt; &lt;refspec&gt;        | ดึงเอาการเปลี่ยนแปลงจาก Remote มาลงที่ Current branch                                 |

### Index

Index เป็นพื้นที่ใช้เตรียมไฟล์ที่พร้อมทำ commit ถัดไป

กลุ่มนี้สามารถเรียกได้หลายอย่าง ได้แก่

- Current directory cache
- Staging area
- Cache
- Staged file

#### คำสั่งที่ใช้กับ Index

| คำสั่ง                            | คำอธิบาย                                                     |
| ------------------------------ | ---------------------------------------------------------- |
| reset HEAD &lt;file(s)...&gt;  | ลบไฟล์ออกจาก Index กลับไปยัง Unstage area                     |
| reset --soft HEAD^             | ย้อนกลับการเปลี่ยนแปลงจาก commit ล่าสดแล้วเอาไปไว้ใน Working tree |
| diff --cached [&lt;commit&gt;] | ดูความแตกต่างของการเปลี่ยนแปลงระหว่า Staged กับ Commit ตัวล่าสุด    |
| commit [-m 'msg']              | บันทึก Index                                                 |
| commit --amend                 | บันทึก Index ลง Commit ล่าสุดแทนการทำ Commit ใหม่                |

### Local repository

Local repository เป็นพื้นที่เก็บทรัพยากร (ไฟล์ source code, text, image ฯลฯ) และมีตัวควบคุมหรือจัดการทรัพยากร (ประวัติ commit, branch, source code, อัปเดตต่าง ๆ) นั้นอยู่ในโฟลเดอร์ `.git`

#### คำสั่งที่ใช้กับ Local repository

| คำสั่ง                                                                 | คำอธิบาย                                               |
| -------------------------------------------------------------------- | ---------------------------------------------------- |
| log                                                                  | ดูรายการ Commit เรียงลำดับจากใหม่สุดไปเก่าสุด                |
| diff &lt;commit&gt; &lt;commit&gt;                                   | ดูการเปลี่ยนแปลงระหว่าง Commit                           |
| branch                                                               | แสดงรายการ Branch                                    |
| branch -d &lt;branch&gt;                                             | ลบ Branch                                            |
| branch --track &lt;new branch name&gt; &lt;remote&gt;/&lt;branch&gt; | สร้าง Branch ใหม่และให้ Track กับ Remote branch          |
| fetch &lt;remote&gt; &lt;refspec&gt;                                 | ดาวน์โหลดอ็อบเจ็กต์จาก Remote                            |
| push                                                                 | อัปเดต Remote ทุก Branch ที่ทั้ง Local และ Server รู้จักกันแล้ว |
| push &lt;remote&gt; &lt;branch&gt;                                   | อัปเดตไปยัง Remote ด้วยชื่อ Branch ที่ระบุ                   |
| push &lt;remote&gt; &lt;local&gt;:&lt;name&gt;                       | อัปเดตไปยัง Remote ด้วย Branch local ไปยัง name ที่ระบุ     |

### Upstream repository

Upstream repository เป็น Repository ที่อยู่บน Hosting ที่คอยเป็นศูนย์กลางในการอัปเดตเวอร์ชันของโปรเจ็กต์ระหว่างเราและเพื่อนร่วมทีม

#### คำสั่งที่ใช้กับ Upstream repository

| คำสั่ง                                         | คำอธิบาย                   |
| ------------------------------------------- | ------------------------ |
| branch -r                                   | แสดงรายการ Remote branch |
| push &lt;remote&gt; --delete &lt;branch&gt; | ลบ Remote branch         |

## เมื่อเกิดปัญหา แต่ไม่รู้จะใช้คำสั่งอะไร ?

ให้ใช้โฟลวตัวนี้ช่วยคุณ [Git mess?](http://justinhileman.info/article/git-pretty/git-pretty.png)

## Git configuration

เราสามารถตั้งค่า Git ได้ด้วยคำสั่ง `git config` เป็นการตั้งค่าให้กับ Git เช่น สีของตัวหนังสือบน command prompt, commit template, name, email, ฯลฯ

### สิ่งที่ควรตั้งค่าตั้งแต่เริ่มต้น

เราควรจะตั้งค่า name และ email ตั้งแต่เริ่มต้นไปเลย การตั้งค่า name และ email
นั้นจะใช้เป็นข้อมูลในการบันทึกไปกับ commit

```git
git config --global user.name "name"
```

ตั้งค่าชื่อที่ต้องการบันทึกลงบน commit

```git
git config --global user.email "email address"
```

ตั้งค่า email ที่ต้องการบันทึกลงบน commit

## Branches

> :memo: commit ใด ๆ ที่ผู้ใช้บันทึกจะเกิดขึ้นบน Branch ปัจจุบันที่ผุ้ใช้กำลังทำงานอยู่

```git
git branch [branch-name]
```

สร้าง branch ใหม่

```git
git checkout [branch-name]
```

สลับไปยัง branch ที่ระบุและอัปเดตไฟล์ทั้งหมดให้อยู่ตามเดิมของ branch นั้น

```git
git merge [branch]
```

รวม branch ที่ระบุมาไว้ฃใน branch ที่ผู้ใช้กำลังทำงานอยู่ ณ ปัจจุบัน

```git
git branch -d [branch-name]
```

ลบ branch

## คำสั่ง Basic

```git
git log
```

รายการประวัติ commit ของ branch ที่กำลังทำงานอยู่ ณ ปัจจุบัน

```git
git status
```

ดูสถานะรายการที่มีการเปลี่ยนแปลงทั้งหมด ณ ปัจจุบัน

```git
git add [file]
```

ทำ snapshot ของไฟล์ต่าง ๆ ที่ต้องการให้บันทึกการเปลี่ยนแปลง

```git
git commit -m "[descriptive message]"
```

บันทึกกลุ่มไฟล์ที่ได้ทำ snapshot ด้วยคำสั่ง git add เรียบร้อยแล้ว

## Synchronize changes

คำสั่งการ Sycnhronize ระหว่าง Local repository กับ Remote repository

```git
git fetch
```

ดาวน์โหลดประวัติ commit ทั้งหมดของ branch ต่าง ๆ ที่ทำ Tracking (Set upstream หากันแล้ว) เรียบร้อยแล้วมากองไว้

```git
git merge
```

รวม commit จาก remote repository ที่ fetch มาแล้วไปไว้ใน local branch

```git
git push
```

อัปโหลด Local branch ขึ้นไปยัง Remote branch

```git
git push --set-upstream [remote] [branch]
```

set-upstream เพื่อ Tracking ระหว่าง Local branch กับ Branch บน Remote repository

```git
git pull
```

อัปเดต local branch ปัจจุบันด้วย commit ต่าง ๆ บน remote branch คำสั่งนี้เกิดจากการทำงานของ 2 คำสั่งต่อไปนี้ร่วมกัน คือ `git fetch` และ `git merge`

## Redo commits

คำสั่งสำหรับการย้อนกลับ หรือยกเลิกการแก้ไขต่าง ๆ

```git
git reset [commit]
```

ย้อนกลับไปยัง commit ที่ระบุ และให้เก็บการเปลี่ยนแปลงของ commit ที่ข้ามไปไว้ด้วย

```git
git reset --hard [commit]
```

ย้อนกลับไปยัง commit ที่ระบุ และให้ลบการเปลี่ยนแปลงทั้งหมดของ commit ที่ข้ามไป

```git
git cherry-pick [commit]
```

เอาการเปลี่ยนแปลงของ commit ที่ระบุมารวมกับการทำงานปัจจุบัน

```git
git revert [commit]
```

ทำตรงกันข้ามของการเปลี่ยจนแปลงทั้งหมดบน commit ที่ระบุ เช่น

- ถ้า commit นั้นมีไฟล์ที่เพิ่มเข้ามา commit ที่จะบันทึกถัดไปจะลบไฟล์นั้นแทน
- ถ้า commit นั้นมีการแก้ไขเพิ่มเข้าไป commit ที่จะบันทึกถัดไปจะลบการแก้ไขนั้นออกแทน

## การใช้ Git บน Visual studio code (vs code)

การทำงาน Source control บน vs code สามารถเข้าไปใช้งานง่าย ๆ โดยไม่ต้องใช้ git cli บน command prompt

![Food & drink](/assets/images/git-resource/git-entrance.PNG)

ก่อนจะเข้ามาใช้งานที่ Extension source control ได้ ที่ Directory นั้น ๆ ต้องมีโฟลเดอร์ .git ก่อน (ผ่านการ `git init` มาแล้ว)

## Extensions

- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)

## Workshop

workshop นี้จะเป็นการพาใช้งาน git เบื้องต้นทั้งการทำงานคนเดียว (Local repository) และการทำงานร่วมกับหลาย ๆ คน (Remote repository)

### การใช้งาน Git เบื้องต้นบน Local repository

#### หัวข้อนี้เราจะลองใช้ git สักเล็กน้อย โดยเราจะทำดังต่อไปนี้

- การ initialize git
- การใช้เครื่องมือ VS code จัดการกับ Source control
- การ Tracked ไฟล์
- การแก้ไขไฟล์
- การ commit พร้อมทั้งการทำ commit message

#### มาเริ่มกันเลย

1. เปิด command promp แล้วไปยัง Directory ที้ต้องการจะสร้าง git source control

   ![Git init location](/assets/images/git-resource/cmd.PNG)

2. ใช้คำสั่ง `git init` เพื่อสร้าง git project

   ```git
   git init
   ```

3. สร้างไฟล์ Text ขึ้นมา 1 ไฟล์ชื่อ drinks.txt

   ![Drinks](/assets/images/git-resource/create-file-drinks.PNG)

   จะสังเกตเห็นว่าที่ไฟล์ drinks.txt จะมีสี และตัวอักษรพิมพ์ใหญ่ปรากฏบนไฟล์ (ฟีเจอร์ของ git ตัวนี้ขึ้นอยู่กับเครื่องมือที่ support ด้วย) ขออธิบายที่ตัวอักษรนะครับ

   1. U: Untracked คือ ไฟล์ที่ยังไม่เคยถูกทำ source control เลย
   2. A: Index Added คือ ไฟล์ที่ถูกเพิ่มเข้ามาใหม่ใน Staging area (หลังจาก add)
   3. M: Modified คือ ไฟล์ที่ถูก Tracked และมีการแก้ไข
   4. R: Index Renamed คือ ไฟล์ที่จะถูกเปลี่ยนชื่อใน commit ถัดไป
   5. D: Deleted คือ ไฟล์ที่จะถูกลบใน commit ถัดไป

4. ใส่ข้อมูลเครื่องดี่ม (หรืออะไรก็ได้)

   ![Drinks some data](/assets/images/git-resource/some-data.PNG)

   จากภาพมีข้อมูลเครื่องดื่ม 3 รายการ ได้แก่ 🍷, 🍺 และ 🥛

5. ทำ Tracked ไฟล์ drinks.txt

   การทำ Track ไฟล์สามารถทำผ่าน interface ของ IDE ที่สนับสนุน หรือ ผ่าน git cli ได้โดยตรง

   ![track file](/assets/images/git-resource/git-add-track.PNG)

   จากภาพจะมีอยู่ 2 วิธีที่ทำ Track ไฟล์ได้

   1. คลิกที่ปุ่ม "+"
   2. ใช้คำสั่ง `git add [filename]`

6. สังเกตผลการทำ track ไฟล์ drinks.txt

   ![added file](/assets/images/git-resource/git-added.PNG)

   จะเห็นว่าสีแดงซ้ายมือ หมายถึงบรรทัดนั้น ๆ ถูกลบออก และสีเขียวที่อยู่ขวามือหมายถึงบรรทัดนั้น ๆ ถูกเพิ่มเข้ามาใหม่

7. commit การทำงาน

   commit เพื่อบันทึกการทำงานโดยการ commit นั้น มีหลักการตั้งชื่ออธิบายการอัปเดตในแต่ละครั้งด้วยเช่นกัน สามารถเข้าไปศึกษาเพิ่มเติมได้ที่ [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

   การทำ commit สามารถทำได้ทั้งผ่าน git cli หรือ ผ่าน interface ของ IDE ที่สนับสนุน โดยจากภาพจะเป็นการทำ commit message ผ่าน GUI ของ vs code

   ![Commit message](/assets/images/git-resource/commit-message.PNG)

   หากใส่ข้อความเรียบร้อยแล้วให้คลิ้กที่ `เครื่องหมายถูก`

8. ตรวจประวัติ commit

   สามารถดูประวัติ commit ของ branch นั้น ๆ ได้ด้วยการไปยังแท็บ `COMMITS`

   ![Commit history](/assets/images/git-resource/commit-history.PNG)

9. แก้ไขข้อมูลไฟล์ drinks.txt

   ลองแก้ไขข้อมูลรายการเครื่องดื่มบางอย่างในไฟล์ drinks.txt เช่น ลบบางเครื่องดื่ม แล้วเพิ่มเครื่องดื่มชนิดใหม่เข้าไป

   ![Add drinks](/assets/images/git-resource/add-new-drink.PNG)

   จากภาพลบ 🍺 ออกไปแล้วเพิ่ม ☕ และ 🍵 เข้ามา

10. Add ไฟล์ drinks.txt เพื่อเข้า staging area สำหรับเตรียม commit ใหม่

    add ไฟล์ที่ต้องการ commit เข้าไปยัง staging area เมื่อ add สำเร็จจะเห็นไฟล์ไปอยู่ใน Tab `Staged Changes` ดังแสดงตามภาพ

    ![Git differenct](/assets/images/git-resource/git-different.PNG)

    หากลองคลิ้กที่ไฟล์ `drinks.txt` บน `Staged Changes` ตัว vs code จะแสดงสิ่งที่นำออก (ไฮไลท์สีแดงฝั่งซ้าย) และสิ่งที่เพิ่มเข้าไป (ไฮไลท์สีเขียวฝั่งขวา)

    จากภาพจะเห็นว่า

    1. ลบ 🍺 บรรทัดที่ 2 ออก แล้วเพิ่ม ☕ เข้าไปแทน
    2. เพิ่ม 🍵 ที่บรรทัดที่ 4

11. commit changes

    บันทึกการทำงานและใส่ commit message ที่สื่อความหมายได้ลงไป

    ![Git commit #2](/assets/images/git-resource/commit-2.PNG)

12. สังเกตประวัติ commits

    ให้ไปที่แท็บ `COMMITS` จะพบว่ามีประวัติการ commit เพิ่มขึ้นมาใหม่ 1 commit

    ![Git history #2](/assets/images/git-resource/commit-history-2.PNG)
