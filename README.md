<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Login & Signup</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg, #667eea, #764ba2);
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }

    .container {
        background: white;
        padding: 30px;
        width: 350px;
        border-radius: 10px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    }

    h2 {
        text-align: center;
    }

    input {
        width: 100%;
        padding: 10px;
        margin: 8px 0;
        border-radius: 5px;
        border: 1px solid #ccc;
    }

    button {
        width: 100%;
        padding: 10px;
        background: #667eea;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        margin-top: 10px;
    }

    button:hover {
        background: #5a67d8;
    }

    .toggle {
        text-align: center;
        margin-top: 10px;
        cursor: pointer;
        color: #667eea;
    }

    .message {
        text-align: center;
        margin-top: 10px;
        font-size: 14px;
    }

    .hidden {
        display: none;
    }
</style>
</head>
<body>

<div class="container">

    <!-- Signup Form -->
    <div id="signupForm">
        <h2>Signup</h2>
        <input type="text" id="signupUsername" placeholder="Username" required>
        <input type="email" id="signupEmail" placeholder="Email" required>
        <input type="password" id="signupPassword" placeholder="Password" required>
        <button onclick="signup()">Sign Up</button>
        <div class="toggle" onclick="showLogin()">Already have an account? Login</div>
        <div class="message" id="signupMessage"></div>
    </div>

    <!-- Login Form -->
    <div id="loginForm" class="hidden">
        <h2>Login</h2>
        <input type="email" id="loginEmail" placeholder="Email" required>
        <input type="password" id="loginPassword" placeholder="Password" required>
        <button onclick="login()">Login</button>
        <div class="toggle" onclick="showSignup()">Don't have an account? Signup</div>
        <div class="message" id="loginMessage"></div>
    </div>

</div>

<script>
function showLogin() {
    document.getElementById("signupForm").classList.add("hidden");
    document.getElementById("loginForm").classList.remove("hidden");
}

function showSignup() {
    document.getElementById("loginForm").classList.add("hidden");
    document.getElementById("signupForm").classList.remove("hidden");
}

function signup() {
    let username = document.getElementById("signupUsername").value;
    let email = document.getElementById("signupEmail").value;
    let password = document.getElementById("signupPassword").value;

    if (username === "" || email === "" || password === "") {
        document.getElementById("signupMessage").innerHTML = "Please fill all fields!";
        return;
    }

    let users = JSON.parse(localStorage.getItem("users")) || [];

    let userExists = users.find(user => user.email === email);
    if (userExists) {
        document.getElementById("signupMessage").innerHTML = "Email already registered!";
        return;
    }

    users.push({ username, email, password });
    localStorage.setItem("users", JSON.stringify(users));

    document.getElementById("signupMessage").style.color = "green";
    document.getElementById("signupMessage").innerHTML = "Signup successful! Please login.";
    
    document.getElementById("signupUsername").value = "";
    document.getElementById("signupEmail").value = "";
    document.getElementById("signupPassword").value = "";
}

function login() {
    let email = document.getElementById("loginEmail").value;
    let password = document.getElementById("loginPassword").value;

    let users = JSON.parse(localStorage.getItem("users")) || [];

    let validUser = users.find(user => user.email === email && user.password === password);

    if (validUser) {
        document.getElementById("loginMessage").style.color = "green";
        document.getElementById("loginMessage").innerHTML = "Login successful! Welcome " + validUser.username;
    } else {
        document.getElementById("loginMessage").style.color = "red";
        document.getElementById("loginMessage").innerHTML = "Invalid email or password!";
    }
}
</script>

</body>
</html>
