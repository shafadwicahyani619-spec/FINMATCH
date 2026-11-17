# FINMATCH
<!DOCTYPE html>
<html lang="id" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FinMatch - Smart Financial & Loan Assistant</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        navy: '#003366',
                        deepBlue: '#00509e',
                        primaryBlue: '#007acc',
                        skyBlue: '#66a3ff',
                        lightBlue: '#cce0ff',
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Chart.js for Visualizations -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- jsPDF for Report Download -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 h-full flex flex-col antialiased selection:bg-lightBlue selection:text-navy">

    <!-- Top Header (Desktop & Mobile Bar) -->
    <header class="bg-navy text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="switchTab('home')">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-primaryBlue to-skyBlue flex items-center justify-center shadow-inner text-white font-bold text-xl">
                    <i class="fa-solid fa-scale-balanced"></i>
                </div>
                <div>
                    <span class="text-lg font-bold tracking-tight text-white">Fin<span class="text-skyBlue">Match</span></span>
                    <span class="block text-[10px] text-skyBlue uppercase tracking-widest font-medium">Smart Financial & Loan Assistant</span>
                </div>
            </div>
            <!-- Desktop Navigation -->
            <nav class="hidden md:flex items-center space-x-1 lg:space-x-2 text-sm font-medium">
                <button onclick="switchTab('home')" class="nav-btn px-3 py-2 rounded-lg transition text-white hover:bg-deepBlue flex items-center space-x-2 active-nav" data-target="home">
                    <i class="fa-solid fa-house w-4"></i><span>Beranda</span>
                </button>
                <button onclick="switchTab('simulation')" class="nav-btn px-3 py-2 rounded-lg transition text-slate-200 hover:bg-deepBlue flex items-center space-x-2" data-target="simulation">
                    <i class="fa-solid fa-calculator w-4"></i><span>Simulasi Anuitas</span>
                </button>
                <button onclick="switchTab('institutions')" class="nav-btn px-3 py-2 rounded-lg transition text-slate-200 hover:bg-deepBlue flex items-center space-x-2" data-target="institutions">
                    <i class="fa-solid fa-building-columns w-4"></i><span>Lembaga & Produk</span>
                </button>
                <button onclick="switchTab('history')" class="nav-btn px-3 py-2 rounded-lg transition text-slate-200 hover:bg-deepBlue flex items-center space-x-2" data-target="history">
                    <i class="fa-solid fa-clock-rotate-left w-4"></i><span>Riwayat</span>
                </button>
                <button onclick="switchTab('about')" class="nav-btn px-3 py-2 rounded-lg transition text-slate-200 hover:bg-deepBlue flex items-center space-x-2" data-target="about">
                    <i class="fa-solid fa-circle-info w-4"></i><span>Tentang</span>
                </button>
                <button onclick="switchTab('finfin')" class="nav-btn px-3 py-2 rounded-lg transition text-slate-200 hover:bg-deepBlue flex items-center space-x-2 relative" data-target="finfin">
                    <i class="fa-solid fa-robot w-4 text-skyBlue"></i><span>Finfin AI</span>
                    <span class="absolute -top-1 -right-1 w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                </button>
                <button onclick="switchTab('profile')" class="nav-btn p-2 rounded-lg transition text-slate-200 hover:bg-deepBlue" data-target="profile" title="Profil Pengguna">
                    <i class="fa-solid fa-user-gear text-lg"></i>
                </button>
            </nav>
            <!-- Mobile Menu Toggle Button -->
            <button onclick="toggleMobileMenu()" class="md:hidden text-white p-2 rounded-lg hover:bg-deepBlue focus:outline-none">
                <i class="fa-solid fa-bars text-xl" id="mobile-menu-icon"></i>
            </button>
        </div>
        <!-- Mobile Dropdown Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-navy border-t border-deepBlue px-4 py-3 space-y-2">
            <button onclick="switchTab('home'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-house w-5 text-skyBlue"></i><span>Beranda</span>
            </button>
            <button onclick="switchTab('simulation'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-calculator w-5 text-skyBlue"></i><span>Simulasi Anuitas</span>
            </button>
            <button onclick="switchTab('institutions'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-building-columns w-5 text-skyBlue"></i><span>Lembaga & Produk</span>
            </button>
            <button onclick="switchTab('history'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-clock-rotate-left w-5 text-skyBlue"></i><span>Riwayat Simulasi</span>
            </button>
            <button onclick="switchTab('about'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-circle-info w-5 text-skyBlue"></i><span>Tujuan & Fungsi</span>
            </button>
            <button onclick="switchTab('finfin'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-robot w-5 text-skyBlue"></i><span>Finfin (AI & Admin Chat)</span>
            </button>
            <button onclick="switchTab('profile'); toggleMobileMenu();" class="w-full text-left px-3 py-2 rounded-lg hover:bg-deepBlue flex items-center space-x-3 text-sm font-medium">
                <i class="fa-solid fa-user-gear w-5 text-skyBlue"></i><span>Profil & Pengaturan</span>
            </button>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 pb-24 md:pb-8">

        <!-- ================= 1. BERANDA (HOME) ================= -->
        <section id="tab-home" class="tab-content active space-y-8">
            <!-- Hero Banner -->
            <div class="bg-gradient-to-r from-navy via-deepBlue to-primaryBlue rounded-3xl p-6 sm:p-10 text-white shadow-xl relative overflow-hidden flex flex-col lg:flex-row items-center justify-between">
                <div class="absolute -right-16 -bottom-16 w-72 h-72 rounded-full bg-skyBlue/20 blur-3xl pointer-events-none"></div>
                <div class="max-w-2xl space-y-4 z-10 text-center lg:text-left">
                    <span class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-skyBlue/20 text-skyBlue text-xs font-semibold tracking-wide uppercase border border-skyBlue/30">
                        <i class="fa-solid fa-shield-halved"></i> Smart Financial & Loan Assistant
                    </span>
                    <h1 class="text-3xl sm:text-5xl font-bold tracking-tight leading-tight">
                        Find the Right Loan, <span class="text-skyBlue">Match Your Financial Ability.</span>
                    </h1>
                    <p class="text-slate-200 text-base sm:text-lg leading-relaxed">
                        Analisis kesehatan finansial, simulasi cicilan pinjaman berbasis anuitas, dan dapatkan rekomendasi lembaga keuangan terpercaya sebelum Anda mengajukan kredit.
                    </p>
                    <div class="pt-4 flex flex-wrap gap-3 justify-center lg:justify-start">
                        <button onclick="switchTab('simulation')" class="px-6 py-3.5 bg-primaryBlue hover:bg-skyBlue hover:text-navy text-white font-semibold rounded-xl shadow-lg transition duration-200 flex items-center space-x-2">
                            <i class="fa-solid fa-calculator"></i><span>Mulai Simulasi Sekarang</span>
                        </button>
                        <button onclick="switchTab('institutions')" class="px-6 py-3.5 bg-white/10 hover:bg-white/25 text-white font-semibold rounded-xl backdrop-blur-md transition duration-200 flex items-center space-x-2 border border-white/20">
                            <i class="fa-solid fa-building-columns"></i><span>Cari Lembaga & Produk</span>
                        </button>
                    </div>
                </div>
                <div class="mt-8 lg:mt-0 z-10 w-full max-w-sm lg:max-w-md">
                    <div class="bg-white/10 backdrop-blur-md p-6 rounded-2xl border border-white/20 shadow-2xl space-y-4">
                        <div class="flex items-center justify-between border-b border-white/10 pb-3">
                            <span class="text-sm font-medium text-slate-200">Simulasi Cepat Anuitas</span>
                            <span class="text-xs bg-emerald-500/20 text-emerald-300 px-2 py-0.5 rounded font-medium">Otomatis</span>
                        </div>
                        <div class="space-y-3 text-sm">
                            <div class="flex justify-between items-center bg-navy/40 p-3 rounded-xl">
                                <span class="text-slate-300">Batas Max Cicilan (40%)</span>
                                <span class="font-bold text-skyBlue" id="hero-max-limit">Rp 2.000.000</span>
                            </div>
                            <div class="flex justify-between items-center bg-navy/40 p-3 rounded-xl">
                                <span class="text-slate-300">Rekomendasi Utama</span>
                                <span class="font-bold text-emerald-300">Bank Konvensional & Syariah</span>
                            </div>
                        </div>
                        <button onclick="switchTab('simulation')" class="w-full py-2.5 bg-white text-navy font-bold rounded-xl hover:bg-lightBlue transition text-sm shadow">
                            Buka Kalkulator FinMatch
                        </button>
                    </div>
                </div>
            </div>

            <!-- Core Feature Summaries -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 hover:shadow-md transition space-y-3">
                    <div class="w-12 h-12 rounded-xl bg-lightBlue text-primaryBlue flex items-center justify-center text-xl font-bold">
                        <i class="fa-solid fa-chart-pie"></i>
                    </div>
                    <h3 class="text-lg font-bold text-navy">Analisis Kapasitas 40%</h3>
                    <p class="text-sm text-slate-600 leading-relaxed">
                        Sistem secara otomatis menghitung batas aman cicilan bulanan Anda (maksimal 40% dari penghasilan bersih dikurangi cicilan aktif).
                    </p>
                </div>
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 hover:shadow-md transition space-y-3">
                    <div class="w-12 h-12 rounded-xl bg-lightBlue text-primaryBlue flex items-center justify-center text-xl font-bold">
                        <i class="fa-solid fa-calculator"></i>
                    </div>
                    <h3 class="text-lg font-bold text-navy">Simulasi Anuitas Presisi</h3>
                    <p class="text-sm text-slate-600 leading-relaxed">
                        Perhitungan cicilan per bulan, rincian total bunga, dan total pembayaran transparan berdasarkan suku bunga efektif tahunan.
                    </p>
                </div>
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 hover:shadow-md transition space-y-3">
                    <div class="w-12 h-12 rounded-xl bg-lightBlue text-primaryBlue flex items-center justify-center text-xl font-bold">
                        <i class="fa-solid fa-robot"></i>
                    </div>
                    <h3 class="text-lg font-bold text-navy">Konsultasi AI Finfin</h3>
                    <p class="text-sm text-slate-600 leading-relaxed">
                        Tanyakan apa saja seputar risiko kredit, alternatif pinjaman, atau perbedaan bank dan koperasi kepada asisten cerdas Finfin.
                    </p>
                </div>
            </div>

            <!-- Annuity Brief & Info Box -->
            <div class="bg-lightBlue/60 border border-skyBlue/30 rounded-2xl p-6 sm:p-8 flex flex-col md:flex-row items-center justify-between gap-6">
                <div class="space-y-3">
                    <span class="text-xs font-bold text-deepBlue uppercase tracking-wider bg-white px-3 py-1 rounded-full shadow-sm">Edukasi Keuangan</span>
                    <h2 class="text-2xl font-bold text-navy">Apa itu Prinsip Anuitas dalam Pinjaman?</h2>
                    <p class="text-sm text-slate-700 leading-relaxed max-w-3xl">
                        Metode anuitas adalah sistem perhitungan cicilan di mana jumlah angsuran per bulan adalah <strong>tetap</strong> (konstan). Dalam setiap cicilan bulanan tersebut, porsi bunga pada awalnya besar dan porsi pokok kecil, yang perlahan bergeser hingga akhir masa pinjaman di mana porsi pokok menjadi lebih besar.
                    </p>
                </div>
                <button onclick="switchTab('about')" class="whitespace-nowrap px-5 py-3 bg-navy hover:bg-deepBlue text-white font-semibold rounded-xl text-sm shadow transition">
                    Pelajari Selengkapnya
                </button>
            </div>
        </section>

        <!-- ================= 2. SIMULASI ANUITAS (LOAN SIMULATION) ================= -->
        <section id="tab-simulation" class="tab-content space-y-8">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-100">
                <div class="border-b border-slate-100 pb-4 mb-6">
                    <h2 class="text-2xl font-bold text-navy">Kalkulator Simulasi & Analisis Pinjaman</h2>
                    <p class="text-sm text-slate-500 mt-1">Masukkan profil keuangan dan kebutuhan pinjaman Anda untuk pengujian kelayakan kredit.</p>
                </div>

                <form id="simulation-form" onsubmit="handleSimulation(event)" class="space-y-8">
                    <!-- Step 1: Financial Profile -->
                    <div class="space-y-4">
                        <h3 class="text-base font-semibold text-deepBlue flex items-center gap-2">
                            <span class="w-7 h-7 rounded-lg bg-lightBlue text-navy flex items-center justify-center text-sm font-bold">1</span>
                            Profil Keuangan Pengguna
                        </h3>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Penghasilan Bersih per Bulan (Rp) *</label>
                                <input type="text" id="net-income" required placeholder="Contoh: 5.000.000" oninput="formatRupiahInput(this)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Pengeluaran Rutin per Bulan (Rp) *</label>
                                <input type="text" id="routine-expenses" required placeholder="Contoh: 2.500.000" oninput="formatRupiahInput(this)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Total Estimasi Aset (Rp)</label>
                                <input type="text" id="total-assets" placeholder="Contoh: 50.000.000" oninput="formatRupiahInput(this)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Jenis Aset Utama</label>
                                <select id="asset-type" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm bg-white">
                                    <option value="Tabungan">Tabungan / Kas</option>
                                    <option value="Deposito">Deposito</option>
                                    <option value="Kendaraan">Kendaraan (Motor/Mobil)</option>
                                    <option value="Tanah/Bangunan">Tanah / Bangunan / Rumah</option>
                                    <option value="Emas">Emas / Logam Mulia</option>
                                    <option value="Lainnya">Lainnya</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Punya Pinjaman Lain?</label>
                                <select id="has-active-loan" onchange="toggleActiveLoanFields()" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm bg-white">
                                    <option value="no">Tidak</option>
                                    <option value="yes">Ya</option>
                                </select>
                            </div>
                            <div id="active-loan-container" class="hidden">
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Total Cicilan Aktif / Bulan (Rp)</label>
                                <input type="text" id="active-loan-amount" placeholder="Contoh: 500.000" oninput="formatRupiahInput(this)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm" value="0">
                            </div>
                        </div>
                    </div>

                    <!-- Step 2: Loan Requirements -->
                    <div class="space-y-4 pt-4 border-t border-slate-100">
                        <h3 class="text-base font-semibold text-deepBlue flex items-center gap-2">
                            <span class="w-7 h-7 rounded-lg bg-lightBlue text-navy flex items-center justify-center text-sm font-bold">2</span>
                            Kebutuhan Pinjaman
                        </h3>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Nominal Pinjaman Diajukan (Rp) *</label>
                                <input type="text" id="loan-amount" required placeholder="Contoh: 20.000.000" oninput="formatRupiahInput(this)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Tenor Pinjaman (Bulan) *</label>
                                <input type="number" id="loan-tenor" required min="1" max="360" placeholder="Contoh: 12" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Suku Bunga Efektif (% p.a) *</label>
                                <input type="number" id="annual-interest" required step="0.01" min="0" max="100" value="11.5" placeholder="Contoh: 11.5" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Jenis Pinjaman</label>
                                <select id="loan-type" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm bg-white">
                                    <option value="KTA">Kredit Tanpa Agunan (KTA)</option>
                                    <option value="Multiguna">Kredit Multiguna</option>
                                    <option value="Kendaraan">Kredit Kendaraan (KKB)</option>
                                    <option value="KPR">Kredit Kepemilikan Rumah (KPR)</option>
                                    <option value="Usaha">Kredit Usaha / UMKM</option>
                                    <option value="Koperasi">Pinjaman Koperasi</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="pt-4 flex justify-end">
                        <button type="submit" class="w-full sm:w-auto px-8 py-3.5 bg-navy hover:bg-primaryBlue text-white font-semibold rounded-xl shadow-lg transition duration-200 flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-calculator"></i><span>Hitung & Analisis Kemampuan</span>
                        </button>
                    </div>
                </form>
            </div>

            <!-- Results Section (Initially Hidden or Placeholder) -->
            <div id="simulation-results" class="hidden space-y-8">
                <!-- Status Banner -->
                <div id="health-status-banner" class="p-6 rounded-3xl shadow-md border flex flex-col md:flex-row items-center justify-between gap-4">
                    <div class="flex items-center space-x-4">
                        <div id="health-icon" class="w-14 h-14 rounded-2xl flex items-center justify-center text-2xl font-bold shadow">
                            <i class="fa-solid fa-check"></i>
                        </div>
                        <div>
                            <span class="text-xs font-bold uppercase tracking-wider opacity-80">Status Kemampuan Finansial</span>
                            <h3 id="health-title" class="text-2xl font-bold">Aman</h3>
                            <p id="health-desc" class="text-sm opacity-90 mt-1">Cicilan berada dalam batas kemampuan dan sisa pendapatan masih memadai.</p>
                        </div>
                    </div>
                    <button onclick="exportToPDF()" class="px-5 py-2.5 bg-white text-navy font-bold rounded-xl shadow hover:bg-lightBlue transition text-sm flex items-center space-x-2 whitespace-nowrap">
                        <i class="fa-solid fa-file-pdf text-red-500"></i><span>Unduh Laporan PDF</span>
                    </button>
                </div>

                <!-- Financial Metrics Cards -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Max Cicilan (40% Bersih)</span>
                        <h4 class="text-2xl font-bold text-navy" id="res-max-installment">Rp 0</h4>
                        <p class="text-xs text-slate-500" id="res-active-note">Cicilan aktif: Rp 0</p>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Estimasi Cicilan Baru</span>
                        <h4 class="text-2xl font-bold text-primaryBlue" id="res-monthly-payment">Rp 0</h4>
                        <p class="text-xs text-slate-500">Per bulan (Anuitas)</p>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Total Bunga Pinjaman</span>
                        <h4 class="text-2xl font-bold text-amber-600" id="res-total-interest">Rp 0</h4>
                        <p class="text-xs text-slate-500">Selama masa tenor</p>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Sisa Pendapatan Bersih</span>
                        <h4 class="text-2xl font-bold text-emerald-600" id="res-remaining-income">Rp 0</h4>
                        <p class="text-xs text-slate-500">Setelah pengeluaran & cicilan</p>
                    </div>
                </div>

                <!-- Detailed Breakdown & Chart -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-4 lg:col-span-2">
                        <h3 class="text-lg font-bold text-navy flex items-center gap-2">
                            <i class="fa-solid fa-chart-line text-primaryBlue"></i> Rincian Pembayaran & Progres
                        </h3>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div class="p-4 rounded-xl bg-slate-50 space-y-2">
                                <span class="text-xs text-slate-500 block">Total Pembayaran (Pokok + Bunga)</span>
                                <span class="text-xl font-bold text-navy" id="res-total-payment">Rp 0</span>
                            </div>
                            <div class="p-4 rounded-xl bg-slate-50 space-y-2">
                                <span class="text-xs text-slate-500 block">Rasio Cicilan terhadap Pendapatan</span>
                                <span class="text-xl font-bold text-primaryBlue" id="res-installment-ratio">0%</span>
                            </div>
                        </div>
                        <!-- Progress bar -->
                        <div class="space-y-1">
                            <div class="flex justify-between text-xs font-semibold text-slate-600">
                                <span>Persentase Beban Cicilan</span>
                                <span id="ratio-percent-text">0%</span>
                            </div>
                            <div class="w-full bg-slate-200 rounded-full h-3 overflow-hidden">
                                <div id="ratio-progress-bar" class="bg-primaryBlue h-3 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>

                    <!-- Chart Container -->
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center justify-center">
                        <h3 class="text-sm font-bold text-navy mb-2 self-start">Komposisi Pinjaman</h3>
                        <div class="w-48 h-48 relative">
                            <canvas id="loanPieChart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Alternative Recommendations (if capacity exceeded) -->
                <div id="alternative-section" class="bg-amber-50 border border-amber-200 rounded-2xl p-6 hidden space-y-4">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 rounded-xl bg-amber-500 text-white flex items-center justify-center text-lg font-bold">
                            <i class="fa-solid fa-lightbulb"></i>
                        </div>
                        <div>
                            <h4 class="text-base font-bold text-amber-900">Saran Alternatif Pinjaman</h4>
                            <p class="text-xs text-amber-700">Nominal atau tenor pengajuan melebihi kapasitas cicilan ideal Anda. Berikut rekomendasi penyesuaian:</p>
                        </div>
                    </div>
                    <div id="alternative-content" class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-sm text-amber-900 font-medium">
                        <!-- Filled by JS -->
                    </div>
                </div>

                <!-- Institution Recommendation Match -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-lg font-bold text-navy flex items-center gap-2">
                            <i class="fa-solid fa-building-columns text-primaryBlue"></i> Rekomendasi Lembaga Keuangan Sesuai Profil
                        </h3>
                        <button onclick="switchTab('institutions')" class="text-xs font-semibold text-primaryBlue hover:underline">Lihat Semua Bank & Koperasi</button>
                    </div>
                    <div id="matched-institutions-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                        <!-- Filled by JS -->
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= 3. LEMBAGA & PRODUK PINJAMAN (INSTITUTIONS) ================= -->
        <section id="tab-institutions" class="tab-content space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100 space-y-6">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                    <div>
                        <h2 class="text-2xl font-bold text-navy">Database Lembaga Keuangan & Produk Pinjaman</h2>
                        <p class="text-sm text-slate-500 mt-1">Informasi resmi bank konvensional, digital, syariah, dan koperasi di Indonesia.</p>
                    </div>
                    <!-- Search & Filter Controls -->
                    <div class="flex flex-wrap gap-3 w-full md:w-auto">
                        <input type="text" id="institution-search" placeholder="Cari bank atau koperasi..." oninput="filterInstitutions()" class="px-4 py-2 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-primaryBlue w-full sm:w-64">
                        <select id="institution-filter" onchange="filterInstitutions()" class="px-4 py-2 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-primaryBlue bg-white">
                            <option value="all">Semua Kategori</option>
                            <option value="Konvensional">Bank Konvensional</option>
                            <option value="Digital">Bank Digital</option>
                            <option value="Syariah">Bank Syariah</option>
                            <option value="Koperasi">Koperasi Simpan Pinjam</option>
                        </select>
                    </div>
                </div>

                <!-- Institutions Grid -->
                <div id="institutions-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Populated by JS -->
                </div>
            </div>
        </section>

        <!-- ================= 4. LAPORAN DAN HISTORY (HISTORY) ================= -->
        <section id="tab-history" class="tab-content space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100 space-y-6">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 border-b border-slate-100 pb-4">
                    <div>
                        <h2 class="text-2xl font-bold text-navy">Riwayat & Laporan Simulasi</h2>
                        <p class="text-sm text-slate-500">Daftar simulasi keuangan yang pernah Anda lakukan di perangkat ini.</p>
                    </div>
                    <button onclick="clearHistory()" class="px-4 py-2 bg-red-50 hover:bg-red-100 text-red-600 font-semibold rounded-xl text-sm transition flex items-center space-x-2">
                        <i class="fa-solid fa-trash-can"></i><span>Hapus Riwayat</span>
                    </button>
                </div>

                <div id="history-container" class="space-y-4">
                    <!-- Populated by JS -->
                </div>
            </div>
        </section>

        <!-- ================= 5. TUJUAN DAN FUNGSI (ABOUT) ================= -->
        <section id="tab-about" class="tab-content space-y-6">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-100 space-y-6">
                <div>
                    <span class="text-xs font-bold text-primaryBlue uppercase tracking-wider bg-lightBlue px-3 py-1 rounded-full">Informasi Aplikasi</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-navy mt-2">Tentang FinMatch & Panduan Sistem</h2>
                    <p class="text-sm text-slate-600 mt-1">Pelajari latar belakang, tujuan, cara kerja rumus anuitas, serta batasan layanan FinMatch.</p>
                </div>

                <!-- Accordion List -->
                <div class="space-y-4">
                    <!-- Accordion 1 -->
                    <div class="border border-slate-200 rounded-2xl overflow-hidden transition">
                        <button onclick="toggleAccordion('acc-1')" class="w-full px-6 py-4 bg-slate-50 hover:bg-slate-100 flex justify-between items-center font-bold text-navy text-left">
                            <span class="flex items-center gap-3"><i class="fa-solid fa-bullseye text-primaryBlue"></i> 1. Latar Belakang & Tujuan Pembuatan</span>
                            <i class="fa-solid fa-chevron-down transition-transform duration-300" id="icon-acc-1"></i>
                        </button>
                        <div id="content-acc-1" class="hidden px-6 py-4 text-sm text-slate-700 leading-relaxed space-y-2 border-t border-slate-100">
                            <p>Banyak masyarakat mengajukan pinjaman tanpa analisis kapasitas finansial yang memadai, berujung pada gagal bayar atau jeratan utang konsumtif.</p>
                            <p>FinMatch dikembangkan untuk membantu masyarakat mengetahui kemampuan finansial riil sebelum mengajukan pinjaman ke bank atau koperasi, memberikan perhitungan anuitas transparan, serta rekomendasi lembaga yang objektif.</p>
                        </div>
                    </div>

                    <!-- Accordion 2 -->
                    <div class="border border-slate-200 rounded-2xl overflow-hidden transition">
                        <button onclick="toggleAccordion('acc-2')" class="w-full px-6 py-4 bg-slate-50 hover:bg-slate-100 flex justify-between items-center font-bold text-navy text-left">
                            <span class="flex items-center gap-3"><i class="fa-solid fa-calculator text-primaryBlue"></i> 2. Penjelasan Prinsip Anuitas & Batas 40%</span>
                            <i class="fa-solid fa-chevron-down transition-transform duration-300" id="icon-acc-2"></i>
                        </button>
                        <div id="content-acc-2" class="hidden px-6 py-4 text-sm text-slate-700 leading-relaxed space-y-2 border-t border-slate-100">
                            <p><strong>Batas Cicilan 40%:</strong> Kebijakan simulasi FinMatch menetapkan bahwa total cicilan ideal tidak melebihi 40% dari penghasilan bersih bulanan, dikurangi cicilan aktif yang sedang berjalan.</p>
                            <p><strong>Rumus Anuitas:</strong> A = P * [i(1+i)^n] / [(1+i)^n - 1], di mana P adalah pokok pinjaman, i adalah suku bunga bulanan (bunga tahunan dibagi 12), dan n adalah tenor dalam bulan.</p>
                        </div>
                    </div>

                    <!-- Accordion 3 -->
                    <div class="border border-slate-200 rounded-2xl overflow-hidden transition">
                        <button onclick="toggleAccordion('acc-3')" class="w-full px-6 py-4 bg-slate-50 hover:bg-slate-100 flex justify-between items-center font-bold text-navy text-left">
                            <span class="flex items-center gap-3"><i class="fa-solid fa-shield-halved text-primaryBlue"></i> 3. Batasan dan Tanggung Jawab Aplikasi</span>
                            <i class="fa-solid fa-chevron-down transition-transform duration-300" id="icon-acc-3"></i>
                        </button>
                        <div id="content-acc-3" class="hidden px-6 py-4 text-sm text-slate-700 leading-relaxed space-y-2 border-t border-slate-100">
                            <p>FinMatch <strong>hanya memberikan simulasi dan rekomendasi awal</strong>, bukan persetujuan kredit resmi (LOA) atau jaminan bahwa pengajuan pinjaman Anda akan diterima oleh lembaga keuangan terkait.</p>
                            <p>Keputusan akhir kredit sepenuhnya berada di tangan bank atau lembaga pembiayaan yang bersangkutan setelah melalui proses <i>credit scoring</i> dan verifikasi dokumen.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= 6. FINFIN (AI ASSISTANT & LIVE CHAT) ================= -->
        <section id="tab-finfin" class="tab-content space-y-6">
            <div class="bg-white rounded-3xl shadow-sm border border-slate-100 overflow-hidden flex flex-col h-[70vh]">
                <!-- Chat Header -->
                <div class="bg-navy text-white p-4 sm:p-5 flex items-center justify-between">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 rounded-2xl bg-gradient-to-tr from-primaryBlue to-skyBlue flex items-center justify-center text-xl font-bold shadow">
                            <i class="fa-solid fa-robot"></i>
                        </div>
                        <div>
                            <div class="flex items-center space-x-2">
                                <h3 class="font-bold text-base">Finfin (Financial Assistant)</h3>
                                <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-pulse"></span>
                            </div>
                            <span class="text-xs text-skyBlue">AI Chatbot & Live Admin Support</span>
                        </div>
                    </div>
                    <div class="flex space-x-2">
                        <button onclick="switchChatMode('ai')" id="btn-mode-ai" class="px-3 py-1.5 rounded-xl bg-primaryBlue text-white text-xs font-semibold shadow transition">AI Finfin</button>
                        <button onclick="switchChatMode('admin')" id="btn-mode-admin" class="px-3 py-1.5 rounded-xl bg-navy hover:bg-deepBlue text-slate-300 text-xs font-semibold transition border border-deepBlue">Live Admin</button>
                    </div>
                </div>

                <!-- Chat Messages Area -->
                <div id="chat-messages" class="flex-grow p-4 sm:p-6 overflow-y-auto space-y-4 bg-slate-50">
                    <!-- Populated by JS -->
                </div>

                <!-- Quick Prompts (for AI) -->
                <div id="quick-prompts" class="px-4 py-2 bg-white border-t border-slate-100 flex flex-wrap gap-2">
                    <button onclick="sendQuickPrompt('Jelaskan hasil simulasi terakhir saya')" class="text-xs bg-lightBlue text-navy px-3 py-1.5 rounded-lg hover:bg-skyBlue hover:text-white transition font-medium">💡 Jelaskan hasil simulasi saya</button>
                    <button onclick="sendQuickPrompt('Apa perbedaan bank konvensional dan syariah?')" class="text-xs bg-lightBlue text-navy px-3 py-1.5 rounded-lg hover:bg-skyBlue hover:text-white transition font-medium">🏦 Perbedaan Konvensional & Syariah</button>
                    <button onclick="sendQuickPrompt('Apa risiko jika cicilan melewati 40% penghasilan?')" class="text-xs bg-lightBlue text-navy px-3 py-1.5 rounded-lg hover:bg-skyBlue hover:text-white transition font-medium">⚠️ Risiko cicilan tinggi</button>
                </div>

                <!-- Chat Input Area -->
                <div class="p-4 bg-white border-t border-slate-200 flex items-center space-x-3">
                    <input type="text" id="chat-input" placeholder="Ketik pertanyaan keuangan Anda di sini..." onkeypress="if(event.key === 'Enter') sendChatMessage()" class="flex-grow px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                    <button onclick="sendChatMessage()" class="px-5 py-3 bg-primaryBlue hover:bg-skyBlue hover:text-navy text-white font-semibold rounded-xl shadow transition flex items-center justify-center">
                        <i class="fa-solid fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- ================= 7. PROFIL PENGGUNA (PROFILE) ================= -->
        <section id="tab-profile" class="tab-content space-y-6">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-100 space-y-6 max-w-2xl mx-auto">
                <div class="border-b border-slate-100 pb-4">
                    <h2 class="text-2xl font-bold text-navy">Profil Pengguna & Privasi</h2>
                    <p class="text-sm text-slate-500">Kelola informasi akun opsional Anda dan kebijakan perlindungan data.</p>
                </div>

                <div class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 mb-1">Nama Lengkap</label>
                        <input type="text" id="user-name" placeholder="Contoh: Budi Santoso" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 mb-1">Alamat Email</label>
                        <input type="email" id="user-email" placeholder="Contoh: budi@email.com" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-primaryBlue text-sm">
                    </div>
                    <div class="p-4 rounded-2xl bg-lightBlue/50 border border-skyBlue/30 text-xs text-navy space-y-2">
                        <p class="font-bold"><i class="fa-solid fa-shield-halved"></i> Kebijakan Keamanan & Privasi FinMatch:</p>
                        <p>Data finansial Anda disimpan secara lokal di browser perangkat ini dan tidak dikirimkan ke server publik. Kami tidak pernah meminta PIN, OTP, atau password perbankan Anda.</p>
                    </div>
                    <button onclick="saveProfile()" class="w-full py-3 bg-navy hover:bg-primaryBlue text-white font-semibold rounded-xl shadow transition text-sm">
                        Simpan Profil
                    </button>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-navy text-white border-t border-deepBlue py-8 mt-auto">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row items-center justify-between gap-4 text-center md:text-left">
            <div class="flex items-center space-x-3">
                <div class="w-8 h-8 rounded-lg bg-primaryBlue flex items-center justify-center text-white font-bold text-sm">
                    <i class="fa-solid fa-scale-balanced"></i>
                </div>
                <span class="font-bold text-sm">FinMatch &copy; 2026 - Smart Financial & Loan Assistant</span>
            </div>
            <p class="text-xs text-slate-400">
                Aplikasi simulasi dan analisis awal pinjaman. Bukan jaminan persetujuan kredit resmi.
            </p>
        </div>
    </footer>

    <!-- JavaScript Application Logic -->
    <script>
        // --- DATABASE LEMBAGA KEUANGAN & PRODUK ---
        const institutionsDB = [
            {
                id: 'bri',
                name: 'Bank BRI',
                category: 'Konvensional',
                logo: 'fa-building-columns',
                profile: 'Bank Badan Usaha Milik Negara (BUMN) terbesar dengan jangkauan luas hingga pedesaan.',
                productName: 'KUPEDES & KUR BRI',
                interest: '6% - 12% p.a (Efektif)',
                method: 'Anuitas & Flat',
                minLoan: 5000000,
                maxLoan: 500000000,
                tenors: '12 - 60 Bulan',
                requirements: 'KTP, KK, Surat Usaha / SK Pegawai, Jaminan (untuk non-KUR)',
                adminFee: '1% dari plafon',
                website: 'https://www.bri.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'bca',
                name: 'Bank BCA',
                category: 'Konvensional',
                logo: 'fa-building-columns',
                profile: 'Bank swasta terkemuka di Indonesia dengan layanan digital dan kredit konsumer handal.',
                productName: 'KTA BCA Personal Loan & KPR BCA',
                interest: '11% - 15% p.a (Efektif)',
                method: 'Anuitas',
                minLoan: 5000000,
                maxLoan: 100000000,
                tenors: '12 - 36 Bulan',
                requirements: 'KTP, NPWP, Slip Gaji, Rekening Koran 3 Bulan terakhir',
                adminFee: 'Rp 100.000 - 1%',
                website: 'https://www.bca.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'mandiri',
                name: 'Bank Mandiri',
                category: 'Konvensional',
                logo: 'fa-building-columns',
                profile: 'Salah satu bank BUMN terbesar penyedia kredit multiguna dan KPR bersaing.',
                productName: 'Mandiri KSM (Kredit Serbaguna Mandiri)',
                interest: '9% - 13% p.a (Efektif)',
                method: 'Anuitas',
                minLoan: 10000000,
                maxLoan: 1500000000,
                tenors: '12 - 180 Bulan',
                requirements: 'WNI, Pegawai Berpenghasilan Tetap, KTP, SK Karyawan',
                adminFee: '1% dari pinjaman',
                website: 'https://www.bankmandiri.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'bni',
                name: 'Bank BNI',
                category: 'Konvensional',
                logo: 'fa-building-columns',
                profile: 'Bank BUMN dengan produk BNI Fleksi untuk pembiayaan pegawai aktif.',
                productName: 'BNI Fleksi Aktif',
                interest: '9.5% - 14% p.a (Efektif)',
                method: 'Anuitas',
                minLoan: 5000000,
                maxLoan: 500000000,
                tenors: '12 - 180 Bulan',
                requirements: 'Payroll di BNI, KTP, NPWP, Surat Pernyataan',
                adminFee: '1%',
                website: 'https://www.bni.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'jago',
                name: 'Bank Jago',
                category: 'Digital',
                logo: 'fa-mobile-screen-button',
                profile: 'Bank berbasis teknologi digital yang terintegrasi dengan ekosistem digital.',
                productName: 'Jago Pinjaman Digital / Partner Lending',
                interest: '12% - 24% p.a (Efektif)',
                method: 'Anuitas Harian/Bulanan',
                minLoan: 1000000,
                maxLoan: 25000000,
                tenors: '1 - 12 Bulan',
                requirements: 'Akun Bank Jago Aktif, KTP, E-KYC',
                adminFee: 'Sesuai mitra',
                website: 'https://www.jago.com',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'blu',
                name: 'BCA Digital (blu)',
                category: 'Digital',
                logo: 'fa-mobile-screen-button',
                profile: 'Layanan perbankan digital dari BCA dengan produk bluLoan.',
                productName: 'bluLoan',
                interest: '12% - 18% p.a (Efektif)',
                method: 'Anuitas',
                minLoan: 3000000,
                maxLoan: 50000000,
                tenors: '1 - 12 Bulan',
                requirements: 'Aplikasi blu by BCA Digital, KTP, Riwayat Transaksi',
                adminFee: '1%',
                website: 'https://www.blubca.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'seabank',
                name: 'SeaBank Indonesia',
                category: 'Digital',
                logo: 'fa-mobile-screen-button',
                profile: 'Bank digital terintegrasi dengan layanan e-commerce.',
                productName: 'SeaBank Pinjam',
                interest: '15% - 24% p.a (Efektif)',
                method: 'Anuitas',
                minLoan: 1000000,
                maxLoan: 15000000,
                tenors: '3 - 12 Bulan',
                requirements: 'Akun SeaBank, KTP, Verifikasi Wajah',
                adminFee: '1%',
                website: 'https://www.seabank.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'bca-syariah',
                name: 'BCA Syariah',
                category: 'Syariah',
                logo: 'fa-hands-holding-child',
                profile: 'Bank Syariah dengan prinsip bagi hasil dan margin tetap (Murabahah).',
                productName: 'Pembiayaan Multiguna iB',
                interest: '9% - 13% p.a (Margin Setara)',
                method: 'Flat / Margin Tetap',
                minLoan: 10000000,
                maxLoan: 250000000,
                tenors: '12 - 60 Bulan',
                requirements: 'KTP, KK, Slip Gaji, Akad Syariah',
                adminFee: 'Sesuai ketentuan',
                website: 'https://www.bcasyariah.co.id',
                lastUpdated: 'Januari 2026'
            },
            {
                id: 'kpri-sejahtera',
                name: 'KPRI Sejahtera Mandiri',
                category: 'Koperasi',
                logo: 'fa-users',
                profile: 'Koperasi simpan pinjam anggota dengan bunga ringan dan proses kekeluargaan.',
                productName: 'Pinjaman Anggota Koperasi',
                interest: '8% - 10% p.a (Flat / Menurun)',
                method: 'Flat / Menurun',
                minLoan: 1000000,
                maxLoan: 50000000,
                tenors: '6 - 36 Bulan',
                requirements: 'Keanggotaan Koperasi, KTP, Rekomendasi Pengurus',
                adminFee: '0.5%',
                website: 'Informasi langsung kantor koperasi',
                lastUpdated: 'Januari 2026'
            }
        ];

        // --- GLOBAL STATE ---
        let currentSimulationData = null;
        let loanPieChartInstance = null;
        let chatMode = 'ai'; // 'ai' or 'admin'
        let chatHistory = [
            { sender: 'finfin', text: 'Halo! Saya Finfin, asisten keuangan digital FinMatch. Ada yang bisa saya bantu seputar simulasi pinjaman, analisis kemampuan finansial, atau pemilihan bank?' }
        ];
        let userSimulationsHistory = JSON.parse(localStorage.getItem('finmatch_history') || '[]');

        // --- NAVIGATION LOGIC ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('bg-deepBlue', 'text-white'));
            document.querySelectorAll('.nav-btn').forEach(el => el.classList.add('text-slate-200'));

            const targetTab = document.getElementById(`tab-${tabId}`);
            if (targetTab) targetTab.classList.add('active');

            const activeBtn = document.querySelector(`.nav-btn[data-target="${tabId}"]`);
            if (activeBtn) {
                activeBtn.classList.add('bg-deepBlue', 'text-white');
                activeBtn.classList.remove('text-slate-200');
            }

            window.scrollTo({ top: 0, behavior: 'smooth' });
            if (tabId === 'institutions') renderInstitutions(institutionsDB);
            if (tabId === 'history') renderHistory();
            if (tabId === 'finfin') renderChatMessages();
        }

        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            const icon = document.getElementById('mobile-menu-icon');
            menu.classList.toggle('hidden');
            icon.classList.toggle('fa-bars');
            icon.classList.toggle('fa-xmark');
        }

        // --- FORMATTING RUPIAH ---
        function formatRupiahInput(input) {
            let val = input.value.replace(/[^,\d]/g, '');
            let split = val.split(',');
            let sisa = split[0].length % 3;
            let rupiah = split[0].substr(0, sisa);
            let ribuan = split[0].substr(sisa).match(/\d{3}/gi);

            if (ribuan) {
                let separator = sisa ? '.' : '';
                rupiah += separator + ribuan.join('.');
            }
            input.value = split[1] !== undefined ? rupiah + ',' + split[1] : rupiah;
        }

        function parseRupiah(str) {
            if (!str) return 0;
            return parseFloat(str.toString().replace(/\./g, '').replace(/,/g, '.')) || 0;
        }

        function formatIDR(num) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(num);
        }

        function toggleActiveLoanFields() {
            const hasLoan = document.getElementById('has-active-loan').value;
            const container = document.getElementById('active-loan-container');
            if (hasLoan === 'yes') {
                container.classList.remove('hidden');
            } else {
                container.classList.add('hidden');
                document.getElementById('active-loan-amount').value = '0';
            }
        }

        // --- SIMULATION & FINANCIAL MATH LOGIC ---
        function handleSimulation(event) {
            event.preventDefault();

            const netIncome = parseRupiah(document.getElementById('net-income').value);
            const routineExpenses = parseRupiah(document.getElementById('routine-expenses').value);
            const totalAssets = parseRupiah(document.getElementById('total-assets').value);
            const assetType = document.getElementById('asset-type').value;
            const hasActiveLoan = document.getElementById('has-active-loan').value;
            const activeLoanAmount = hasActiveLoan === 'yes' ? parseRupiah(document.getElementById('active-loan-amount').value) : 0;

            const loanAmount = parseRupiah(document.getElementById('loan-amount').value);
            const loanTenor = parseInt(document.getElementById('loan-tenor').value);
            const annualInterest = parseFloat(document.getElementById('annual-interest').value);
            const loanType = document.getElementById('loan-type').value;

            // Validation
            if (netIncome <= 0) {
                showNotification('Penghasilan bersih harus lebih dari 0.');
                return;
            }
            if (loanAmount <= 0 || loanTenor <= 0) {
                showNotification('Nominal pinjaman dan tenor harus valid.');
                return;
            }

            // 1. Max Installment Limit (40% of net income minus active loans)
            const maxInstallmentLimitAll = 0.40 * netIncome;
            const maxNewInstallmentAllowed = maxInstallmentLimitAll - activeLoanAmount;

            // 2. Annuity Calculation
            // A = P * [i(1+i)^n] / [(1+i)^n - 1]
            const monthlyInterestRate = (annualInterest / 100) / 12;
            let monthlyPayment = 0;
            let totalPayment = 0;
            let totalInterest = 0;

            if (monthlyInterestRate === 0) {
                monthlyPayment = loanAmount / loanTenor;
                totalPayment = loanAmount;
                totalInterest = 0;
            } else {
                const i = monthlyInterestRate;
                const n = loanTenor;
                monthlyPayment = loanAmount * (i * Math.pow(1 + i, n)) / (Math.pow(1 + i, n) - 1);
                totalPayment = monthlyPayment * n;
                totalInterest = totalPayment - loanAmount;
            }

            // 3. Remaining Income Analysis
            const remainingIncome = netIncome - routineExpenses - activeLoanAmount - monthlyPayment;
            const installmentRatio = (monthlyPayment / netIncome) * 100;

            // 4. Financial Health Status Assessment
            let status = 'Aman';
            let statusColor = 'bg-emerald-50 text-emerald-700 border-emerald-200';
            let iconClass = 'fa-solid fa-check text-emerald-600 bg-emerald-100';
            let statusTitle = 'Aman (Layak)';
            let statusDesc = 'Cicilan berada dalam batas kemampuan 40% dan sisa pendapatan bulanan Anda masih sangat memadai.';

            if (monthlyPayment > maxNewInstallmentAllowed || remainingIncome < 0) {
                status = 'Tidak Disarankan';
                statusColor = 'bg-rose-50 text-rose-700 border-rose-200';
                iconClass = 'fa-solid fa-triangle-exclamation text-rose-600 bg-rose-100';
                statusTitle = 'Tidak Disarankan (Melebihi Kapasitas)';
                statusDesc = 'Estimasi cicilan melebihi batas 40% penghasilan bersih atau membuat sisa pendapatan negatif. Risiko gagal bayar tinggi.';
            } else if (monthlyPayment > (0.8 * maxNewInstallmentAllowed) || remainingIncome < (0.2 * netIncome)) {
                status = 'Perlu Dipertimbangkan';
                statusColor = 'bg-amber-50 text-amber-700 border-amber-200';
                iconClass = 'fa-solid fa-circle-exclamation text-amber-600 bg-amber-100';
                statusTitle = 'Perlu Dipertimbangkan (Batas Kritis)';
                statusDesc = 'Cicilan mendekati batas maksimal kemampuan Anda dan sisa pendapatan relatif terbatas. Pertimbangkan pinjaman lebih kecil.';
            }

            currentSimulationData = {
                date: new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'long', year: 'numeric' }),
                netIncome, routineExpenses, totalAssets, assetType, activeLoanAmount,
                loanAmount, loanTenor, annualInterest, loanType,
                monthlyPayment, totalInterest, totalPayment, remainingIncome,
                installmentRatio, status, statusTitle
            };

            // Save to Local History
            userSimulationsHistory.unshift(currentSimulationData);
            if (userSimulationsHistory.length > 20) userSimulationsHistory.pop();
            localStorage.setItem('finmatch_history', JSON.stringify(userSimulationsHistory));

            // Render Results UI
            document.getElementById('simulation-results').classList.remove('hidden');
            document.getElementById('res-max-installment').innerText = formatIDR(maxNewInstallmentAllowed > 0 ? maxNewInstallmentAllowed : 0);
            document.getElementById('res-active-note').innerText = `Cicilan aktif: ${formatIDR(activeLoanAmount)}`;
            document.getElementById('res-monthly-payment').innerText = formatIDR(monthlyPayment);
            document.getElementById('res-total-interest').innerText = formatIDR(totalInterest);
            document.getElementById('res-remaining-income').innerText = formatIDR(remainingIncome);
            document.getElementById('res-total-payment').innerText = formatIDR(totalPayment);
            document.getElementById('res-installment-ratio').innerText = installmentRatio.toFixed(1) + '%';
            document.getElementById('ratio-percent-text').innerText = installmentRatio.toFixed(1) + '%';
            
            const progressBar = document.getElementById('ratio-progress-bar');
            progressBar.style.width = Math.min(installmentRatio, 100) + '%';
            progressBar.className = `h-3 rounded-full transition-all duration-500 ${installmentRatio > 40 ? 'bg-rose-500' : installmentRatio > 30 ? 'bg-amber-500' : 'bg-emerald-500'}`;

            // Banner status
            const banner = document.getElementById('health-status-banner');
            banner.className = `p-6 rounded-3xl shadow-md border flex flex-col md:flex-row items-center justify-between gap-4 ${statusColor}`;
            document.getElementById('health-title').innerText = statusTitle;
            document.getElementById('health-desc').innerText = statusDesc;
            const iconDiv = document.getElementById('health-icon');
            iconDiv.className = `w-14 h-14 rounded-2xl flex items-center justify-center text-2xl font-bold shadow ${iconClass}`;

            // Alternative section if capacity exceeded
            const altSection = document.getElementById('alternative-section');
            const altContent = document.getElementById('alternative-content');
            if (status === 'Tidak Disarankan' || status === 'Perlu Dipertimbangkan') {
                altSection.classList.remove('hidden');
                const suggestedLoan = maxNewInstallmentAllowed * ((Math.pow(1 + monthlyInterestRate, loanTenor) - 1) / (monthlyInterestRate * Math.pow(1 + monthlyInterestRate, loanTenor)));
                const suggestedTenor = loanTenor + 12;
                altContent.innerHTML = `
                    <div class="p-3 bg-white/60 rounded-xl">
                        <span class="block text-xs text-amber-700">Rekomendasi Plafon Aman</span>
                        <span class="font-bold text-amber-900">${formatIDR(suggestedLoan > 0 ? suggestedLoan : loanAmount * 0.5)}</span>
                    </div>
                    <div class="p-3 bg-white/60 rounded-xl">
                        <span class="block text-xs text-amber-700">Rekomendasi Tenor Alternatif</span>
                        <span class="font-bold text-amber-900">${suggestedTenor} Bulan (untuk meringankan cicilan)</span>
                    </div>
                `;
            } else {
                altSection.classList.add('hidden');
            }

            // Render Chart
            renderLoanPieChart(loanAmount, totalInterest);

            // Render Matched Institutions
            renderMatchedInstitutions(loanAmount, loanTenor);

            // Scroll to results
            document.getElementById('simulation-results').scrollIntoView({ behavior: 'smooth' });
        }

        function renderLoanPieChart(principal, interest) {
            const ctx = document.getElementById('loanPieChart').getContext('2d');
            if (loanPieChartInstance) loanPieChartInstance.destroy();

            loanPieChartInstance = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Pokok Pinjaman', 'Total Bunga'],
                    datasets: [{
                        data: [principal, interest],
                        backgroundColor: ['#00509e', '#66a3ff'],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 11 } } }
                    }
                }
            });
        }

        function renderMatchedInstitutions(loanAmount, loanTenor) {
            const container = document.getElementById('matched-institutions-list');
            const filtered = institutionsDB.filter(inst => loanAmount >= inst.minLoan && loanAmount <= inst.maxLoan).slice(0, 3);

            if (filtered.length === 0) {
                container.innerHTML = `<p class="text-sm text-slate-500 col-span-full">Tidak ada produk spesifik yang persis dengan nominal ini. Silakan cek daftar lengkap lembaga.</p>`;
                return;
            }

            container.innerHTML = filtered.map(inst => `
                <div class="bg-slate-50 border border-slate-200 p-4 rounded-2xl flex flex-col justify-between space-y-3">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 rounded-xl bg-lightBlue text-navy flex items-center justify-center font-bold">
                            <i class="fa-solid ${inst.logo}"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-navy text-sm">${inst.name}</h4>
                            <span class="text-xs text-primaryBlue font-medium">${inst.productName}</span>
                        </div>
                    </div>
                    <div class="text-xs space-y-1 text-slate-600">
                        <p><strong>Bunga:</strong> ${inst.interest}</p>
                        <p><strong>Tenor:</strong> ${inst.tenors}</p>
                    </div>
                    <a href="${inst.website}" target="_blank" class="w-full py-2 bg-navy hover:bg-primaryBlue text-white text-xs font-semibold rounded-xl text-center transition block">
                        Kunjungi Website Resmi <i class="fa-solid fa-arrow-up-right-from-square ml-1"></i>
                    </a>
                </div>
            `).join('');
        }

        // --- INSTITUTIONS & FILTER ---
        function renderInstitutions(list) {
            const grid = document.getElementById('institutions-grid');
            grid.innerHTML = list.map(inst => `
                <div class="bg-white border border-slate-200 p-6 rounded-3xl shadow-sm hover:shadow-md transition space-y-4 flex flex-col justify-between">
                    <div class="space-y-3">
                        <div class="flex items-center justify-between">
                            <div class="w-12 h-12 rounded-2xl bg-lightBlue text-navy flex items-center justify-center text-xl font-bold">
                                <i class="fa-solid ${inst.logo}"></i>
                            </div>
                            <span class="text-xs font-semibold px-3 py-1 rounded-full bg-slate-100 text-slate-700">${inst.category}</span>
                        </div>
                        <div>
                            <h3 class="text-lg font-bold text-navy">${inst.name}</h3>
                            <span class="text-xs font-semibold text-primaryBlue">${inst.productName}</span>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">${inst.profile}</p>
                        <div class="p-3 rounded-xl bg-slate-50 text-xs space-y-1 text-slate-700">
                            <p><strong>Suku Bunga:</strong> ${inst.interest}</p>
                            <p><strong>Metode:</strong> ${inst.method}</p>
                            <p><strong>Tenor:</strong> ${inst.tenors}</p>
                            <p><strong>Plafon:</strong> ${formatIDR(inst.minLoan)} - ${formatIDR(inst.maxLoan)}</p>
                            <p><strong>Syarat Utama:</strong> ${inst.requirements}</p>
                        </div>
                    </div>
                    <div class="pt-2 border-t border-slate-100 flex items-center justify-between">
                        <span class="text-[10px] text-slate-400">Update: ${inst.lastUpdated}</span>
                        <a href="${inst.website}" target="_blank" class="px-4 py-2 bg-navy hover:bg-primaryBlue text-white text-xs font-semibold rounded-xl transition flex items-center space-x-1">
                            <span>Situs Resmi</span> <i class="fa-solid fa-arrow-up-right-from-square text-[10px]"></i>
                        </a>
                    </div>
                </div>
            `).join('');
        }

        function filterInstitutions() {
            const query = document.getElementById('institution-search').value.toLowerCase();
            const category = document.getElementById('institution-filter').value;

            const filtered = institutionsDB.filter(inst => {
                const matchQuery = inst.name.toLowerCase().includes(query) || inst.productName.toLowerCase().includes(query);
                const matchCat = category === 'all' || inst.category === category;
                return matchQuery && matchCat;
            });

            renderInstitutions(filtered);
        }

        // --- HISTORY MANAGEMENT ---
        function renderHistory() {
            const container = document.getElementById('history-container');
            if (userSimulationsHistory.length === 0) {
                container.innerHTML = `<div class="text-center py-12 text-slate-400"><i class="fa-solid fa-folder-open text-4xl mb-3"></i><p>Belum ada riwayat simulasi tersimpan.</p></div>`;
                return;
            }

            container.innerHTML = userSimulationsHistory.map((item, idx) => `
                <div class="bg-slate-50 border border-slate-200 p-5 rounded-2xl flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                    <div class="space-y-1">
                        <div class="flex items-center space-x-2">
                            <span class="text-xs font-bold text-slate-400">${item.date}</span>
                            <span class="text-xs px-2.5 py-0.5 rounded-full font-semibold ${item.status === 'Aman' ? 'bg-emerald-100 text-emerald-700' : item.status === 'Perlu Dipertimbangkan' ? 'bg-amber-100 text-amber-700' : 'bg-rose-100 text-rose-700'}">${item.status}</span>
                        </div>
                        <h4 class="font-bold text-navy text-base">Pinjaman ${item.loanType}: ${formatIDR(item.loanAmount)} (${item.loanTenor} Bulan)</h4>
                        <p class="text-xs text-slate-600">Penghasilan: ${formatIDR(item.netIncome)} | Cicilan/Bulan: <strong class="text-primaryBlue">${formatIDR(item.monthlyPayment)}</strong></p>
                    </div>
                    <div class="flex items-center space-x-2 w-full md:w-auto justify-end">
                        <button onclick="loadHistoryItem(${idx})" class="px-4 py-2 bg-primaryBlue hover:bg-skyBlue hover:text-navy text-white text-xs font-semibold rounded-xl transition">
                            Lihat Simulasi
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function loadHistoryItem(idx) {
            const item = userSimulationsHistory[idx];
            if (!item) return;
            switchTab('simulation');
            document.getElementById('net-income').value = item.netIncome.toLocaleString('id-ID');
            document.getElementById('routine-expenses').value = item.routineExpenses.toLocaleString('id-ID');
            document.getElementById('total-assets').value = item.totalAssets ? item.totalAssets.toLocaleString('id-ID') : '';
            document.getElementById('asset-type').value = item.assetType || 'Tabungan';
            document.getElementById('loan-amount').value = item.loanAmount.toLocaleString('id-ID');
            document.getElementById('loan-tenor').value = item.loanTenor;
            document.getElementById('annual-interest').value = item.annualInterest;
            document.getElementById('loan-type').value = item.loanType;
            // Trigger calculation
            document.getElementById('simulation-form').dispatchEvent(new Event('submit'));
        }

        function clearHistory() {
            userSimulationsHistory = [];
            localStorage.removeItem('finmatch_history');
            renderHistory();
            showNotification('Riwayat berhasil dibersihkan.');
        }

        // --- PDF EXPORT (jsPDF) ---
        function exportToPDF() {
            if (!currentSimulationData) {
                showNotification('Lakukan simulasi terlebih dahulu sebelum mengunduh laporan.');
                return;
            }

            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            doc.setFillColor(0, 51, 102);
            doc.rect(0, 0, 210, 30, 'F');
            doc.setTextColor(255, 255, 255);
            doc.setFontSize(18);
            doc.text('FinMatch - Laporan Simulasi & Analisis Pinjaman', 14, 18);

            doc.setTextColor(50, 50, 50);
            doc.setFontSize(12);
            let y = 45;
            doc.text(`Tanggal Simulasi: ${currentSimulationData.date}`, 14, y);
            y += 10;
            doc.text(`Status Kemampuan Finansial: ${currentSimulationData.statusTitle}`, 14, y);
            y += 15;

            doc.setFontSize(14);
            doc.setTextColor(0, 51, 102);
            doc.text('Profil Finansial Pengguna:', 14, y);
            y += 8;
            doc.setFontSize(11);
            doc.setTextColor(50, 50, 50);
            doc.text(`- Penghasilan Bersih: ${formatIDR(currentSimulationData.netIncome)}`, 18, y); y += 7;
            doc.text(`- Pengeluaran Rutin: ${formatIDR(currentSimulationData.routineExpenses)}`, 18, y); y += 7;
            doc.text(`- Cicilan Aktif Lainnya: ${formatIDR(currentSimulationData.activeLoanAmount)}`, 18, y); y += 12;

            doc.setFontSize(14);
            doc.setTextColor(0, 51, 102);
            doc.text('Detail Kebutuhan & Hasil Simulasi Anuitas:', 14, y);
            y += 8;
            doc.setFontSize(11);
            doc.setTextColor(50, 50, 50);
            doc.text(`- Jenis Pinjaman: ${currentSimulationData.loanType}`, 18, y); y += 7;
            doc.text(`- Plafon Pinjaman: ${formatIDR(currentSimulationData.loanAmount)}`, 18, y); y += 7;
            doc.text(`- Tenor: ${currentSimulationData.loanTenor} Bulan (${(currentSimulationData.loanTenor/12).toFixed(1)} Tahun)`, 18, y); y += 7;
            doc.text(`- Suku Bunga: ${currentSimulationData.annualInterest}% p.a`, 18, y); y += 7;
            doc.text(`- Estimasi Cicilan Per Bulan: ${formatIDR(currentSimulationData.monthlyPayment)}`, 18, y); y += 7;
            doc.text(`- Total Bunga Pinjaman: ${formatIDR(currentSimulationData.totalInterest)}`, 18, y); y += 7;
            doc.text(`- Total Pembayaran Keseluruhan: ${formatIDR(currentSimulationData.totalPayment)}`, 18, y); y += 7;
            doc.text(`- Sisa Pendapatan Bersih: ${formatIDR(currentSimulationData.remainingIncome)}`, 18, y); y += 15;

            doc.setFontSize(9);
            doc.setTextColor(100, 100, 100);
            doc.text('Catatan: Laporan ini dihasilkan secara otomatis oleh FinMatch (Smart Financial & Loan Assistant).', 14, y);
            y += 6;
            doc.text('Bukan merupakan persetujuan kredit resmi dari lembaga keuangan.', 14, y);

            doc.save(`FinMatch_Laporan_Simulasi_${new Date().toISOString().slice(0,10)}.pdf`);
            showNotification('Laporan PDF berhasil diunduh.');
        }

        // --- ACCORDION TOGGLE ---
        function toggleAccordion(id) {
            const content = document.getElementById(`content-${id}`);
            const icon = document.getElementById(`icon-${id}`);
            content.classList.toggle('hidden');
            icon.classList.toggle('rotate-180');
        }

        // --- FINFIN AI & ADMIN CHATBOT ---
        function switchChatMode(mode) {
            chatMode = mode;
            const btnAI = document.getElementById('btn-mode-ai');
            const btnAdmin = document.getElementById('btn-mode-admin');
            const quickPrompts = document.getElementById('quick-prompts');

            if (mode === 'ai') {
                btnAI.className = 'px-3 py-1.5 rounded-xl bg-primaryBlue text-white text-xs font-semibold shadow transition';
                btnAdmin.className = 'px-3 py-1.5 rounded-xl bg-navy hover:bg-deepBlue text-slate-300 text-xs font-semibold transition border border-deepBlue';
                quickPrompts.classList.remove('hidden');
                chatHistory = [{ sender: 'finfin', text: 'Halo! Saya Finfin AI. Ada yang ingin didiskusikan tentang perhitungan anuitas atau hasil simulasi Anda?' }];
            } else {
                btnAdmin.className = 'px-3 py-1.5 rounded-xl bg-primaryBlue text-white text-xs font-semibold shadow transition';
                btnAI.className = 'px-3 py-1.5 rounded-xl bg-navy hover:bg-deepBlue text-slate-300 text-xs font-semibold transition border border-deepBlue';
                quickPrompts.classList.add('hidden');
                chatHistory = [{ sender: 'admin', text: 'Halo, saya Admin FinMatch. Saat ini admin sedang online (Estimasi respons: < 5 menit). Ada pertanyaan seputar produk bank atau kendala simulasi?' }];
            }
            renderChatMessages();
        }

        function renderChatMessages() {
            const container = document.getElementById('chat-messages');
            container.innerHTML = chatHistory.map(msg => {
                const isUser = msg.sender === 'user';
                return `
                    <div class="flex ${isUser ? 'justify-end' : 'justify-start'}">
                        <div class="max-w-[80%] sm:max-w-[70%] p-4 rounded-2xl text-sm ${isUser ? 'bg-primaryBlue text-white rounded-br-none' : 'bg-white text-slate-800 shadow-sm border border-slate-100 rounded-bl-none'}">
                            <p>${msg.text}</p>
                            <span class="block text-[10px] mt-1 text-right ${isUser ? 'text-skyBlue' : 'text-slate-400'}">${new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</span>
                        </div>
                    </div>
                `;
            }).join('');
            container.scrollTop = container.scrollHeight;
        }

        function sendQuickPrompt(text) {
            document.getElementById('chat-input').value = text;
            sendChatMessage();
        }

        function sendChatMessage() {
            const input = document.getElementById('chat-input');
            const text = input.value.trim();
            if (!text) return;

            chatHistory.push({ sender: 'user', text });
            input.value = '';
            renderChatMessages();

            // Simulate intelligent response
            setTimeout(() => {
                let reply = '';
                const lower = text.toLowerCase();
                if (chatMode === 'ai') {
                    if (lower.includes('simulasi') || lower.includes('hasil')) {
                        if (currentSimulationData) {
                            reply = `Berdasarkan simulasi terakhir Anda untuk pinjaman ${formatIDR(currentSimulationData.loanAmount)} (${currentSimulationData.loanTenor} bulan), status finansial Anda adalah "${currentSimulationData.statusTitle}" dengan cicilan bulanan ${formatIDR(currentSimulationData.monthlyPayment)}.`;
                        } else {
                            reply = `Anda belum melakukan simulasi. Silakan isi kalkulator pada menu "Simulasi Anuitas" terlebih dahulu agar saya dapat memberikan analisis kontekstual!`;
                        }
                    } else if (lower.includes('konvensional') || lower.includes('syariah')) {
                        reply = `Bank Konvensional menggunakan suku bunga pasar dengan sistem anuitas atau flat. Sementara Bank Syariah menggunakan prinsip bagi hasil atau margin tetap (seperti akad Murabahah) tanpa riba.`;
                    } else if (lower.includes('risiko') || lower.includes('40%')) {
                        reply = `Risiko utama jika cicilan melewati 40% penghasilan adalah terganggunya arus kas bulanan untuk kebutuhan pokok dan dana darurat, yang berpotensi memicu kredit macet.`;
                    } else {
                        reply = `Pertanyaan yang bagus! Prinsip utama dalam mengajukan pinjaman adalah memastikan cicilan baru tidak mengganggu pengeluaran rutin dan sisa pendapatan Anda tetap aman.`;
                    }
                } else {
                    reply = `Terima kasih pesan Anda telah diterima oleh Admin FinMatch. Staf kami akan segera merespons secara langsung melalui sistem live chat.`;
                }

                chatHistory.push({ sender: chatMode === 'ai' ? 'finfin' : 'admin', text: reply });
                renderChatMessages();
            }, 800);
        }

        // --- PROFILE SAVING ---
        function saveProfile() {
            const name = document.getElementById('user-name').value;
            const email = document.getElementById('user-email').value;
            localStorage.setItem('finmatch_user', JSON.stringify({ name, email }));
            showNotification('Profil berhasil disimpan secara lokal.');
        }

        // --- NOTIFICATION MODAL TOAST ---
        function showNotification(message) {
            const toast = document.createElement('div');
            toast.className = 'fixed bottom-5 right-5 z-50 bg-navy text-white px-6 py-3 rounded-2xl shadow-xl text-sm flex items-center space-x-3 transition transform translate-y-0 opacity-100';
            toast.innerHTML = `<i class="fa-solid fa-circle-info text-skyBlue"></i><span>${message}</span>`;
            document.body.appendChild(toast);
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // Load profile on start if exists
        window.onload = function() {
            const savedUser = JSON.parse(localStorage.getItem('finmatch_user') || '{}');
            if (savedUser.name) document.getElementById('user-name').value = savedUser.name;
            if (savedUser.email) document.getElementById('user-email').value = savedUser.email;
        }
    </script>
</body>
</html>
