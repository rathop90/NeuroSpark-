document.addEventListener("DOMContentLoaded", function () {
    loadBooks();
    checkAdmin();
});

function loadBooks() {
    let bookList = document.getElementById("book-list");
    let books = JSON.parse(localStorage.getItem("books")) || [];
    let isAdmin = localStorage.getItem("admin") === "true";

    bookList.innerHTML = "";
    books.forEach((book, index) => {
        let bookDiv = document.createElement("div");
        bookDiv.classList.add("book");
        bookDiv.innerHTML = `<p>${book}</p>` + 
            (isAdmin ? `<button onclick="deleteBook(${index})">❌ Delete</button>` : "");
        bookList.appendChild(bookDiv);
    });
}

function uploadBook() {
    let fileInput = document.getElementById("book-upload");
    if (fileInput.files.length === 0) {
        alert("Please select a file!");
        return;
    }

    let fileName = fileInput.files[0].name;
    let books = JSON.parse(localStorage.getItem("books")) || [];
    books.push(fileName);
    localStorage.setItem("books", JSON.stringify(books));

    loadBooks();
}

function deleteBook(index) {
    let books = JSON.parse(localStorage.getItem("books"));
    books.splice(index, 1);
    localStorage.setItem("books", JSON.stringify(books));
    loadBooks();
}

function adminLogin() {
    let password = prompt("Enter Admin Password:");
    if (password === "051085") {  // ✅ Fixed Password
        localStorage.setItem("admin", "true");
        alert("Admin Mode Activated!");
        checkAdmin();
        loadBooks();
    } else {
        alert("Wrong Password!");
    }
}

function adminLogout() {
    localStorage.removeItem("admin");
    alert("Admin Mode Deactivated!");
    checkAdmin();
    loadBooks();
}

function checkAdmin() {
    let isAdmin = localStorage.getItem("admin") === "true";
    document.getElementById("admin-section").style.display = isAdmin ? "block" : "none";
    document.getElementById("logout-btn").style.display = isAdmin ? "inline-block" : "none";
    document.getElementById("login-btn").style.display = isAdmin ? "none" : "inline-block";
}
