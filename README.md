function toggleSidebar() {
  const sidebar = document.getElementById("sidebar");
  const menuButton = document.getElementById("menuButton");
  if (sidebar.style.left === "-250px") {
    sidebar.style.left = "0";
    menuButton.style.left = "240px";
  } else {
    sidebar.style.left = "-250px";
    menuButton.style.left = "10px";
  }
}

function showSection(section) {
  document.querySelectorAll('.section').forEach(sec => sec.classList.add('hidden'));
  document.getElementById(section + "Section").classList.remove("hidden");
}

function updatePic(event) {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = () => {
    localStorage.setItem("profilePic", reader.result);
    loadProfile();
  };
  if (file) reader.readAsDataURL(file);
}

function saveProfile() {
  localStorage.setItem("userName", document.getElementById("userName").value);
  localStorage.setItem("userMobile", document.getElementById("userMobile").value);
  localStorage.setItem("userEmail", document.getElementById("userEmail").value);
  alert("প্রোফাইল সংরক্ষণ হয়েছে!");
  loadProfile();
}

function loadProfile() {
  document.getElementById("userName").value = localStorage.getItem("userName") || "";
  document.getElementById("userMobile").value = localStorage.getItem("userMobile") || "";
  document.getElementById("userEmail").value = localStorage.getItem("userEmail") || "";

  document.getElementById("displayName").innerText = "নাম: " + (localStorage.getItem("userName") || "");
  document.getElementById("displayMobile").innerText = "মোবাইল: " + (localStorage.getItem("userMobile") || "");
  document.getElementById("displayEmail").innerText = "ইমেইল: " + (localStorage.getItem("userEmail") || "");
  document.getElementById("displayPic").src = localStorage.getItem("profilePic") || "profile.jpg";
}

function saveTask() {
  const date = document.getElementById("taskDate").value;
  const day = document.getElementById("daySelect").value;
  const pieces = parseInt(document.getElementById("taskPieces").value);
  const rate = parseInt(document.getElementById("taskRate").value);
  const money = pieces * rate;

  if (date && day && !isNaN(pieces) && !isNaN(rate)) {
    let tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
    tasks.push({ date, day, pieces, rate, money });
    localStorage.setItem("tasks", JSON.stringify(tasks));
    loadTasks();
    loadTodayTask();
    alert("আজকের কাজ যুক্ত হয়েছে!");
  }
}

function loadTodayTask() {
  let tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
  if (tasks.length > 0) {
    const last = tasks[tasks.length - 1];
    document.getElementById("displayDate").innerText = last.date;
    document.getElementById("displayDay").innerText = last.day;
    document.getElementById("displayPieces").innerText = last.pieces;
    document.getElementById("displayRate").innerText = last.rate;
    document.getElementById("displayTotalMoney").innerText = last.money;
  }
}

function loadTasks() {
  let tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
  const body = document.getElementById("taskBody");
  body.innerHTML = "";
  let totalPieces = 0;
  let totalMoney = 0;

  tasks.forEach(task => {
    body.innerHTML += `<tr><td>${task.date}</td><td>${task.day}</td><td>${task.pieces}</td><td>${task.rate}</td><td>${task.money}</td></tr>`;
    totalPieces += task.pieces;
    totalMoney += task.money;
  });

  document.getElementById("totalPieces").innerText = totalPieces;
  document.getElementById("totalMoney").innerText = totalMoney;
}

function clearAllTasks() {
  if (confirm("আপনি কি সব কাজ মুছে ফেলতে চান?")) {
    localStorage.removeItem("tasks");
    loadTasks();
    loadTodayTask();
  }
}

function editTasks() {
  alert("Edit অপশন ভবিষ্যতে যুক্ত হবে। আপাতত কেবল Clear করুন।");
}

window.onload = () => {
  loadProfile();
  loadTasks();
  loadTodayTask();
  showSection("home");
};
