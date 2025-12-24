# Contact-book-
Contact book application 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website Layout</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
                <!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Contacts App</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- PWA -->
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#1976d2">

<!-- Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<style>
:root {
    --bg: #e3f2fd;
    --primary: #1976d2;
    --card: #ffffff;
    --text: #000;
}

.dark {
    --bg: #0d1b2a;
    --primary: #2196f3;
    --card: #1b263b;
    --text: #fff;
}

* {
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    margin: 0;
    background: var(--bg);
    color: var(--text);
    display: flex;
    justify-content: center;
}

.app {
    width: 100%;
    max-width: 400px;
    height: 100vh;
    background: var(--bg);
    display: flex;
    flex-direction: column;
    animation: fade 0.4s ease;
}

@keyframes fade { from {opacity:0;} to{opacity:1;} }

.header {
    background: var(--primary);
    color: white;
    padding: 15px;
    text-align: center;
    font-size: 18px;
    position: relative;
}

.toggle { position: absolute; right: 15px; top: 15px; cursor: pointer; }

.content { flex: 1; padding: 15px; overflow-y: auto; }

.card {
    background: var(--card);
    padding: 15px;
    border-radius: 12px;
    margin-bottom: 15px;
    animation: slide 0.3s ease;
}

@keyframes slide { from {transform:translateY(20px);opacity:0;} to {transform:translateY(0);opacity:1;} }

input { width:100%; padding:10px; margin-top:10px; }
button { width:100%; padding:10px; margin-top:10px; border:none; background:var(--primary); color:white; border-radius:6px; cursor:pointer; }
.navbar { display:flex; border-top:1px solid #ccc; background:var(--card); }
.navbar button { flex:1; background:none; border:none; padding:8px; color:var(--text);}
.navbar button.active { color:var(--primary);}
</style>
</head>

<body>

<div class="app">

<!-- LOGIN -->
<div class="content" id="login">
    <div class="card">
        <i class="fa-solid fa-user-lock"></i>
        <h3>Login</h3>
        <input id="username" placeholder="Username">
        <input id="password" type="password" placeholder="Password">
        <button onclick="login()">Login</button>
    </div>
</div>

<!-- MAIN APP -->
<div id="main" style="display:none;">
    <div class="header">
        <i class="fa-solid fa-address-book"></i> Contacts App
        <span class="toggle" onclick="toggleDark()">
            <i class="fa-solid fa-moon"></i>
        </span>
    </div>

    <div class="content" id="content"></div>

    <div class="navbar">
        <button class="active" onclick="searchTab(this)">
            <i class="fa-solid fa-magnifying-glass"></i><br>Search
        </button>
        <button onclick="contactsTab(this)">
            <i class="fa-solid fa-users"></i><br>Contacts
        </button>
        <button onclick="createTab(this)">
            <i class="fa-solid fa-user-plus"></i><br>Create
        </button>
    </div>
</div>

</div>

<script>
let contacts = JSON.parse(localStorage.getItem("contacts") || "[]");
const content = document.getElementById("content");

function login() {
    const username = document.getElementById("username").value;
    const password = document.getElementById("password").value;
    if(username && password){
        document.getElementById("login").style.display = "none";
        document.getElementById("main").style.display = "block";
        searchTab(document.querySelector(".navbar button"));
    } else { alert("Enter username and password!"); }
}

function setActive(btn){
    document.querySelectorAll(".navbar button").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
}

function toggleDark(){ document.body.classList.toggle("dark"); }

function searchTab(btn){
    setActive(btn);
    content.innerHTML = `
        <div class="card">
            <h3><i class="fa-solid fa-magnifying-glass"></i> Search Contact</h3>
            <input id="searchInput" placeholder="Search by name" oninput="filterContacts()">
            <div id="searchResults"></div>
        </div>`;
    filterContacts();
}

function filterContacts(){
    const query = document.getElementById("searchInput").value.toLowerCase();
    const results = contacts.filter(c=>c.name.toLowerCase().includes(query));
    const resultsDiv = document.getElementById("searchResults");
    if(resultsDiv){
        if(results.length === 0){ resultsDiv.innerHTML = "<p>No contacts found.</p>"; }
        else { resultsDiv.innerHTML = results.map(c=>`<p>• ${c.name} (${c.phone})</p>`).join(""); }
    }
}

function contactsTab(btn){
    setActive(btn);
    if(contacts.length===0){ content.innerHTML=`<div class="card"><h3><i class="fa-solid fa-users"></i> Contacts</h3><p>No contacts yet.</p></div>`; return;}
    content.innerHTML = `<div class="card"><h3><i class="fa-solid fa-users"></i> Contacts</h3>${contacts.map(c=>`<p>• ${c.name} (${c.phone})</p>`).join("")}</div>`;
}

function createTab(btn){
    setActive(btn);
    content.innerHTML = `<div class="card">
        <h3><i class="fa-solid fa-user-plus"></i> Create Contact</h3>
        <input id="newName" placeholder="Name">
        <input id="newPhone" placeholder="Phone Number">
        <button onclick="saveContact()">Save Contact</button>
    </div>`;
}

function saveContact(){
    const name = document.getElementById("newName").value;
    const phone = document.getElementById("newPhone").value;
    if(name && phone){
        contacts.push({name, phone});
        localStorage.setItem("contacts", JSON.stringify(contacts));
        alert("Contact saved!");
        createTab(document.querySelector(".navbar button:last-child"));
    } else { alert("Enter both name and phone!"); }
}

// Register Service Worker
if('serviceWorker' in navigator){
    navigator.serviceWorker.register('sw.js').then(()=>console.log("Service Worker Registered"));
}
</script>

</body>
</html>

            </ul>
        </nav>
    </header>
    <main>
        <section id="home">Hero content here</section>
        <section id="about">About content</section>
    </main>
    <footer>Copyright info</footer>
</body>
</html>
