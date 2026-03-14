<!DOCTYPE html>
<html lang="ht">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FastPay Transfer History</title>
<style>
*{
  box-sizing:border-box;
  font-family:Arial,sans-serif;
}

body{
  margin:0;
  background:#eef2f7;
  padding:20px;
  color:#111827;
}

.container{
  max-width:430px;
  margin:auto;
}

.back{
  display:inline-block;
  margin-bottom:16px;
  color:#ff4d3d;
  text-decoration:none;
  font-weight:bold;
}

.topbar{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:18px;
}

.logo{
  font-size:30px;
  font-weight:bold;
  color:#ff5a3d;
}

.profile{
  width:46px;
  height:46px;
  border-radius:14px;
  background:linear-gradient(135deg,#ff5a3d,#ff7a59);
  color:white;
  display:flex;
  align-items:center;
  justify-content:center;
  font-weight:bold;
  font-size:18px;
  box-shadow:0 6px 16px rgba(255,90,61,0.25);
}

.hero{
  background:linear-gradient(135deg,#111827,#374151);
  color:white;
  border-radius:26px;
  padding:24px;
  margin-bottom:18px;
  box-shadow:0 12px 28px rgba(17,24,39,0.22);
}

.hero h1{
  margin:0 0 10px 0;
  font-size:30px;
  line-height:1.2;
}

.hero p{
  margin:0;
  font-size:15px;
  line-height:1.6;
  opacity:0.98;
}

.section{
  background:white;
  border-radius:22px;
  padding:18px;
  box-shadow:0 8px 18px rgba(0,0,0,0.06);
  margin-bottom:18px;
}

.section h2{
  margin:0 0 14px 0;
  font-size:22px;
}

.stat-grid{
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:12px;
}

.stat-box{
  background:#f8fafc;
  border-radius:16px;
  padding:14px;
}

.stat-title{
  color:#6b7280;
  font-size:13px;
  margin-bottom:6px;
}

.stat-value{
  font-size:20px;
  font-weight:bold;
  color:#111827;
}

.transfer-item{
  padding:12px 0;
  border-bottom:1px solid #eee;
}

.transfer-item:last-child{
  border-bottom:none;
}

.transfer-top{
  display:flex;
  justify-content:space-between;
  gap:12px;
  align-items:flex-start;
}

.transfer-name{
  font-weight:bold;
  font-size:15px;
}

.transfer-amount{
  font-weight:bold;
  font-size:15px;
  text-align:right;
}

.transfer-date{
  color:#6b7280;
  font-size:13px;
  margin-top:4px;
}

.transfer-sub{
  color:#6b7280;
  font-size:13px;
  margin-top:4px;
  line-height:1.45;
}

.status{
  display:inline-block;
  margin-top:6px;
  background:#fff7ed;
  color:#c2410c;
  border-radius:999px;
  padding:5px 10px;
  font-size:12px;
  font-weight:bold;
}

.empty{
  color:#6b7280;
  font-size:14px;
}

.main-btn{
  display:block;
  width:100%;
  text-align:center;
  padding:15px;
  border-radius:16px;
  text-decoration:none;
  font-weight:bold;
  font-size:16px;
  margin-top:12px;
}

.orange-btn{
  background:#ff4d3d;
  color:white;
}

.dark-btn{
  background:#111827;
  color:white;
}
</style>
</head>
<body>

<div class="container">

<a href="wallet.html" class="back">← Back Wallet</a>

<div class="topbar">
  <div class="logo">FastPay</div>
  <div class="profile" id="profileLetter">L</div>
</div>

<div class="hero">
  <h1>Transfer History</h1>
  <p>Isit la ou ka wè tout transfè ou fè yo ak dènye estati yo.</p>
</div>

<div class="section">
  <h2>Rezime Transfè</h2>

  <div class="stat-grid">
    <div class="stat-box">
      <div class="stat-title">Total Transfers</div>
      <div class="stat-value" id="totalTransfers">0</div>
    </div>

    <div class="stat-box">
      <div class="stat-title">Last Amount</div>
      <div class="stat-value" id="lastAmount">0</div>
    </div>

    <div class="stat-box">
      <div class="stat-title">Last Receiver</div>
      <div class="stat-value" id="lastReceiver">Pa gen</div>
    </div>

    <div class="stat-box">
      <div class="stat-title">Last Status</div>
      <div class="stat-value" id="lastStatus">N/A</div>
    </div>
  </div>
</div>

<div class="section">
  <h2>Tout transfè yo</h2>
  <div id="transferBox" class="empty">Pa gen transfè pou kounye a.</div>

  <a href="transfer.html" class="main-btn orange-btn">Nouvo Transfer</a>
  <a href="dashboard.html" class="main-btn dark-btn">Ale sou Dashboard</a>
</div>

</div>

<script>
const savedUser = JSON.parse(localStorage.getItem("fastpay_user"));
const transfers = JSON.parse(localStorage.getItem("fastpay_transfers")) || [];

if(savedUser && savedUser.fullname){
  document.getElementById("profileLetter").innerHTML =
    savedUser.fullname.charAt(0).toUpperCase();
}

document.getElementById("totalTransfers").innerHTML = transfers.length;

if(transfers.length > 0){
  document.getElementById("lastAmount").innerHTML = transfers[0].amount || "0";
  document.getElementById("lastReceiver").innerHTML = transfers[0].receiverName || "Pa gen";
  document.getElementById("lastStatus").innerHTML = transfers[0].status || "Pending";
}

function loadTransfers(){
  const box = document.getElementById("transferBox");

  if(transfers.length === 0){
    box.innerHTML = "Pa gen transfè pou kounye a.";
    return;
  }

  box.innerHTML = "";

  transfers.forEach(function(item){
    box.innerHTML += `
      <div class="transfer-item">
        <div class="transfer-top">
          <div>
            <div class="transfer-name">${item.receiverName || 'Receiver'}</div>
            <div class="transfer-date">${item.date || ''}</div>
          </div>
          <div class="transfer-amount">${item.amount || '0'}</div>
        </div>

        <div class="transfer-sub"><strong>Type:</strong> ${item.transferType || 'N/A'}</div>
        <div class="transfer-sub"><strong>Receiver ID:</strong> ${item.receiverId || 'N/A'}</div>
        <div class="transfer-sub"><strong>Currency:</strong> ${item.currency || 'N/A'}</div>
        <div class="transfer-sub"><strong>Details:</strong> ${item.details || 'N/A'}</div>
        <div class="status">${item.status || 'Pending Verification'}</div>
      </div>
    `;
  });
}

loadTransfers();
</script>

</body>
</html># transfer-history.html
