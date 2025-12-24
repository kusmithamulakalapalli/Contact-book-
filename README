<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Contact Book</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="app">
    <h1>📘 Contact Book</h1>

    <input type="text" id="name" placeholder="Name">
    <input type="text" id="phone" placeholder="Phone Number">
    <input type="email" id="email" placeholder="Email">

    <button onclick="addContact()">➕ Add Contact</button>

    <input type="text" id="search" placeholder="🔍 Search Contact" onkeyup="searchContact()">

    <ul id="contactList"></ul>
</div>

<script src="script.js"></script>
</body>
</html>
<h2>📞 Call History</h2>

<div class="history">
    <h3>Dialed Calls</h3>
    <ul id="dialedList"></ul>

    <h3>Missed Calls</h3>
    <ul id="missedList"></ul>
</div>
body {
    font-family: Arial, sans-serif;
    background: #0b3c5d;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.app {
    background: #1f6f8b;
    padding: 20px;
    border-radius: 10px;
    width: 320px;
}

h1 {
    text-align: center;
}

input {
    width: 100%;
    padding: 8px;
    margin: 5px 0;
    border-radius: 5px;
    border: none;
}

button {
    width: 100%;
    padding: 8px;
    background: #3282b8;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #bbe1fa;
    color: black;
}

li {
    background: #bbe1fa;
    color: black;
    margin: 5px 0;
    padding: 5px;
    border-radius: 5px;
    list-style: none;
}
.history {
    background: #0b3c5d;
    padding: 10px;
    border-radius: 8px;
    margin-top: 10px;
}

.history h3 {
    color: #bbe1fa;
    font-size: 16px;
}
let contacts = JSON.parse(localStorage.getItem("contacts")) || [];
let dialedCalls = JSON.parse(localStorage.getItem("dialedCalls")) || [];
let missedCalls = JSON.parse(localStorage.getItem("missedCalls")) || [];

function addContact() {
    let name = document.getElementById("name").value;
    let phone = document.getElementById("phone").value;
    let email = document.getElementById("email").value;

    if (name === "" || phone === "") {
        alert("Name and Phone are required");
        return;
    }

    contacts.push({ name, phone, email, blocked: false });
    contacts.sort((a, b) => a.name.localeCompare(b.name));

    localStorage.setItem("contacts", JSON.stringify(contacts));
    displayContacts();
}

function displayContacts() {
    let list = document.getElementById("contactList");
    list.innerHTML = "";

    contacts.forEach((c, index) => {
        let li = document.createElement("li");
        li.innerHTML = `
            <b>${c.name}</b><br>
            📞 ${c.phone}<br>
            📧 ${c.email || "N/A"}<br>

            <button onclick="makeCall('${c.name}','${c.phone}')">📲 Call</button>
            <button onclick="missCall('${c.name}','${c.phone}')">❌ Missed</button>
            <button onclick="blockContact(${index})">
                ${c.blocked ? "Unblock" : "Block"}
            </button>
        `;
        list.appendChild(li);
    });

    displayHistory();
}

function makeCall(name, phone) {
    dialedCalls.unshift(`${name} - ${phone}`);
    localStorage.setItem("dialedCalls", JSON.stringify(dialedCalls));
    displayHistory();
    window.location.href = `tel:${phone}`;
}

function missCall(name, phone) {
    missedCalls.unshift(`${name} - ${phone}`);
    localStorage.setItem("missedCalls", JSON.stringify(missedCalls));
    displayHistory();
}

function displayHistory() {
    let dialedList = document.getElementById("dialedList");
    let missedList = document.getElementById("missedList");

    dialedList.innerHTML = "";
    missedList.innerHTML = "";

    dialedCalls.forEach(call => {
        let li = document.createElement("li");
        li.textContent = call;
        dialedList.appendChild(li);
    });

    missedCalls.forEach(call => {
        let li = document.createElement("li");
        li.textContent = call;
        missedList.appendChild(li);
    });
}

function blockContact(index) {
    contacts[index].blocked = !contacts[index].blocked;
    localStorage.setItem("contacts", JSON.stringify(contacts));
    displayContacts();
}

function searchContact() {
    let value = document.getElementById("search").value.toLowerCase();
    let list = document.getElementById("contactList");
    list.innerHTML = "";

    contacts.filter(c => c.name.toLowerCase().includes(value))
        .forEach(c => {
            let li = document.createElement("li");
            li.innerHTML = `<b>${c.name}</b> - ${c.phone}`;
            list.appendChild(li);
        });
}

displayContacts();
