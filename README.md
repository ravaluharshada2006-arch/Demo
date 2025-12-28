<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>E-Student ID Card System</title>

<script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>

<style>
body{
    font-family: Arial;
    background: #e6e6e6;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.student-block{
    background: white;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 20px;
    width: 260px;
}

button{
    margin: 5px 0;
    padding: 6px 12px;
    cursor: pointer;
    width: 180px;
}

.id-card{
    width:220px;
    height:350px;
    background:linear-gradient(180deg,#6a11cb,#2575fc);
    color:white;
    border-radius:12px;
    padding:10px;
    text-align:center;
    margin-top:10px;
}
.photo{
    width:70px;
    height:90px;
    background:white;
    margin:5px auto;
}

.photo img{
    width:100%;
    height:100%;
}

.qr{
    background:white;
    padding:6px;
    width:90px;
    margin:5px auto;
}
</style>
</head>

<body>

<h2>E-Student ID Card & QR System</h2>



<div id="studentsContainer"></div>

<button onclick="addStudent()">➕ Add Student</button>
<button onclick="generateAll()">🎯 Generate ID Cards</button>

<script>
const REQUIRED_FEE = 1000;

function addStudent(){
    const container = document.getElementById("studentsContainer");
    const block = document.createElement("div");
    block.className = "student-block";
    block.innerHTML = `
        ID: <input class="sid"><br>
        Name: <input class="sname"><br>
        Village: <input class="svillage"><br>
        Paid Amount: <input class="samount" type="number"><br>
        Status: <input class="sstatus" readonly><br>
        <div class="idcard"></div>
    `;
    container.appendChild(block);
}

addStudent();

function generateAll(){
    document.querySelectorAll(".student-block").forEach(block=>{
        const id = block.querySelector(".sid").value;
        const name = block.querySelector(".sname").value;
        const village = block.querySelector(".svillage").value;
        const amount = parseFloat(block.querySelector(".samount").value) || 0;

        const status = amount >= REQUIRED_FEE ? "PAID" : "PENDING";
        block.querySelector(".sstatus").value = status;

        const qrData = `
     College: New Satara College Of Engineering Management (Poly.),Korti
     ID: ${id}
     Name: ${name}
     Village: ${village}
     Payment: ${status}
        `;

        block.querySelector(".idcard").innerHTML = `
        <div class="id-card">
            <b>New Satara College Of Engineering Management (Poly.),Korti</b>
            <div class="photo">
                <img src="https://www.shutterstock.com/image-photo/outdoor-photo-indian-college-student-260nw-2500411537.jpg">
            </div><br>
            
            
             <div class="qr">
                <canvas id="qr-${id}"></canvas>
            </div>
            <small>Scan for verification</small>
        </div>
        `;

        const canvas = document.getElementById(`qr-${id}`);
        QRCode.toCanvas(canvas, qrData, { width: 80 });
    });
}
</script>

</body>
</html>
