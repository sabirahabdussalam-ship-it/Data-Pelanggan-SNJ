DATA PELANGGAN SRN
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PD-PELANGGAN | Cloud System</title>
    <!-- Desain UI Modern (Bootstrap) -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f0f2f5; font-family: sans-serif; }
        .navbar { background-color: #003366; border-bottom: 3px solid #ffc107; }
        .card { border-radius: 15px; border: none; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    </style>
</head>
<body>

<nav class="navbar navbar-dark p-3 shadow-sm">
    <div class="container">
        <a class="navbar-brand fw-bold" href="#">🏛️ PANGKALAN DATA CLOUD</a>
    </div>
</nav>

<div class="container my-5">
    <div class="row g-4">
        <!-- FORM INPUT -->
        <div class="col-lg-4">
            <div class="card p-4">
                <h5 class="fw-bold text-primary mb-3">Input Transaksi</h5>
                <form id="formInput">
                    <input type="text" id="nama" class="form-control mb-2" placeholder="Nama Pelanggan" required>
                    <input type="text" id="telp" class="form-control mb-2" placeholder="No. Telepon">
                    <input type="text" id="npwp" class="form-control mb-2" placeholder="NPWP">
                    <textarea id="barang" class="form-control mb-2" placeholder="Barang yang dibeli"></textarea>
                    <select id="status" class="form-select mb-3">
                        <option>Reguler</option>
                        <option>Premium</option>
                        <option>VIP</option>
                    </select>
                    <button type="submit" class="btn btn-primary w-100 fw-bold">SIMPAN DATA</button>
                </form>
            </div>
        </div>

        <!-- TABEL DATA -->
        <div class="col-lg-8">
            <div class="card overflow-hidden">
                <table class="table table-hover mb-0">
                    <thead class="table-dark">
                        <tr>
                            <th>Profil</th>
                            <th>NPWP</th>
                            <th>Barang</th>
                            <th>Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="tabelBody">
                        <!-- Data Muncul Otomatis -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<!-- KONEKSI FIREBASE -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
  import { getDatabase, ref, push, onValue, remove } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

  // MASUKKAN KODE FIREBASE CONFIG KAMU DI SINI
  const firebaseConfig = {
    apiKey: "MASUKKAN_API_KEY",
    authDomain: "PROYEK_KAMU.firebaseapp.com",
    databaseURL: "https://PROYEK_KAMU.firebaseio.com",
    projectId: "PROYEK_KAMU",
    storageBucket: "PROYEK_KAMU.appspot.com",
    messagingSenderId: "...",
    appId: "..."
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);
  const dbRef = ref(db, 'pelanggan');

  // FUNGSI SIMPAN
  document.getElementById('formInput').addEventListener('submit', (e) => {
    e.preventDefault();
    push(dbRef, {
      nama: document.getElementById('nama').value,
      telp: document.getElementById('telp').value,
      npwp: document.getElementById('npwp').value,
      No rekening : document.getElementById('No rekening').value,
    });
    document.getElementById('formInput').reset();
  });

  // FUNGSI TAMPIL REALTIME
  onValue(dbRef, (snapshot) => {
    const tabel = document.getElementById('tabelBody');
    tabel.innerHTML = "";
    snapshot.forEach((child) => {
      const d = child.val();
      tabel.innerHTML += `
        <tr>
          <td><strong>${d.nama}</strong><br><small>${d.telp}</small></td>
          <td>${d.npwp}</td>
          <td><span class="badge bg-info text-dark">${d.barang}</span></td>
          <td><button onclick="hapus('${child.key}')" class="btn btn-sm btn-danger">Hapus</button></td>
        </tr>`;
    });
  });

  // FUNGSI HAPUS
  window.hapus = (key) => {
    if(confirm('Hapus data?')) remove(ref(db, 'pelanggan/' + key));
  }
</script>
</body>
</html>
