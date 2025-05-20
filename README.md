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


5. login.component.html


6. login.component.css


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

