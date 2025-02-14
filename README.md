# QLCT1
Quản lý chi tiêu 



Mô tả dự án: Quản Lý Chi Tiêu

Tổng quan

Ứng dụng Quản Lý Chi Tiêu là một công cụ giúp người dùng theo dõi thu nhập và chi tiêu cá nhân một cách dễ dàng. Dữ liệu được lưu trữ trên trình duyệt (localStorage) để không bị mất khi tải lại trang.



Tính năng chính

Thêm giao dịch (thu nhập hoặc chi tiêu)

 Chọn danh mục giao dịch (Ăn uống, Mua sắm, Giải trí, v.v.)

 Hiển thị số dư hiện tại

 Xóa giao dịch không cần thiết

 Lưu trữ dữ liệu với localStorage để giữ lại sau khi tải lại trang



Công nghệ sử dụng

HTML: Xây dựng giao diện trang web

CSS: Thiết kế và tạo phong cách cho ứng dụng

JavaScript: Xử lý logic, thao tác dữ liệu, cập nhật giao diện

LocalStorage: Lưu trữ dữ liệu giao dịch trên trình duyệt.

CODE: 
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản Lý Chi Tiêu</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }
        .container {
            max-width: 400px;
            margin: auto;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 10px;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
        }
        input, select, button {
            width: 100%;
            margin: 5px 0;
            padding: 8px;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            display: flex;
            justify-content: space-between;
            padding: 5px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Quản Lý Chi Tiêu</h2>
        <p>Số dư hiện tại: <strong id="balance">0</strong> VND</p>
        <input type="number" id="amount" placeholder="Nhập số tiền" />
        <select id="category">
            <option value="income">Thu nhập</option>
            <option value="food">Ăn uống</option>
            <option value="shopping">Mua sắm</option>
            <option value="entertainment">Giải trí</option>
        </select>
        <button onclick="addTransaction()">Thêm giao dịch</button>
        <ul id="transactionList"></ul>
    </div>

    <script>
        let balance = 0;
        let transactions = JSON.parse(localStorage.getItem("transactions")) || [];

        function updateUI() {
            document.getElementById("transactionList").innerHTML = "";
            balance = 0;
            transactions.forEach((t, index) => {
                balance += t.type === "income" ? t.amount : -t.amount;
                let li = document.createElement("li");
                li.innerHTML = `${t.category}: ${t.amount} VND <button onclick="removeTransaction(${index})">X</button>`;
                document.getElementById("transactionList").appendChild(li);
            });
            document.getElementById("balance").textContent = balance;
            localStorage.setItem("transactions", JSON.stringify(transactions));
        }

        function addTransaction() {
            let amount = parseInt(document.getElementById("amount").value);
            let category = document.getElementById("category").value;
            if (!amount || amount <= 0) return alert("Nhập số tiền hợp lệ!");
            transactions.push({ amount, category, type: category === "income" ? "income" : "expense" });
            updateUI();
        }

        function removeTransaction(index) {
            transactions.splice(index, 1);
            updateUI();
        }

        updateUI();
    </script>
</body>
</html>
