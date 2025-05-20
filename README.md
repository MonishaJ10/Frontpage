<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>3D Carousel with Login</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      background: linear-gradient(135deg, #121212, #1e1e1e);
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
    }

    .container {
      display: flex;
      width: 90%;
      max-width: 1000px;
      height: 520px;
      border-radius: 20px;
      overflow: hidden;
      box-shadow: 0 0 25px rgba(0,0,0,0.6);
      background: rgba(255, 255, 255, 0.05);
      backdrop-filter: blur(10px);
    }

    .carousel-wrapper {
      flex: 1;
      perspective: 1000px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(255, 255, 255, 0.05);
      overflow: hidden;
    }

    .carousel {
      width: 220px;
      height: 320px;
      position: relative;
      transform-style: preserve-3d;
      animation: rotate 20s infinite linear;
    }

    .carousel.paused {
      animation-play-state: paused;
    }

    .carousel div {
      position: absolute;
      width: 180px;
      height: 160px; /* Increased height */
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.1rem;
      font-weight: bold;
      color: #eee;
      box-shadow: 0 8px 32px rgba(0,0,0,0.25);
      cursor: pointer;
      transition: transform 0.3s, background 0.3s;
      backdrop-filter: blur(4px);
      text-align: center;
      padding: 10px;
    }

    .carousel div:hover {
      background: rgba(255, 255, 255, 0.15);
      transform: scale(1.05);
    }

    .carousel div.selected {
      border: 2px solid #1e90ff;
      background: rgba(30, 144, 255, 0.15);
    }

    .carousel div:nth-child(1) {
      transform: rotateY(0deg) translateZ(180px);
    }

    .carousel div:nth-child(2) {
      transform: rotateY(120deg) translateZ(180px);
    }

    .carousel div:nth-child(3) {
      transform: rotateY(240deg) translateZ(180px);
    }

    @keyframes rotate {
      from { transform: rotateY(0deg); }
      to { transform: rotateY(360deg); }
    }

    

    .login-box form {
     display: flex;
    flex-direction: column;
  align-items: flex-start; /* This will center child elements like the button */
}


    .login-wrapper {
      flex: 1;
      padding: 40px;
      background: rgba(34, 34, 34, 0.8);
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .login-box {
      width: 100%;
      max-width: 320px;
    }

    .login-box h2 {
      margin-bottom: 25px;
      font-weight: 700;
      color: #f0f0f0;
      text-align: center;
    }

    .login-box label {
      display: block;
      margin-bottom: 6px;
      color: #bbb;
    }

    .login-box input {
      width: 100%;
      padding: 12px;
      margin-bottom: 20px;
      border-radius: 8px;
      border: 1.5px solid #444;
      background: #333;
      color: #eee;
      font-size: 1rem;
    }

    .login-box input:focus {
      border-color: #1e90ff;
      outline: none;
      background: #2a2a2a;
    }

    .login-box button {
      /* width: 100%; */
      width: auto;
      padding: 10px 20px;
      font-size: 0.95rem;
      border-radius: 20px;
      margin: 10px;
      align-self: center;
      /* padding: 12px; */
      background: #1e90ff;
      color: white;
      border: none;
      /* border-radius: 8px; */
      /* font-size: 1.1rem; */
      font-weight: 600;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    .login-box button:hover {
      background: #0f6adf;
    }

    /* Style for the SSO login text */
    .login-box p.sso-login {
      text-align: center;
      margin-top: 15px;
      color: #bbb;
      font-size: 0.9rem;
    }

    .login-box p.sso-login a {
      color: #1e90ff;
      text-decoration: none;
      cursor: pointer;
    }

    .login-box p.sso-login a:hover {
      text-decoration: underline;
    }

    @media (max-width: 800px) {
      .container {
        flex-direction: column;
        height: auto;
      }

      .carousel-wrapper, .login-wrapper {
        width: 100%;
        height: 300px;
      }

      .carousel-wrapper {
        padding: 20px;
      }

      .carousel {
        width: 180px;
        height: 220px;
      }

      .carousel div {
        width: 140px;
        height: 120px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="carousel-wrapper">
      <div class="carousel" id="carousel">
        <div onclick="toggleSelection(this)">Socrates-NextGen</div>
        <div onclick="toggleSelection(this)">Documentations</div>
        <div onclick="toggleSelection(this)">Tutorials</div>
      </div>
    </div>
    <div class="login-wrapper">
      <div class="login-box">
        <h2>Login</h2>
        <form>
          <label for="uid">UID</label>
          <input type="text" id="uid" placeholder="Enter UID" required />
          <label for="password">Password</label>
          <input type="password" id="password" placeholder="Enter Password" required />
          <button type="submit">Login</button>
          
             <p class="sso-login">(or) Login with <a href="#">SSO</a></p>
          
         
        </form>
      </div>
    </div>
  </div>

  <script>
    const carousel = document.getElementById('carousel');
    const boxes = carousel.querySelectorAll('div');

    function toggleSelection(element) {
      const isSelected = element.classList.contains('selected');
      boxes.forEach(box => box.classList.remove('selected'));

      if (!isSelected) {
        element.classList.add('selected');
        carousel.classList.add('paused');
      } else {
        carousel.classList.remove('paused');
      }
    }
  </script>
</body>
</html>


Header.component.html
<div class="top-bar" role="banner" aria-label="Top navigation bar">
<img ngSrc="assets/LOGO.png" width="150" height="150" alt="BNP LOGO" priority>
<!-- <span>BNP PARIBAS</span> -->
<span class="italic font-semibold text-2x1">NextGen Recon</span>
</div>

Header.component.css
-top-bar {
background-color:
#1a4d2e;
width: 100%;
border-radius: 0.5rem 0.5rem 0 0;
display: flex;
align-items: center;
gap: 0.75rem;
padding: 0.5rem 1rem;
box-sizing: border-box;
user-select:
none;
font-weight: bold;
color : white;
text-shadow: 1px 1px 2px
#2e7d32;
font-size: 0.875rem;
font-style: italic;
box-shadow: 0 2px 6px rgb(000/ 0.3);
}
.top-bar img {
width: auto;
height: 40px;
margin-right: 10px;
}

Header.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing;
import { HeaderComponent } from './header.component';
describe('HeaderComponent', () => {
let component: HeaderComponent;
let fixture: ComponentFixture<HeaderComponent>;
beforeEach (async () => {
await TestBed.configureTestingModule({
imports: [HeaderComponent]
})
.compileComponents();
fixture = TestBed.createComponent (HeaderComponent);
component = fixture.componentInstance;
fixture.detectChanges();
});
it('should create', () => {
expect(component).toBeTruthy();
});
});

header.component.ts
import { Component } from '@angular/core';
import { NgOptimizedImage } from '@angular/common
- @Component({
selector: 'app-header',
standalone: true,
imports: [NgOptimizedImage],
templateUrl: './header.component.html',
styleUrl: './header.component.css'
})

export class HeaderComponent {
}

ANGULAR

Step 1: Angular CLI commands to create the project and components
bash
Copy
Edit
# Create a new Angular standalone project without routing and skip tests if you want
ng new nextgen-carousel --standalone --skip-tests

cd nextgen-carousel

# Generate header component (standalone)
ng generate component header --standalone

# Generate login component (standalone)
ng generate component login --standalone

# The app component is already generated as standalone, edit it directly
Step 2: Project Structure
pgsql
Copy
Edit
nextgen-carousel/
├── src/
│   ├── app/
│   │   ├── header/
│   │   │   ├── header.component.ts
│   │   │   ├── header.component.html
│   │   │   ├── header.component.css
│   │   ├── login/
│   │   │   ├── login.component.ts
│   │   │   ├── login.component.html
│   │   │   ├── login.component.css
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.css
│   ├── assets/
│   │   └── LOGO.png
│   ├── index.html
│   └── main.ts
├── angular.json
└── package.json
Step 3: Code for each file
1. header.component.ts
import { Component } from '@angular/core';
import { NgOptimizedImage } from '@angular/common';

@Component({
  selector: 'app-header',
  standalone: true,
  imports: [NgOptimizedImage],
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent {}
2. header.component.html
<div class="top-bar" role="banner" aria-label="Top navigation bar">
  <img ngSrc="assets/LOGO.png" width="150" height="40" alt="BNP LOGO" priority />
  <span class="italic font-semibold text-2xl">NextGen Recon</span>
</div>
3. header.component.css
.top-bar {
  background-color: #1a4d2e;
  width: 100%;
  border-radius: 0.5rem 0.5rem 0 0;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.5rem 1rem;
  box-sizing: border-box;
  user-select: none;
  font-weight: bold;
  color: white;
  text-shadow: 1px 1px 2px #2e7d32;
  font-size: 1.125rem;
  font-style: italic;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

.top-bar img {
  width: auto;
  height: 40px;
  margin-right: 10px;
}

4. login.component.ts
import { Component } from '@angular/core';

import { CommonModule } from '@angular/common';

import { FormsModule } from '@angular/forms';

I

}

@Component({

selector: 'app-login',

standalone: true,

imports: [CommonModule, Forms Module],

templateUrl: './login.component.html',

styleUrl: './login.component.css'

})

export class LoginComponent {

items = ['Socrates-NextGen', 'Documentations', 'Tutorials'];

selectedIndex: number | null = null;

toggleSelection(index: number): void {

this.selectedIndex = this.selectedIndex === index ? null: index;

}

getTransform(index: number): string {

const angle index * 120;

return rotateY($(angle)deg) translateZ(180px);

}




5. login.component.html

<div class="container">

<div class="carousel-wrapper">

<div class="carousel" [class.paused]="selectedIndex !== null">

<div

*ngFor="let item of items; let i index"

[class.selected]="selectedIndex === i"

(click)="toggleSelection(i)"

[style.transform]="getTransform(i)"

{{ item }}

</div>

</div>

</div>

<div class="login-wrapper">

<div class="login-box">

<h2>Login</h2>

<form>

<label for="uid">UID</label>

>

I

<input type="text" id="uid" placeholder="Enter UID" required />

<label for="password">Password</label>

<input type="password" id="password" placeholder="Enter Password" required />

<button type="submit">Login</button>

<p class="sso-login">(or) Login with <a href="#">SSO</a></p>

</form>

</div>

</div>

</div>


6. login.component.css
.carousel-wrapper {

flex: 1;

perspective: 1000px;

display: flex;

align-items: center;

justify-content: center;

background:

rgba(255, 255, 255, 0.05);

overflow: hidden;

}

.carousel {

width: 220px;

height: 320px;

position: relative;

transform-style: preserve-3d;

animation: rotate 20s infinite linear;

}

I

.carousel.paused {

animation-play-state: paused;

}


.carousel div {

position: absolute;

width: 180px;

height: 160px;

background: rgba(255, 255, 255, 0.08);

border: 1px solid Irgba(255, 255, 255, 0.15);

border-radius: 10px;

display: flex;

align-items: center;

justify-content: center;

font-size: 1.1rem;

font-weight: bold;

color: #eee;

box-shadow: 0 8px 32px

Irgba(0,0,0,0.25);

cursor: pointer;

transition: transform 0.3s, background 0.3s;

backdrop-filter: blur (4px);

text-align: center;

padding: 10px;


}

.carousel div:hover {

background: Irgba(255, 255, 255, 0.15);

transform: scale(1.05);

}

carousel div.selected {

border: 2px solid #1e90ff;

background:rgba(30, 144, 255, 0.15);

}

carousel div:nth-child(1) {

transform: rotateY(Odeg) translateZ(180px);

}

.carousel div:nth-child(2) {

transform: rotateY(120deg) translateZ (180px);

}

-carousel div:nth-child(3) {

transform: rotateY(240deg) translateZ (180px);

}

I

@keyframes rotate {

from { transform: rotateY(0deg); }

to { transform: rotateY(360deg); }

}

.login-wrapper {

height: 100vh;

width: 100vw;

flex: 1;

padding: 40px;

background: rgba(34, 34, 34, 0.8)

padding: 40px;

background: rgba(34, 34, 34, 0.8);

display: flex;

align-items: center;

justify-content: center;

}

.login-box {

height: 100vh;

width: 100vw;

max-width: 320px;

}

.login-box h2 {

margin-bottom: 25px;

margin-top: 25px;

font-weight: 700;

color: #fefefe;

text-align: center;

}

login-box label {

display: block;

margin-bottom: 6px;

color: #bbb;

}

login-box input {

width: 100%;

padding: 12px;

margin-bottom: 20px;

border-radius: 8px;

border: 1.5px solid #444;

background:#333;

color: #eee;

font-size: 1rem;

}

.login-box input:focus {

border-color: #1e90ff;

outline: none;

background: #2a2a2a;

}

login-box button:hover {

| background: #0f6adf;

}

login-box p.sso-login {

text-align: center;

margin-top: 15px;

color: #bbb;

font-size: 0.9rem;
}

.login-box p.sso-login a {

color: #1e90ff;

text-decoration: none;

cursor: pointer;

}

login-box button {

width: auto;

padding: 10px 20px;

font-size: 0.95rem;

border-radius: 20px;

margin: 10px auto;

background:#1e90ff;

color: white;

border: none;

font-weight: 600;

cursor: pointer;

transition: background 0.3s ease;

align-self: center;

}

.login-box p.sso-login a:hover {

text-decoration: underline;

}

@media (max-width: 800px) {

.container {

flex-direction: column;

height: auto;
}
.carousel-wrapper, login-wrapper {

width: 100%;

height: 300px;

.carousel-wrapper {

padding: 20px;

}

.carousel {

width: 180px;

height: 220px;

}

carousel div {

width: 140px;

height: 120px;

}



7. app.component.ts

import { Component } from '@angular/core';
import { HeaderComponent } from './header/header.component';
import { LoginComponent } from './login/login.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HeaderComponent, LoginComponent],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {}


8. app.component.html

<app-header></app-header>
<app-login></app-login>

9. app.component.css

body, html {

margin: 0;

height: 100%;

font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

background: linear-gradient(135deg, #121212, #1e1e1e);

color: white;

}

.container {

display: flex;

max-width: 1000px;

width: 90%;

margin: 2rem auto;

height: 520px;

border-radius: 20px;

overflow: hidden;

box-shadow: 0 0 25px Irgba(0, 0, 0, 0.6);

background: rgba(255, 255, 255, 0.05);

backdrop-filter: blur (10px);

}

app-login

flex: 1;

display:

flex;

align-items: center;

justify-content: center;

}
------------------------------------------------------------------------------------------
shades 
login.component.html
<div class="container">
  <div class="carousel-wrapper">
    <div class="carousel" [class.paused]="selectedIndex !== null">
      <div
        *ngFor="let item of items; let i = index"
        [class.selected]="selectedIndex === i"
        (click)="toggleSelection(i)"
        [style.transform]="getTransform(i)"
      >
        {{ item }}
      </div>
    </div>
  </div>

  <div class="switch">
    <div *ngIf="isLogin" class="Login">
      <div class="title">Login</div>
      <div class="item">Reconciliation</div>
      <div class="item">Trade matching</div>

      <div class="wrapper">
        <input
          type="text"
          placeholder="UID"
          class="betterOutline"
          [(ngModel)]="uid"
        />
        <input
          type="password"
          placeholder="Password"
          class="betterOutline"
          [(ngModel)]="password"
        />
      </div>

      <div class="item">
        <button (click)="onLogin()">Submit</button>
      </div>

      <div class="item">(or)</div>

      <div class="item" style="font-size: 0.8em; color: black;">
        Login with
        <span class="LoginSign" (click)="onSwitch()">&nbsp;SSO</span>
      </div>
    </div>

    <div *ngIf="!isLogin" class="Login">
      <div class="title">Sign Up</div>
      <div class="item">Connect, create, and match</div>
      <div class="item">All in one place</div>

      <div class="item">
        <button>SSO Login</button>
      </div>

      <div class="item" style="font-size: 0.8em; color: black;">
        Have account?
        <span class="LoginSign" (click)="onSwitch()">&nbsp;Login</span>
      </div>
    </div>
  </div>
</div>

css
background: linear-gradient(
  to bottom,
  rgba(0, 0, 0, 0.95) 0%,
  rgba(40, 40, 40, 0.8) 50%,
  rgba(230, 230, 230, 0.1) 100%
);
.switch {
  /* existing styles */
  box-shadow: inset 0px 0px 20px rgba(0, 0, 0, 0.6), 0 0 10px rgba(0, 0, 0, 0.2);
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 4dvw;
  width: 100%;
  height: 100dvh;
  background-color: #d5d3d22a;
  flex-wrap: wrap;
}

.carousel-wrapper {
  flex: 1;
  perspective: 1000px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.05);
  overflow: hidden;
}

.carousel {
  width: 220px;
  height: 320px;
  position: relative;
  transform-style: preserve-3d;
  animation: rotate 20s infinite linear;
}

.carousel.paused {
  animation-play-state: paused;
}

.carousel div {
  position: absolute;
  width: 180px;
  height: 160px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  font-weight: bold;
  color: #eee;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  transition: transform 0.3s, background 0.3s;
  backdrop-filter: blur(4px);
  text-align: center;
  padding: 10px;
}

.carousel div:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: scale(1.05);
}

.carousel div.selected {
  border: 2px solid #1e90ff;
  background: rgba(30, 144, 255, 0.15);
}

.carousel div:nth-child(1) {
  transform: rotateY(0deg) translateZ(180px);
}

.carousel div:nth-child(2) {
  transform: rotateY(120deg) translateZ(180px);
}

.carousel div:nth-child(3) {
  transform: rotateY(240deg) translateZ(180px);
}

@keyframes rotate {
  from {
    transform: rotateY(0deg);
  }
  to {
    transform: rotateY(360deg);
  }
}

.switch {
  box-shadow: rgba(10, 10, 10, 0.26) 1px 1px 20px;
  border-top-left-radius: 5dvh;
  border-bottom-right-radius: 5dvh;
  background: linear-gradient(
    0deg,
    rgb(201, 201, 201) 22%,
    rgba(0, 0, 0, 1) 69%,
    rgba(9, 9, 20, 1) 79%,
    rgb(0, 0, 0) 100%
  );
  width: 25%;
  position: relative;
  top: 4dvh;
  min-width: 280px;
}

.Login .title {
  display: flex;
  justify-content: center;
  padding: 0.5em 0;
  color: white;
  font-size: 1.1em;
  font-style: italic;
}

.Login .item {
  color: white;
  font-size: 0.8em;
  display: flex;
  padding: 1.2dvh;
  justify-content: center;
}

.wrapper {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 4dvh;
  align-items: center;
  padding: 1em 1em;
}

.betterOutline::placeholder {
  font-style: italic;
}

.betterOutline:focus {
  --width: 3px;
  --color: rgb(0 119 72);
  outline: transparent;
  box-shadow: 0px var(--width) var(--color);
}

.betterOutline {
  font-family: inherit;
  font-size: 0.8em;
  color: white;
  border: none;
  border-radius: calc((1rem + 1.25rem * 2) / 2);
  background-color: #333;
  padding: 0.5rem 1.25rem;
  box-shadow: none;
  transition: box-shadow 0.15s;
  width: 80%;
}

.Login button {
  border-radius: 50px;
  background-color: rgb(1, 1, 1);
  color: rgb(255, 253, 253);
  cursor: pointer;
  font-size: 0.9em;
  padding: 0.7dvh 1dvw;
  font-style: italic;
  letter-spacing: 0.2dvw;
}

.Login button:hover {
  box-shadow: rgb(10, 10, 10) 1px 1px 10px;
  transition: ease-in 0.5s;
}

.LoginSign {
  cursor: pointer;
}

.LoginSign:hover {
  color: #2e0cefa0;
  transition: 0.7s ease;
}

/* Responsive design */
@media (max-width: 800px) {
  .container {
    flex-direction: column;
    height: auto;
  }

  .carousel-wrapper, .switch {
    width: 100%;
    height: auto;
    padding: 20px;
  }

  .carousel {
    width: 180px;
    height: 220px;
  }

  .carousel div {
    width: 140px;
    height: 120px;
  }
}
