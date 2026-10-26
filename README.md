# Library-management-project-
Library management interface using html ,css, javascript 
<!DOCTYPE html>  <html lang="en">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <title>Library Management System</title>    <style>  
    * {  
      box-sizing: border-box;  
    }  
  
    body {  
      margin: 0;  
      font-family: Arial, sans-serif;  
      background: #f5f6ff;  
      color: #222641;  
    }  
  
    header {  
      background: white;  
      padding: 20px;  
      display: flex;  
      justify-content: space-between;  
      align-items: center;  
      box-shadow: 0 3px 10px #ddd;  
    }  
  
    header h2 {  
      color: #6558e8;  
      margin: 0;  
    }  
  
    .container {  
      max-width: 1100px;  
      margin: 30px auto;  
      padding: 15px;  
    }  
  
    .stats {  
      display: grid;  
      grid-template-columns: repeat(3, 1fr);  
      gap: 15px;  
      margin-bottom: 20px;  
    }  
  
    .stat, .panel {  
      background: white;  
      padding: 20px;  
      border-radius: 12px;  
      box-shadow: 0 3px 12px #ddd;  
    }  
  
    .stat h3 {  
      color: #6558e8;  
      margin-top: 0;  
    }  
  
    .layout {  
      display: grid;  
      grid-template-columns: 1fr 2fr;  
      gap: 20px;  
    }  
  
    input, select {  
      width: 100%;  
      padding: 11px;  
      margin: 8px 0 14px;  
      border: 1px solid #ddd;  
      border-radius: 7px;  
    }  
  
    button {  
      padding: 10px 13px;  
      border: none;  
      border-radius: 7px;  
      cursor: pointer;  
      margin: 3px;  
    }  
  
    .primary {  
      background: #6558e8;  
      color: white;  
    }  
  
    .danger {  
      background: #ffedf0;  
      color: #c23c52;  
    }  
  
    .secondary {  
      background: #efedff;  
      color: #6558e8;  
    }  
  
    table {  
      width: 100%;  
      border-collapse: collapse;  
      margin-top: 15px;  
    }  
  
    th, td {  
      padding: 12px 8px;  
      border-bottom: 1px solid #eee;  
      text-align: left;  
    }  
  
    th {  
      background: #f5f3ff;  
    }  
  
    .table-wrap {  
      overflow-x: auto;  
    }  
  
    @media (max-width: 750px) {  
      .layout {  
        grid-template-columns: 1fr;  
      }  
  
      .stats {  
        grid-template-columns: 1fr;  
      }  
    }  
  </style>  </head>  <body>  <header>  
  <h2>📚 Library Hub</h2>  
  <button class="secondary" onclick="resetBooks()">  
    Reset Books  
  </button>  
</header>  <div class="container">    <h1>Library Management Interface</h1>  
  <p>Add, search, edit, issue and return books.</p>    <div class="stats">  
    <div class="stat">  
      <h3>Total Books</h3>  
      <h2 id="totalBooks">0</h2>  
    </div>  <div class="stat">  
  <h3>Available Books</h3>  
  <h2 id="availableBooks">0</h2>  
</div>  

<div class="stat">  
  <h3>Issued Books</h3>  
  <h2 id="issuedBooks">0</h2>  
</div>

  </div>    <div class="layout">  <section class="panel">  
  <h2 id="formTitle">Add a Book</h2>  

  <form id="bookForm">  
    <input type="hidden" id="bookId">  

    <label>Book Title</label>  
    <input id="title" required placeholder="Enter book title">  

    <label>Author</label>  
    <input id="author" required placeholder="Enter author name">  

    <label>Category</label>  
    <select id="category" required>  
      <option value="">Choose category</option>  
      <option>Fiction</option>  
      <option>Science</option>  
      <option>Technology</option>  
      <option>History</option>  
      <option>Education</option>  
      <option>Biography</option>  
      <option>Other</option>  
    </select>  

    <label>Published Year</label>  
    <input id="year" type="number" min="1000" max="2100">  

    <button class="primary" type="submit" id="submitBtn">  
      Add Book  
    </button>  

    <button class="secondary" type="button" onclick="cancelEdit()">  
      Cancel  
    </button>  
  </form>  
</section>  

<section class="panel">  
  <h2>Book Collection</h2>  

  <input id="search" placeholder="Search books..."  
         oninput="renderBooks()">  

  <select id="statusFilter" onchange="renderBooks()">  
    <option value="">All Statuses</option>  
    <option value="Available">Available</option>  
    <option value="Issued">Issued</option>  
  </select>  

  <div class="table-wrap">  
    <table>  
      <thead>  
        <tr>  
          <th>Book Details</th>  
          <th>Category</th>  
          <th>Status</th>  
          <th>Actions</th>  
        </tr>  
      </thead>  

      <tbody id="bookTable"></tbody>  
    </table>  
  </div>  
</section>

  </div>  
</div>  <script>  
  let books = JSON.parse(  
    localStorage.getItem("libraryBooks") || "null"  
  ) || [  
    {  
      id: 1,  
      title: "The Alchemist",  
      author: "Paulo Coelho",  
      category: "Fiction",  
      year: 1988,  
      status: "Available",  
      borrower: ""  
    },  
    {  
      id: 2,  
      title: "Clean Code",  
      author: "Robert C. Martin",  
      category: "Technology",  
      year: 2008,  
      status: "Issued",  
      borrower: "Arun"  
    }  
  ];  
  
  function saveBooks() {  
    localStorage.setItem(  
      "libraryBooks",  
      JSON.stringify(books)  
    );  
  }  
  
  function escapeHTML(value) {  
    return String(value ?? "").replace(/[&<>"']/g, char => ({  
      "&": "&amp;",  
      "<": "&lt;",  
      ">": "&gt;",  
      '"': "&quot;",  
      "'": "&#39;"  
    })[char]);  
  }  
  
  function updateStats() {  
    document.getElementById("totalBooks").textContent =  
      books.length;  
  
    document.getElementById("availableBooks").textContent =  
      books.filter(book => book.status === "Available").length;  
  
    document.getElementById("issuedBooks").textContent =  
      books.filter(book => book.status === "Issued").length;  
  }  
  
  function renderBooks() {  
    const search = document.getElementById("search")  
      .value.toLowerCase().trim();  
  
    const status = document.getElementById("statusFilter").value;  
  
    const filteredBooks = books.filter(book => {  
      const matchesSearch =  
        book.title.toLowerCase().includes(search) ||  
        book.author.toLowerCase().includes(search) ||  
        book.category.toLowerCase().includes(search);  
  
      const matchesStatus =  
        status === "" || book.status === status;  
  
      return matchesSearch && matchesStatus;  
    });  
  
    document.getElementById("bookTable").innerHTML =  
      filteredBooks.map(book => `  
        <tr>  
          <td>  
            <strong>${escapeHTML(book.title)}</strong><br>  
            ${escapeHTML(book.author)}  
            ${book.year ? "<br>" + escapeHTML(book.year) : ""}  
            ${book.borrower  
              ? "<br>Borrower: " + escapeHTML(book.borrower)  
              : ""}  
          </td>  
  
          <td>${escapeHTML(book.category)}</td>  
  
          <td>${escapeHTML(book.status)}</td>  
  
          <td>  
            <button class="secondary"  
              onclick="editBook(${book.id})">  
              Edit  
            </button>  
  
            <button class="primary"  
              onclick="toggleBook(${book.id})">  
              ${book.status === "Available" ? "Issue" : "Return"}  
            </button>  
  
            <button class="danger"  
              onclick="deleteBook(${book.id})">  
              Delete  
            </button>  
          </td>  
        </tr>  
      `).join("");  
  
    updateStats();  
  }  
  
  document.getElementById("bookForm")  
    .addEventListener("submit", function(event) {  
      event.preventDefault();  
  
      const id = document.getElementById("bookId").value;  
  
      const bookData = {  
        title: document.getElementById("title").value.trim(),  
        author: document.getElementById("author").value.trim(),  
        category: document.getElementById("category").value,  
        year: document.getElementById("year").value  
      };  
  
      if (id) {  
        const book = books.find(item => item.id === Number(id));  
  
        if (book) {  
          Object.assign(book, bookData);  
        }  
      } else {  
        books.push({  
          id: Date.now(),  
          ...bookData,  
          status: "Available",  
          borrower: ""  
        });  
      }  
  
      saveBooks();  
      cancelEdit();  
      renderBooks();  
    });  
  
  function editBook(id) {  
    const book = books.find(item => item.id === id);  
  
    if (!book) return;  
  
    document.getElementById("bookId").value = book.id;  
    document.getElementById("title").value = book.title;  
    document.getElementById("author").value = book.author;  
    document.getElementById("category").value = book.category;  
    document.getElementById("year").value = book.year || "";  
  
    document.getElementById("formTitle").textContent = "Edit Book";  
    document.getElementById("submitBtn").textContent = "Save Changes";  
  
    window.scrollTo({ top: 0, behavior: "smooth" });  
  }  
  
  function cancelEdit() {  
    document.getElementById("bookForm").reset();  
    document.getElementById("bookId").value = "";  
    document.getElementById("formTitle").textContent = "Add a Book";  
    document.getElementById("submitBtn").textContent = "Add Book";  
  }  
  
  function deleteBook(id) {  
    const book = books.find(item => item.id === id);  
  
    if (!book) return;  
  
    if (confirm("Delete " + book.title + "?")) {  
      books = books.filter(item => item.id !== id);  
      saveBooks();  
      renderBooks();  
    }  
  }  
  
  function toggleBook(id) {  
    const book = books.find(item => item.id === id);  
  
    if (!book) return;  
  
    if (book.status === "Available") {  
      const borrower = prompt("Enter borrower's name:");  
  
      if (borrower === null) return;  
  
      if (!borrower.trim()) {  
        alert("Please enter the borrower's name.");  
        return;  
      }  
  
      book.status = "Issued";  
      book.borrower = borrower.trim();  
    } else {  
      book.status = "Available";  
      book.borrower = "";  
    }  
  
    saveBooks();  
    renderBooks();  
  }  
  
  function resetBooks() {  
    if (confirm("Reset the book list to demo data?")) {  
      books = [  
        {  
          id: 1,  
          title: "The Alchemist",  
          author: "Paulo Coelho",  
          category: "Fiction",  
          year: 1988,  
          status: "Available",  
          borrower: ""  
        },  
        {  
          id: 2,  
          title: "Clean Code",  
          author: "Robert C. Martin",  
          category: "Technology",  
          year: 2008,  
          status: "Issued",  
          borrower: "Arun"  
        }  
      ];  
  
      saveBooks();  
      cancelEdit();  
      renderBooks();  
    }  
  }  
  
  renderBooks();  
</script>  </body>  
</html>  
Entha correct ah kudu and html css javascript tanitanya kudu
