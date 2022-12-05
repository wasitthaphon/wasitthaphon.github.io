---
layout: post
title: "Angular work with REST Api"
date: 2022-12-05 08:00:00 +0700
categories: Angular
---

# Angular X REST Api

## Overview

สร้าง CRUD ง่าย ๆ

![Overview](/assets/images/angularXrest-api/overview.png)

## Prerequisite

- Node JS
- Angular 13+
- Angular Bootstrap 5+

## How to

1. ติดตั้ง Node JS
2. ติดตั้ง Angular cli

```
npm install -g @angular/cli
```

3. สร้าง Angular web application ใหม่

```
ng new [name]
```

4. ลง package angular bootstrap

```
ng add @ng-bootstrap/ng-bootstrap
```

5. ตั้งค่า base api url ที่ต้องการจะไปดึงข้อมูล
   - environments/environment.ts
   - web api sample [jsonplaceholder](https://jsonplaceholder.typicode.com/)

```ts
export const environment = {
  production: false,
  mockApiUrl: "https://jsonplaceholder.typicode.com",
};
```

6. เพิ่ม modules ที่จะใช้งาน (app.module.ts)

```ts
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";

import { AppRoutingModule } from "./app-routing.module";
import { AppComponent } from "./app.component";
import { NgbModule } from "@ng-bootstrap/ng-bootstrap";
import { HttpClientModule } from "@angular/common/http";
import { FormsModule, ReactiveFormsModule } from "@angular/forms";

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    AppRoutingModule,
    NgbModule,
    HttpClientModule,
    FormsModule,
    ReactiveFormsModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

7. สร้าง Model สำหรับกำหนด Type ในการรับ ส่งข้อมูล
   - สร้างโฟลเดอร์ชื่อ models
   - สร้างไฟล์ใหม่ชื่อ post-blog.model.ts

```ts
export interface PostBlogModel {
  userId: number;
  id: number;
  title: string;
  body: string;
}
```

8. สร้าง Layout หน้าเว็บ (app.component.html)
   การทำงานของหน้านี้จะแบ่งเป็น 2 คอลัมน์ โดย คอลัมน์แรกจะเป็นหน้ารายการ
   และคอลัมน์ที่ 2 จะเป็นส่วนของการ maintenance

```html
<div class="container-fluid h-100 w-100">
  <div class="row p-4">
    <div class="col-12 col-md-6">
      <h2>Posts</h2>

      <!-- Search -->
      <div class="input-group">
        <div class="input-group-prepend">
          <div class="input-group-text">
            <i class="bi bi-search"></i>
          </div>
        </div>
        <input
          type="text"
          class="form-control"
          autocomplete="off"
          id="search"
          name="search"
          [(ngModel)]="term"
          (ngModelChange)="onSearch()"
        />
      </div>

      <!-- Table -->
      <div *ngIf="isPostsLoading; else showPosts" class="text-center">
        <span class="spinner-border text-primary"></span>
      </div>

      <ng-template #showPosts>
        <div style="height: 600px" class="overflow-auto my-4">
          <table class="table table-bordered table-hover">
            <thead>
              <tr>
                <th>UserId</th>
                <th>Id</th>
                <th>Title</th>
                <th>Action</th>
              </tr>
            </thead>

            <tbody>
              <tr *ngFor="let post of filteredPosts">
                <td>{{ post.userId }}</td>
                <td>{{ post.id }}</td>
                <td>{{ post.title }}</td>
                <td>
                  <button
                    class="btn btn-sm btn-outline-primary"
                    (click)="onSelectClick(post.id)"
                  >
                    Select
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </ng-template>
    </div>

    <div class="col-12 col-md-6">
      <h2>Maintenance</h2>

      <form [formGroup]="form">
        <div class="form-grop">
          <label for="title">Title</label>
          <input
            type="text"
            id="title"
            name="title"
            formControlName="title"
            class="form-control"
          />
        </div>

        <div class="form-group">
          <label for="body">Body</label>
          <textarea
            name="body"
            id="body"
            formControlName="body"
            cols="30"
            rows="10"
            class="form-control"
          ></textarea>
        </div>

        <div *ngIf="isMaintenanceLoading" class="text-center">
          <span class="spinner spinner-border text-primary"></span>
        </div>

        <div *ngIf="!isEdit" class="text-center">
          <button
            class="btn btn-outline-primary"
            type="button"
            (click)="onAdd()"
            [disabled]="!form.valid || isMaintenanceLoading"
          >
            Add
          </button>
        </div>

        <div *ngIf="isEdit" class="text-center">
          <button
            type="button"
            class="btn btn-outline-secondary"
            [disabled]="isMaintenanceLoading"
            (click)="onCancel()"
          >
            Cancel
          </button>

          <button
            type="button"
            class="btn btn-outline-warning mx-2"
            (click)="onEdit()"
            [disabled]="!form.valid || isMaintenanceLoading"
          >
            Edit
          </button>
          <button
            type="button"
            class="btn btn-outline-danger"
            (click)="onDelete()"
            [disabled]="!form.valid || isMaintenanceLoading"
          >
            Delete
          </button>
        </div>
      </form>
    </div>
  </div>
</div>
```

### สร้าง Service รองรับการทำงานกับ Api

1. สร้างไฟล์ post-service ด้วยคำสั่งช่วยสร้าง

```
    ng g s services/post --skip-tests
```

2. สร้างฟังก์สำหรับการทำงานกับ api ต่าง ๆ

ประกาศ constructor และ life cycle function บางตัว

```ts
@Injectable({
  providedIn: "root",
})
export class PostService implements OnInit, OnDestroy {
  constructor(private http: HttpClient) {}

  ngOnInit(): void {}

  ngOnDestroy(): void {}
}
```

3. สร้างตัวแปรเพื่อรองรับการทำงานเป็นตัวพักรายการ post

```ts
    posts: Subject<PostBlogModel[]>;

    constructor(private http: HttpClient) {
        this.posts = new Subject<PostBlogModel[]>();
    }

    ngOnInit(): void {
        this.posts.next([]);
    }

    ngOnDestroy(): void {
        this.posts.unsubscribe();
    }

```

4. สร้างฟังกชันก์สำหรับดึงรายการ post

```ts
    // get all posts
    getAllPost(): Observable<PostBlogModel[]> {
        this.http.get(`${environment.mockApiUrl}/posts`).subscribe({
            next: (result: any) => {
            this.posts.next(result);
            },
        });

        return this.posts.asObservable();
    }
```

5. สร้างฟังก์ชันสำหรับการเพิ่ม post ใหม่

```ts
    // create new post
    createPost(newPost: PostBlogModel) {
        return this.http.post(`${environment.mockApiUrl}/posts`, newPost);
    }
```

6. สร้างฟังก์ชันสำหรับการอัปเดต post

```ts
    // update post
    updatePost(newPost: PostBlogModel) {
        return this.http.put(
            `${environment.mockApiUrl}/posts/${newPost.id}`,
            newPost
        );
    }
```

7. สร้างฟังก์ชันสำหรับการลบ post

```ts
    // delete post
    deletePost(id: number) {
        return this.http.delete(`${environment.mockApiUrl}/posts/${id}`);
    }
```

### จัดการการทำงานหลังบ้านของไฟล์ Layout html (app.component.html)

1. เพิ่ม implements onInit

```ts
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent implements OnInit {
  ngOnInit(): void {}
}
```

2. ประกาศตัวแปร

```ts
    // ใช้เป็น temporary ตอนดึงข้อมูลครั้งแรก
    posts: PostBlogModel[] = [];

    // ใช้สำหรับการฟิลเตอร์ตอนค้นหา
    filteredPosts: PostBlogModel[];

    // ใช้เป็น temporary สำหรับ post ทำเลือกอยู่ ณ เวลานั้น
    selectedPost!: PostBlogModel;

    // Boolean ใช้ประกอบการแสดงผลบนเว็บ
    isPostsLoading: boolean;
    isMaintenanceLoading: boolean;
    isEdit: boolean;

    term: string;

    form: FormGroup = new FormGroup({
        userId: new FormControl(0),
        id: new FormControl(0),
        title: new FormControl('', Validators.required),
        body: new FormControl('', Validators.required),
    });

    constructor(private postService: PostService) {
        this.isPostsLoading = false;
        this.isEdit = false;
        this.isMaintenanceLoading = false;
        this.term = '';
        this.filteredPosts = [];
    }
```

3. เริ่มต้นการดึงข้อมูล Post เมื่อมายังหน้าแรก

```ts
    ngOnInit(): void {
        // กำกับให้เปิด spinner
        this.isPostsLoading = true;

        // ดึงข้อมูลจาก api
        this.postService.getAllPost().subscribe({
            next: (result) => {
                // ถ้า response ok จะเข้า blog นี้

                // เก็บรายการที่ดึงได้ทั้งหมด
                this.posts = result;

                // ค้นหาตามฟิลเตอร์
                this.onSearch();

                // mark spinner ปิด
                this.isPostsLoading = false;
            },
            error: (err) => {
                // ถ้า response error จะเข้า blog นี้
                this.isPostsLoading = false;
            },
        });
    }

```

4. สร้างฟังก์ชันค้นหา

```ts
    onSearch() {
        // ถ้า term blank ให้ return post ทั้งหมด
        if (this.term.length == 0) {
            this.filteredPosts = this.posts;
            return;
        }

        this.filteredPosts = this.posts.filter(
        (item) => item.title.indexOf(this.term.trim()) > -1
        );
    }
```

5. สร้างฟังก์ชันแสดงผลเมื่อเลือกรายการบนตาราง

```ts
    onSelectClick(id: number) {

        // เก็บข้อมูล post ที่เลือกขณะนั้น
        this.selectedPost = this.filteredPosts.find((item) => item.id == id)!;

        // ถ้าไม่เจอ ไม่ให้ทำงานต่อ
        if (this.selectedPost == null) {
        return;
        }

        // อัปเดตค่าลงไปในฟอร์ม
        this.form.patchValue({
        ...this.selectedPost,
        });

        // กำกับสถานะของหน้า ณ ตอนนั้นว่ากำลังอยู่ในช่วง maintenance
        this.isEdit = true;
    }

```

6. สร้างฟังก์ชันสำหรับการเพิ่มรายการ

```ts
    onAdd() {
        let newPost: PostBlogModel;
        let latestId: number = this.posts.length + 1;

        this.isMaintenanceLoading = true;

        // เตรียมข้อมูล post ใหม่ที่กำลังจะเพิ่มเข้าไป
        newPost = { ...this.form.value };
        newPost.id = latestId;
        newPost.userId = 1;

        // เรียก api เพื่อสร้าง post ใหม่
        this.postService.createPost(newPost).subscribe({
        next: (res) => {

            // ถ้าสร้างสำเร็จ ให้เพิ่มข้อมูลลงไปใน temporary และประมวลผลหน้าเว็บใหม่
            this.posts = [newPost, ...this.posts];

            this.onSearch();
            this.form.reset();
            this.isMaintenanceLoading = false;
        },
        error: (err) => {
            this.isMaintenanceLoading = false;
        },
        });
    }
```

7. สร้างฟังก์ชันสำหรับการแก้ไข post

```ts
  onEdit() {
    let newPost: PostBlogModel;
    newPost = { ...this.form.value };

    this.isMaintenanceLoading = true;

    this.postService.updatePost(newPost).subscribe({
      next: (res) => {
        this.posts.forEach((post, i) => {
          if (post.id == newPost.id) {
            this.posts[i] = newPost;
            this.selectedPost = newPost;
          }
        });
        this.onSearch();
        this.isMaintenanceLoading = false;
      },
      error: (err) => {
        console.error(err);
        this.isMaintenanceLoading = false;
      },
    });
  }
```

8. สร้างฟังก์ชันสำหรับการลบ post

```ts
  onDelete() {
    let tempPosts: PostBlogModel[] = [];

    this.isMaintenanceLoading = true;

    this.postService.deletePost(this.selectedPost.id).subscribe({
      next: (res) => {
        this.posts.forEach((post) => {
          if (post.id != this.selectedPost.id) {
            tempPosts.push(post);
          }
        });

        this.posts = tempPosts.slice();
        this.onSearch();
        this.form.reset();
        this.isEdit = false;
        this.isMaintenanceLoading = false;
      },
      error: (err) => {
        console.error(err);
        this.isMaintenanceLoading = false;
      },
    });
  }
```

9. สร้างฟังก์ชันสำหรับการยกเลิก maintenance

```ts
  onCancel() {
    this.form.reset();
    this.isEdit = false;
  }
```
