<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Model T Saluran?", "id": "Representasi Kapasitansi Di Tengah Rangkaian." },
  { "en": "Apa Itu Konstanta Propagasi?", "id": "Ukuran Pelemahan Dan Pergeseran Fasa." },
  { "en": "Apa Itu Attenuation Constant?", "id": "Bagian Riil Dari Konstanta Propagasi." },
  { "en": "Apa Itu Phase Constant?", "id": "Bagian Imajiner Dari Konstanta Propagasi." },
  { "en": "Apa Itu Insulator String Efficiency?", "id": "Distribusi Tegangan Pada Rangkaian Isolator." },
  { "en": "Apa Itu Grading Ring Isolator?", "id": "Meratakan Distribusi Tegangan Isolator Gantung." },
  { "en": "Apa Itu Arcing Horn Isolator?", "id": "Melindungi Isolator Dari Busur Api." },
  { "en": "Apa Itu Sag Pada Transmisi?", "id": "Lengkungan Kabel Di Antara Tiang." },
  { "en": "Apa Itu Span Length?", "id": "Jarak Antara Dua Tiang Transmisi." },
  { "en": "Apa Itu Ground Clearance?", "id": "Jarak Terendah Kabel Ke Tanah." },
  { "en": "Apa Itu Galloping Conductor?", "id": "Ayunan Vertikal Kabel Akibat Angin." },
  { "en": "Apa Itu Armor Rod?", "id": "Pelindung Kabel Di Titik Jepit." },
  { "en": "Apa Itu Tower Lattice?", "id": "Tiang Listrik Rangka Baja Terbuka." },
  { "en": "Apa Itu Tower Monopole?", "id": "Tiang Listrik Tunggal Bentuk Pipa." },
  { "en": "Apa Itu Crossarm Tower?", "id": "Lengan Tiang Penyangga Kabel Fasa." },
  { "en": "Apa Itu Ground Wire Tower?", "id": "Kabel Pelindung Petir Paling Atas." },
  { "en": "Apa Itu OPGW Cable?", "id": "Kabel Ground Berisi Serat Optik." },
  { "en": "Apa Itu Footing Resistance?", "id": "Tahanan Pentanahan Kaki Tiang Tower." },
  { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Tanah Ditanam Paralel Jalur." },
  { "en": "Apa Itu Right of Way? (ROW)", "id": "Jalur Tanah Bebas Bawah Transmisi." },
  { "en": "Apa Itu SUTT?", "id": "Saluran Udara Tegangan Tinggi." },
  { "en": "Apa Itu SUTET?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Apa Itu SKTT?", "id": "Saluran Kabel Tegangan Tinggi Tanah." },
  { "en": "Apa Itu GIS Substation?", "id": "Gardu Induk Berisolasi Gas SF6." },
  { "en": "Apa Itu AIS Substation?", "id": "Gardu Induk Berisolasi Udara Terbuka." },
  { "en": "Apa Itu Busbar Configuration?", "id": "Susunan Rel Daya Gardu Induk." },
  { "en": "Apa Itu Single Busbar?", "id": "Satu Rel Untuk Semua Sirkuit." },
  { "en": "Apa Itu Double Busbar?", "id": "Dua Rel Utama Fleksibilitas Tinggi." },
  { "en": "Apa Itu Ring Busbar?", "id": "Rel Melingkar Hemat Pemutus Tenaga." },
  { "en": "Apa Itu Breaker and Half?", "id": "Tiga Breaker Untuk Dua Sirkuit." },
  { "en": "Apa Itu Transfer Bus?", "id": "Rel Cadangan Saat Perbaikan Breaker." },
  { "en": "Apa Itu Bus Coupler?", "id": "Penghubung Antara Dua Rel Utama." },
  { "en": "Apa Itu Bus Section?", "id": "Pemisah Bagian Pada Satu Rel." },
  { "en": "Apa Itu Circuit Breaker?", "id": "Saklar Pemutus Arus Beban Gangguan." },
  { "en": "Apa Itu Disconnector Switch?", "id": "Saklar Pemisah Rangkaian Tanpa Beban." },
  { "en": "Apa Itu Earthing Switch?", "id": "Saklar Pembumian Untuk Keamanan Kerja." },
  { "en": "Apa Itu Lightning Arrester?", "id": "Pelindung Peralatan Dari Surja Petir." },
  { "en": "Apa Itu Current Transformer? (CT)", "id": "Trafo Penurun Arus Untuk Metering." },
  { "en": "Apa Itu Voltage Transformer? (VT)", "id": "Trafo Penurun Tegangan Untuk Metering." },
  { "en": "Apa Itu Capacitive Voltage Transformer?", "id": "VT Menggunakan Pembagi Tegangan Kapasitor." },
  { "en": "Apa Itu Wave Trap?", "id": "Perangkap Sinyal Komunikasi Frekuensi Tinggi." },
  { "en": "Apa Itu Marshalling Kiosk?", "id": "Kotak Terminal Kabel Kontrol Lapangan." },
  { "en": "Apa Itu Battery Bank?", "id": "Sumber Daya DC Cadangan Gardu." },
  { "en": "Apa Itu Battery Charger?", "id": "Alat Pengisi Daya Baterai DC." },
  { "en": "Apa Itu DC Distribution Panel?", "id": "Panel Pembagi Listrik Arus Searah." },
  { "en": "Apa Itu AC Distribution Panel?", "id": "Panel Pembagi Listrik Arus Bolak-Balik." },
  { "en": "Apa Itu Control Room?", "id": "Ruang Pusat Pengendalian Gardu Induk." },
  { "en": "Apa Itu Inter-Control Center Protocol? (ICCP)", "id": "Komunikasi Data Antar Pusat Kontrol." },
  { "en": "Apa Itu State Estimator SCADA?", "id": "Algoritma Perkiraan Kondisi Sistem Tenaga." },
  { "en": "Apa Itu Contingency Analysis?", "id": "Simulasi Dampak Kegagalan Komponen Sistem." },
  { "en": "Apa Itu Network Topology Processing?", "id": "Menentukan Status Sambungan Jaringan Listrik." },
  { "en": "Apa Itu Generation Dispatch Control?", "id": "Mengatur Output Generator Sesuai Jadwal." },
  { "en": "Apa Itu Automatic Volt-Var Control?", "id": "Kontrol Otomatis Tegangan Dan Reaktif." },
  { "en": "Apa Itu Load Forecasting?", "id": "Prediksi Beban Listrik Masa Depan." },
  { "en": "Apa Itu Operator Training Simulator?", "id": "Simulasi Pelatihan Operator Pusat Kontrol." },
  { "en": "Apa Itu Tagging And Interlocking?", "id": "Prosedur Keselamatan Operasi Peralatan Switching." },
  { "en": "Apa Itu Historical Information System?", "id": "Database Penyimpanan Data Operasi Jangka Panjang." },
  { "en": "Apa Itu Wattmetric Earth Fault?", "id": "Proteksi Tanah Sistem Netral Terisolasi." },
  { "en": "Apa Itu Sensitive Earth Fault? (SEF)", "id": "Proteksi Gangguan Tanah Arus Kecil." },
  { "en": "Apa Itu Neutral Admittance Protection?", "id": "Proteksi Tanah Berbasis Admitansi Netral." },
  { "en": "Apa Itu Transient Earth Fault?", "id": "Gangguan Tanah Sesaat Cepat Hilang." },
  { "en": "Apa Itu Cross-Differential Protection?", "id": "Proteksi Diferensial Dua Saluran Paralel." },
  { "en": "Apa Itu Teed Circuit Protection?", "id": "Proteksi Saluran Bercabang Tiga Terminal." },
  { "en": "Apa Itu Weak Infeed Logic?", "id": "Logika Trip Saat Sumber Lemah." },
  { "en": "Apa Itu Echo Logic?", "id": "Memantulkan Sinyal Trip Ke Pengirim." },
  { "en": "Apa Itu Current Reversal Guard?", "id": "Mencegah Trip Salah Saat Arus Balik." },
  { "en": "Apa Itu Switch-Onto-Fault Logic?", "id": "Trip Instan Menutup Ke Gangguan." },
  { "en": "Apa Itu Open Conductor Detection?", "id": "Mendeteksi Kabel Putus Tanpa Hubung Tanah." },
  { "en": "Apa Itu Broken Bar Detection?", "id": "Mendeteksi Batang Rotor Motor Putus." },
  { "en": "Apa Itu Motor Start Protection?", "id": "Membatasi Jumlah Start Motor Per Jam." },
  { "en": "Apa Itu Locked Rotor Protection?", "id": "Proteksi Motor Macet Saat Start." },
  { "en": "Apa Itu Jammed Rotor Protection?", "id": "Proteksi Motor Macet Saat Operasi." },
  { "en": "Apa Itu Underpower Protection?", "id": "Proteksi Beban Hilang (Pompa Kosong)." },
  { "en": "Apa Itu Phase Reversal Protection?", "id": "Mencegah Motor Putar Balik Arah." },
  { "en": "Apa Itu Incomplete Sequence Protection?", "id": "Gagal Start Motor Dalam Waktu Tertentu." },
  { "en": "Apa Itu Restart Inhibition?", "id": "Mencegah Start Ulang Motor Panas." },
  { "en": "Apa Itu Speed Switch Motor?", "id": "Saklar Deteksi Kecepatan Nol Motor." },
  { "en": "Apa Itu Vibration Monitoring System?", "id": "Sistem Pantau Getaran Mesin Berputar." },
  { "en": "Apa Itu Partial Discharge Monitoring?", "id": "Sistem Pantau Peluahan Listrik Isolasi." },
  { "en": "Apa Itu Online DGA Monitor?", "id": "Analisis Gas Trafo Secara Real-Time." },
  { "en": "Apa Itu Bushing Monitoring System?", "id": "Pantau Tan Delta Bushing Trafo." },
  { "en": "Apa Itu OLTC Monitoring?", "id": "Pantau Kondisi Tap Changer Trafo." },
  { "en": "Apa Itu Circuit Breaker Monitor?", "id": "Pantau Gas, Pegas, Dan Kontak Breaker." },
  { "en": "Apa Itu Battery Monitoring System?", "id": "Pantau Tegangan, Suhu, Impedansi Baterai." },
  { "en": "Apa Itu Cable Temperature Monitoring?", "id": "Pantau Suhu Kabel Serat Optik." },
  { "en": "Apa Itu Sag Monitoring System?", "id": "Pantau Lendutan Kabel Saluran Transmisi." },
  { "en": "Apa Itu Ice Load Monitoring?", "id": "Pantau Beban Es Pada Kabel." },
  { "en": "Apa Itu Dynamic Line Rating?", "id": "Kapasitas Kabel Berubah Sesuai Cuaca." },
  { "en": "Apa Itu Wide Area Protection?", "id": "Sistem Proteksi Terintegrasi Area Luas." },
  { "en": "Apa Itu System Integrity Protection?", "id": "Skema Proteksi Integritas Sistem Tenaga." },
  { "en": "Apa Itu Remedial Action Scheme?", "id": "Tindakan Otomatis Cegah Runtuh Sistem." },
  { "en": "Apa Itu Under-Voltage Load Shedding?", "id": "Lepas Beban Saat Tegangan Runtuh." },
  { "en": "Apa Itu Under-Frequency Load Shedding?", "id": "Lepas Beban Saat Frekuensi Turun." },
  { "en": "Apa Itu Special Protection Scheme?", "id": "Skema Proteksi Khusus Kondisi Tertentu." },
  { "en": "Apa Itu Generator Rejection Scheme?", "id": "Lepas Pembangkit Saat Beban Hilang." },
  { "en": "Apa Itu Load Rejection Test?", "id": "Uji Lepas Beban Pembangkit Tiba-Tiba." },
  { "en": "Apa Itu Capability Curve Generator?", "id": "Grafik Batas Operasi Aman Generator." },
  { "en": "Apa Itu P-Q Diagram?", "id": "Diagram Daya Aktif Dan Reaktif." },
  { "en": "Apa Itu Armature Reaction?", "id": "Efek Medan Stator Terhadap Rotor." },
  { "en": "Apa Itu Direct Axis Reactance? (Xd)", "id": "Reaktansi Sinkron Sumbu Langsung." },
  { "en": "Apa Itu Quadrature Axis Reactance? (Xq)", "id": "Reaktansi Sinkron Sumbu Kuadratur." },
  { "en": "Apa Itu Subtransient Reactance? (Xd'')", "id": "Reaktansi Saat Awal Hubung Singkat." },
  { "en": "Apa Itu Transient Reactance? (Xd')", "id": "Reaktansi Beberapa Siklus Setelah Gangguan." },
  { "en": "Apa Itu Synchronous Reactance? (Xs)", "id": "Reaktansi Saat Operasi Stabil." },
  { "en": "Apa Itu Time Constant Generator?", "id": "Waktu Peluruhan Arus Hubung Singkat." },
  { "en": "Apa Itu Short Circuit Ratio? (SCR)", "id": "Ukuran Kestabilan Relatif Generator." },
  { "en": "Apa Itu Inertia Constant? (H)", "id": "Energi Kinetik Rotor Per MVA." },
  { "en": "Apa Itu Damping Winding?", "id": "Lilitan Peredam Osilasi Pada Rotor." },
  { "en": "Apa Itu Negative Sequence Heating?", "id": "Pemanasan Rotor Akibat Beban Unbalance." },
  { "en": "Apa Itu Shaft Current Protection?", "id": "Proteksi Arus Bocor Poros Generator." },
  { "en": "Apa Itu Bearing Insulation?", "id": "Isolasi Bantalan Cegah Arus Poros." },
  { "en": "Apa Itu Hydrogen Purity?", "id": "Persentase Gas Hidrogen Pendingin Generator." },
  { "en": "Apa Itu Stator Water Conductivity?", "id": "Kemurnian Air Pendingin Lilitan Stator." },
  { "en": "Apa Itu Seal Oil Pressure?", "id": "Tekanan Minyak Penyekat Gas Hidrogen." },
  { "en": "Apa Itu Excitation Ceiling Voltage?", "id": "Tegangan Maksimum Sistem Eksitasi." },
  { "en": "Apa Itu Field Forcing?", "id": "Memaksa Arus Medan Maksimum Sesaat." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Alat Peredam Ayunan Daya Generator." },
  { "en": "Apa Itu Automatic Synchronizer?", "id": "Alat Sinkronisasi Generator Ke Grid." },
  { "en": "Apa Itu Dead Bus Sensor?", "id": "Deteksi Busbar Tidak Bertegangan." },
  { "en": "Apa Itu Synchroscope?", "id": "Meter Penunjuk Beda Fasa Sudut." },
  { "en": "Apa Itu Phase Angle Meter?", "id": "Alat Ukur Sudut Fasa Listrik." },
  { "en": "Apa Itu Frequency Meter?", "id": "Alat Ukur Frekuensi Gelombang Listrik." },
  { "en": "Apa Itu Power Factor Meter?", "id": "Alat Ukur Faktor Daya (Cosphi)." },
  { "en": "Apa Itu Var Meter?", "id": "Alat Ukur Daya Reaktif (VAR)." },
  { "en": "Apa Itu Watt Meter?", "id": "Alat Ukur Daya Aktif (Watt)." },
  { "en": "Apa Itu Watt-Hour Meter?", "id": "Alat Ukur Energi Listrik (KWH)." },
  { "en": "Apa Itu Maximum Demand Meter?", "id": "Pencatat Beban Puncak Rata-Rata." },
  { "en": "Apa Itu Smart Meter?", "id": "Meter Listrik Digital Komunikasi Dua Arah." },
  { "en": "Apa Itu Prepayment Meter?", "id": "Meter Listrik Sistem Bayar Dimuka." },
  { "en": "Apa Itu CT Metering Class?", "id": "Kelas Akurasi Trafo Arus Meter." },
  { "en": "Apa Itu VT Metering Class?", "id": "Kelas Akurasi Trafo Tegangan Meter." },
  { "en": "Apa Itu Burden Metering?", "id": "Beban Impedansi Rangkaian Sekunder Meter." },
  { "en": "Apa Itu Test Block Meter?", "id": "Terminal Uji Dan Kalibrasi Meter." },
  { "en": "Apa Itu Phantom Load Test?", "id": "Uji Meter Dengan Beban Buatan." },
  { "en": "Apa Itu Meter Seal?", "id": "Segel Pengaman Anti Tamper Meter." },
  { "en": "Apa Itu Tamper Detection?", "id": "Deteksi Gangguan Ilegal Pada Meter." },
  { "en": "Apa Itu Reverse Energy?", "id": "Energi Listrik Mengalir Balik Arah." },
  { "en": "Apa Itu Import-Export Meter?", "id": "Meter Ukur Dua Arah Aliran." },
  { "en": "Apa Itu Four Quadrant Metering?", "id": "Pengukuran Aktif Reaktif Maju Mundur." },
  { "en": "Apa Itu Load Profile?", "id": "Rekaman Data Beban Per Interval." },
  { "en": "Apa Itu Time of Use Tariff?", "id": "Tarif Listrik Berbeda Berdasarkan Waktu." },
  { "en": "Apa Itu Peak Load Tariff?", "id": "Tarif Mahal Saat Beban Puncak." },
  { "en": "Apa Itu Off-Peak Tariff?", "id": "Tarif Murah Saat Luar Puncak." },
  { "en": "Apa Itu Automatic Meter Reading? (AMR)", "id": "Pembacaan Meter Jarak Jauh Otomatis." },
  { "en": "Apa Itu Advanced Metering Infrastructure? (AMI)", "id": "Infrastruktur Meter Cerdas Terintegrasi." },
  { "en": "Apa Itu Meter Data Management? (MDM)", "id": "Sistem Pengelola Data Meter AMI." },
  { "en": "Apa Itu Head End System?", "id": "Pusat Pengumpul Data Meter Cerdas." },
  { "en": "Apa Itu Meter Tamper Switch?", "id": "Saklar Deteksi Pembukaan Tutup Meter." },
  { "en": "Apa Itu Optical Port Probe?", "id": "Alat Baca Meter Lewat Inframerah." },
  { "en": "Apa Itu Load Survey Data?", "id": "Rekaman Data Beban Berkala Meter." },
  { "en": "Apa Itu Metering CT Class?", "id": "Kelas Akurasi Trafo Arus Meter." },
  { "en": "Apa Itu Accuracy Class 0.2S?", "id": "Akurasi Tinggi Pada Arus Rendah." },
  { "en": "Apa Itu Burden Rating VT?", "id": "Beban Maksimum Trafo Tegangan Meter." },
  { "en": "Apa Itu Phantom Loading Test?", "id": "Uji Meter Dengan Beban Semu." },
  { "en": "Apa Itu Meter Error Calculation?", "id": "Menghitung Persentase Kesalahan Ukur Meter." },
  { "en": "Apa Itu Creep Test KWH?", "id": "Uji Putaran Piringan Tanpa Arus." },
  { "en": "Apa Itu Starting Current Test?", "id": "Uji Arus Minimum Penggerak Meter." },
  { "en": "Apa Itu Dial Test Meter?", "id": "Uji Putaran Piringan Terhadap Register." },
  { "en": "Apa Itu Register Ratio?", "id": "Rasio Putaran Gigi Pencatat Meter." },
  { "en": "Apa Itu Meter Constant K?", "id": "Jumlah Putaran Per Kilowatt Jam." },
  { "en": "Apa Itu Pulse Constant?", "id": "Jumlah Pulsa Per Satuan Energi." },
  { "en": "Apa Itu Maximum Demand Indicator?", "id": "Jarum Penunjuk Beban Puncak Tertinggi." },
  { "en": "Apa Itu Cumulative Demand Register?", "id": "Total Akumulasi Beban Puncak Reset." },
  { "en": "Apa Itu Resetting Demand?", "id": "Mengembalikan Jarum Demand Ke Nol." },
  { "en": "Apa Itu Time Switch Meter?", "id": "Saklar Waktu Pindah Tarif Meter." },
  { "en": "Apa Itu Ripple Control Receiver?", "id": "Penerima Sinyal Kendali Jarak Jauh." },
  { "en": "Apa Itu Prepayment Token?", "id": "Kode Angka Pulsa Listrik Prabayar." },
  { "en": "Apa Itu STS Standard Token?", "id": "Standar Spesifikasi Transfer Standar Token." },
  { "en": "Apa Itu Key Change Token?", "id": "Token Pengganti Kunci Enkripsi Meter." },
  { "en": "Apa Itu Clear Credit Token?", "id": "Token Menghapus Sisa Kredit Meter." },
  { "en": "Apa Itu Power Limit Token?", "id": "Token Pengubah Batas Daya Meter." },
  { "en": "Apa Itu Tariff Index?", "id": "Indeks Tarif Pada Meter Digital." },
  { "en": "Apa Itu Fraud Detection?", "id": "Deteksi Kecurangan Pemakaian Listrik." },
  { "en": "Apa Itu Bypass Detection?", "id": "Deteksi Sambungan Langsung Tanpa Meter." },
  { "en": "Apa Itu Missing Potential?", "id": "Deteksi Hilangnya Tegangan Masukan Meter." },
  { "en": "Apa Itu Current Reversal?", "id": "Deteksi Arus Balik Pada Meter." },
  { "en": "Apa Itu Magnetic Influence?", "id": "Deteksi Medan Magnet Ganggu Meter." },
  { "en": "Apa Itu Top Cover Open?", "id": "Deteksi Tutup Atas Meter Dibuka." },
  { "en": "Apa Itu Terminal Cover Open?", "id": "Deteksi Tutup Terminal Meter Dibuka." },
  { "en": "Apa Itu Neutral Disturbance?", "id": "Gangguan Pada Kawat Netral Meter." },
  { "en": "Apa Itu Single Wire Earth?", "id": "Mengambil Listrik Lewat Tanah Langsung." },
  { "en": "Apa Itu Meter Box Seal?", "id": "Segel Pengaman Kotak Pelindung Meter." },
  { "en": "Apa Itu MCB Segel?", "id": "MCB Pembatas Daya Disegel PLN." },
  { "en": "Apa Itu P2TL Operasi?", "id": "Penertiban Pemakaian Tenaga Listrik Ilegal." },
  { "en": "Apa Itu Tagihan Susulan?", "id": "Tagihan Akibat Pelanggaran Pemakaian Listrik." },
  { "en": "Apa Itu Golongan Tarif R1?", "id": "Tarif Rumah Tangga Daya Kecil." },
  { "en": "Apa Itu Golongan Tarif B1?", "id": "Tarif Bisnis Skala Kecil." },
  { "en": "Apa Itu Golongan Tarif I3?", "id": "Tarif Industri Menengah Tegangan Menengah." },
  { "en": "Apa Itu Faktor Kali Meter?", "id": "Pengali Pembacaan Meter Pakai CT." },
  { "en": "Apa Itu Stand Meter Awal?", "id": "Angka Meter Awal Bulan Pembacaan." },
  { "en": "Apa Itu Stand Meter Akhir?", "id": "Angka Meter Akhir Bulan Pembacaan." },
  { "en": "Apa Itu Jam Nyala?", "id": "Rata-Rata Pemakaian Jam Per Bulan." },
  { "en": "Apa Itu Biaya Beban?", "id": "Biaya Tetap Abonemen Listrik Bulanan." },
  { "en": "Apa Itu Biaya Pemakaian?", "id": "Biaya Berdasarkan Jumlah KWH Terpakai." },
  { "en": "Apa Itu Pajak Penerangan Jalan?", "id": "Pajak Listrik Untuk Pemerintah Daerah." },
  { "en": "Apa Itu Materai Listrik?", "id": "Biaya Materai Pada Tagihan Listrik." },
  { "en": "Apa Itu Uang Jaminan Langganan?", "id": "Deposit Awal Pelanggan Pascabayar." },
  { "en": "Apa Itu Biaya Penyambungan?", "id": "Biaya Pasang Baru Listrik PLN." },
  { "en": "Apa Itu Sertifikat Laik Operasi?", "id": "Sertifikat Keamanan Instalasi Rumah." },
  { "en": "Apa Itu Lembaga Inspeksi Teknik?", "id": "Lembaga Penerbit Sertifikat Laik Operasi." },
  { "en": "Apa Itu NIDI?", "id": "Nomor Identitas Instalasi Tenaga Listrik." },
  { "en": "Apa Itu Instalatir Listrik?", "id": "Kontraktor Pemasang Instalasi Listrik." },
  { "en": "Apa Itu Diagram Pengawatan Rumah?", "id": "Gambar Rangkaian Kabel Instalasi Rumah." },
  { "en": "Apa Itu PHB Utama?", "id": "Panel Hubung Bagi Utama Gedung." },
  { "en": "Apa Itu Sub Panel?", "id": "Panel Cabang Dari Panel Utama." },
  { "en": "Apa Itu Grouping MCB?", "id": "Pembagian Kelompok Beban Pada MCB." },
  { "en": "Apa Itu Stop Kontak Arde?", "id": "Stop Kontak Dengan Terminal Grounding." },
  { "en": "Apa Itu Saklar Ganda?", "id": "Saklar Dua Tombol Dua Lampu." },
  { "en": "Apa Itu Saklar Tukar?", "id": "Saklar Hotel Kendali Dua Tempat." },
  { "en": "Apa Itu Saklar Silang?", "id": "Saklar Kendali Tiga Tempat Lebih." },
  { "en": "Apa Itu Fitting Plafon?", "id": "Dudukan Lampu Tempel Di Langit-Langit." },
  { "en": "Apa Itu Inbow Dus?", "id": "Kotak Saklar Tanam Dalam Tembok." },
  { "en": "Apa Itu Outbow Dus?", "id": "Kotak Saklar Tempel Luar Tembok." },
  { "en": "Apa Itu T-Dus Cabang?", "id": "Kotak Percabangan Kabel Instalasi Pipa." },
  { "en": "Apa Itu Embodus?", "id": "Nama Lain Dari Inbow Dus." },
  { "en": "Apa Itu Kabel NYM 3x2.5?", "id": "Kabel Tiga Kawat Dua Setengah." },
  { "en": "Apa Itu Warna Fasa R?", "id": "Merah (Standar Lama) Atau Hitam." },
  { "en": "Apa Itu Warna Fasa S?", "id": "Kuning (Standar Lama) Atau Cokelat." },
  { "en": "Apa Itu Warna Fasa T?", "id": "Hitam (Standar Lama) Atau Abu-Abu." },
  { "en": "Apa Itu Warna Netral?", "id": "Warna Biru Pada Kabel Instalasi." },
  { "en": "Apa Itu Warna Grounding?", "id": "Warna Hijau Kuning Loreng Kabel." },
  { "en": "Apa Itu Isolasi PVC?", "id": "Bahan Isolasi Kabel Paling Umum." },
  { "en": "Apa Itu Isolasi XLPE?", "id": "Isolasi Tahan Panas Dan Kuat." },
  { "en": "Apa Itu Konduit High Impact?", "id": "Pipa Pelindung Kabel Tahan Benturan." },
  { "en": "Apa Itu Klem Pipa?", "id": "Pengikat Pipa Konduit Ke Dinding." },
  { "en": "Apa Itu Elbow Pipa?", "id": "Sambungan Pipa Bentuk Siku L." },
  { "en": "Apa Itu Sock Pipa?", "id": "Sambungan Lurus Antar Dua Pipa." },
  { "en": "Apa Itu Lasdop Kabel?", "id": "Penutup Sambungan Kabel Puntir Plastik." },
  { "en": "Apa Itu Isolasi Tape?", "id": "Pita Perekat Isolator Sambungan Kabel." },
  { "en": "Apa Itu Tespen Listrik?", "id": "Obeng Deteksi Tegangan Fasa Listrik." },
  { "en": "Apa Itu Multitester Analog?", "id": "Alat Ukur Jarum Serbaguna Listrik." },
  { "en": "Apa Itu Multitester Digital?", "id": "Alat Ukur Angka Serbaguna Listrik." },
  { "en": "Apa Itu Clamp Meter?", "id": "Tang Ukur Arus Tanpa Putus." },
  { "en": "Apa Itu Earth Tester?", "id": "Alat Ukur Tahanan Pembumian Tanah." },
  { "en": "Apa Itu Insulation Tester?", "id": "Alat Ukur Tahanan Isolasi Kabel." },
  { "en": "Apa Itu Phase Detector?", "id": "Alat Cek Urutan Fasa Motor." },
  { "en": "Apa Itu Lux Meter?", "id": "Alat Ukur Tingkat Pencahayaan Lampu." },
  { "en": "Apa Itu Frequency Counter?", "id": "Alat Penghitung Frekuensi Sinyal Listrik." },
  { "en": "Apa Itu Power Meter?", "id": "Alat Ukur Daya Listrik Watt." },
  { "en": "Apa Itu Energy Meter Monitor?", "id": "Alat Pantau Pemakaian Energi Harian." },
  { "en": "Apa Itu Smart Plug?", "id": "Stop Kontak Pintar Kontrol Wifi." },
  { "en": "Apa Itu Smart Switch?", "id": "Saklar Pintar Kontrol Via HP." },
  { "en": "Apa Itu Motion Sensor Switch?", "id": "Saklar Otomatis Deteksi Gerakan Orang." },
  { "en": "Apa Itu Photocell Switch?", "id": "Saklar Otomatis Cahaya Matahari (PJU)." },
  { "en": "Apa Itu Timer Switch?", "id": "Saklar Otomatis Berdasarkan Waktu Setting." },
  { "en": "Apa Itu Standar IEC 60044?", "id": "Standar Internasional Untuk Transformator Instrumen." },
  { "en": "Apa Itu Standar IEC 60255?", "id": "Standar Internasional Untuk Relay Proteksi." },
  { "en": "Apa Itu Standar IEC 60099?", "id": "Standar Internasional Untuk Surge Arrester." },
  { "en": "Apa Itu Standar IEEE C37.118?", "id": "Standar Untuk Synchrophasor Pada Sistem Tenaga." },
  { "en": "Apa Itu Breakdown Voltage Minyak?", "id": "Tegangan Tembus Listrik Minyak Isolasi." },
  { "en": "Apa Itu Acidity Test Minyak?", "id": "Mengukur Kadar Asam Oksidasi Minyak." },
  { "en": "Apa Itu Flash Point Minyak?", "id": "Suhu Terendah Uap Minyak Menyala." },
  { "en": "Apa Itu Interfacial Tension Minyak?", "id": "Tegangan Permukaan Antara Minyak Dan Air." },
  { "en": "Apa Itu Tan Delta Minyak?", "id": "Faktor Disipasi Dielektrik Cairan Isolasi." },
  { "en": "Apa Itu Gas Hidrogen Trafo?", "id": "Gas Terlarut Indikasi Partial Discharge." },
  { "en": "Apa Itu Gas Metana Trafo?", "id": "Gas Terlarut Indikasi Pemanasan Minyak." },
  { "en": "Apa Itu Gas Etana Trafo?", "id": "Gas Terlarut Indikasi Panas Menengah." },
  { "en": "Apa Itu Gas Etilena Trafo?", "id": "Gas Terlarut Indikasi Panas Tinggi." },
  { "en": "Apa Itu Gas Asetilena Trafo?", "id": "Gas Terlarut Indikasi Busur Api." },
  { "en": "Apa Itu Rogers Ratio Method?", "id": "Metode Analisis Perbandingan Gas DGA." },
  { "en": "Apa Itu Key Gas Method?", "id": "Analisis Berdasarkan Gas Paling Dominan." },
  { "en": "Apa Itu Furan Analysis?", "id": "Analisis Kimia Degradasi Kertas Isolasi." },
  { "en": "Apa Itu Degree of Polymerization?", "id": "Ukuran Panjang Rantai Selulosa Kertas." },
  { "en": "Apa Itu Corrosive Sulfur?", "id": "Senyawa Sulfur Merusak Konduktor Tembaga." },
  { "en": "Apa Itu Copper Sulfide?", "id": "Endapan Hitam Konduktif Pada Isolasi." },
  { "en": "Apa Itu Passivator Minyak?", "id": "Zat Penahan Reaksi Sulfur Korosif." },
  { "en": "Apa Itu Oil Reclamation?", "id": "Proses Kimia Mengembalikan Kualitas Minyak." },
  { "en": "Apa Itu Oil Degassing?", "id": "Proses Membuang Gas Terlarut Minyak." },
  { "en": "Apa Itu Vacuum Drying Minyak?", "id": "Pengeringan Minyak Menggunakan Ruang Hampa." },
  { "en": "Apa Itu Emisivitas Termografi?", "id": "Efisiensi Permukaan Memancarkan Radiasi Panas." },
  { "en": "Apa Itu Delta-T Termografi?", "id": "Selisih Suhu Titik Panas Normal." },
  { "en": "Apa Itu Solar Blind Camera?", "id": "Kamera Korona Tidak Terganggu Matahari." },
  { "en": "Apa Itu UV Detection?", "id": "Deteksi Sinar Ultraviolet Dari Korona." },
  { "en": "Apa Itu Ultrasonic Arcing Detection?", "id": "Deteksi Suara Frekuensi Tinggi Arcing." },
  { "en": "Apa Satuan Contact Resistance?", "id": "Mikro Ohm." },
  { "en": "Apa Itu Breaker Timing Test?", "id": "Uji Kecepatan Buka Tutup Kontak." },
  { "en": "Apa Itu Travel Curve Analysis?", "id": "Analisis Grafik Pergerakan Mekanik Breaker." },
  { "en": "Apa Itu Breaker Damping?", "id": "Peredaman Kejut Akhir Gerakan Breaker." },
  { "en": "Apa Itu Spring Charging Failure?", "id": "Gagalnya Motor Mengisi Pegas Breaker." },
  { "en": "Apa Itu Trip Coil Burnout?", "id": "Terbakarnya Kumparan Trip Akibat Macet." },
  { "en": "Apa Itu Anti-Pumping Logic?", "id": "Mencegah Breaker Tutup Buka Berulang." },
  { "en": "Apa Itu Pole Discrepancy Logic?", "id": "Proteksi Ketidaksamaan Posisi Fasa Breaker." },
  { "en": "Apa Itu Circuit Breaker Endurance?", "id": "Ketahanan Breaker Terhadap Operasi Berulang." },
  { "en": "Apa Itu Making Capacity?", "id": "Kemampuan Menutup Pada Arus Gangguan." },
  { "en": "Apa Itu Breaking Capacity?", "id": "Kemampuan Memutus Arus Hubung Singkat." },
  { "en": "Apa Itu Asymmetrical Breaking Current?", "id": "Arus Gangguan Dengan Komponen DC." },
  { "en": "Apa Itu DC Component Fault?", "id": "Komponen Searah Pada Arus Gangguan." },
  { "en": "Apa Itu X/R Ratio System?", "id": "Rasio Reaktansi Terhadap Resistansi Sistem." },
  { "en": "Apa Itu Fault Level Calculation?", "id": "Perhitungan Besar Arus Hubung Singkat." },
  { "en": "Apa Itu Per Unit System?", "id": "Sistem Satuan Relatif Terhadap Basis." },
  { "en": "Apa Itu Base MVA?", "id": "Nilai Daya Dasar Perhitungan PU." },
  { "en": "Apa Itu Base Voltage?", "id": "Nilai Tegangan Dasar Perhitungan PU." },
  { "en": "Apa Itu Impedance Diagram?", "id": "Diagram Nilai Impedansi Komponen Sistem." },
  { "en": "Apa Itu Reactance Diagram?", "id": "Diagram Nilai Reaktansi Komponen Sistem." },
  { "en": "Apa Itu Symmetrical Components?", "id": "Metode Analisis Sistem Tidak Seimbang." },
  { "en": "Apa Itu Positive Sequence Network?", "id": "Rangkaian Urutan Positif (Normal)." },
  { "en": "Apa Itu Negative Sequence Network?", "id": "Rangkaian Urutan Negatif (Tidak Seimbang)." },
  { "en": "Apa Itu Zero Sequence Network?", "id": "Rangkaian Urutan Nol (Gangguan Tanah)." },
  { "en": "Apa Itu Sequence Filter Relay?", "id": "Rangkaian Pemisah Komponen Urutan Fasa." },
  { "en": "Apa Itu Relay Sampling Rate?", "id": "Frekuensi Pengambilan Data Sinyal Analog." },
  { "en": "Apa Itu Analog Digital Converter?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu Anti-Aliasing Filter?", "id": "Filter Mencegah Kesalahan Sampling Sinyal." },
  { "en": "Apa Itu Digital Filtering?", "id": "Pemrosesan Sinyal Secara Matematika Digital." },
  { "en": "Apa Itu Fourier Transform Relay?", "id": "Ekstraksi Fasor Fundamental Dari Gelombang." },
  { "en": "Apa Itu Phasor Estimation?", "id": "Perhitungan Besar Dan Sudut Fasor." },
  { "en": "Apa Itu Frequency Tracking?", "id": "Penyesuaian Sampling Terhadap Frekuensi Sistem." },
  { "en": "Apa Itu Trip Logic?", "id": "Logika Keputusan Pemutusan Sirkuit." },
  { "en": "Apa Itu Binary Input Relay?", "id": "Input Status Digital Ke Relay." },
  { "en": "Apa Itu Binary Output Relay?", "id": "Output Kontak Kontrol Dari Relay." },
  { "en": "Apa Itu Watchdog Contact?", "id": "Kontak Indikasi Kesehatan Internal Relay." },
  { "en": "Apa Itu Relay Self-Test?", "id": "Diagnostik Otomatis Internal Relay Proteksi." },
  { "en": "Apa Itu Event Recording?", "id": "Pencatatan Kejadian Perubahan Status Sistem." },
  { "en": "Apa Itu Disturbance Record?", "id": "Rekaman Gelombang Saat Terjadi Gangguan." },
  { "en": "Apa Itu COMTRADE File?", "id": "Format Standar Data Rekaman Gangguan." },
  { "en": "Apa Itu Logical Node IEC61850?", "id": "Blok Fungsi Standar Dalam Relay." },
  { "en": "Apa Itu Data Set IEC61850?", "id": "Kumpulan Data Untuk Dikirimkan." },
  { "en": "Apa Itu Report Control Block?", "id": "Pengaturan Pengiriman Laporan Ke SCADA." },
  { "en": "Apa Itu GOOSE Publishing?", "id": "Pengiriman Pesan Cepat Antar Relay." },
  { "en": "Apa Itu GOOSE Subscription?", "id": "Penerimaan Pesan Cepat Dari Relay." },
  { "en": "Apa Itu Sampled Value Publishing?", "id": "Pengiriman Data Arus Tegangan Digital." },
  { "en": "Apa Itu Merging Unit Sync?", "id": "Sinkronisasi Waktu Pengambil Sampel Data." },
  { "en": "Apa Itu Process Bus Bandwidth?", "id": "Kapasitas Data Jaringan Level Proses." },
  { "en": "Apa Itu Station Bus Architecture?", "id": "Struktur Jaringan Level Gardu Induk." },
  { "en": "Apa Itu Redundant Ethernet?", "id": "Jaringan Ganda Untuk Keandalan Tinggi." },
  { "en": "Apa Itu PRP Protocol?", "id": "Protokol Redundansi Paralel Tanpa Jeda." },
  { "en": "Apa Itu HSR Protocol?", "id": "Protokol Redundansi Cincin Tanpa Jeda." },
  { "en": "Apa Itu Substation Cyber Security?", "id": "Keamanan Sistem Digital Gardu Induk." },
  { "en": "Apa Itu Role Based Access?", "id": "Hak Akses Berdasarkan Jabatan User." },
  { "en": "Apa Itu Password Complexity?", "id": "Aturan Kerumitan Kata Sandi Sistem." },
  { "en": "Apa Itu Security Logging?", "id": "Pencatatan Aktivitas Akses Ke Sistem." },
  { "en": "Apa Itu Firewall Configuration?", "id": "Pengaturan Aturan Lalu Lintas Jaringan." },
  { "en": "Apa Itu Intrusion Detection System?", "id": "Pendeteksi Serangan Jaringan Mencurigakan." },
  { "en": "Apa Itu Physical Security Substation?", "id": "Pengamanan Fisik Area Gardu Induk." },
  { "en": "Apa Itu Access Control System?", "id": "Sistem Kunci Pintu Elektronik Terpusat." },
  { "en": "Apa Itu Video Surveillance?", "id": "Sistem Kamera CCTV Pemantau Gardu." },
  { "en": "Apa Itu Fire Detection System?", "id": "Sistem Sensor Pendeteksi Kebakaran Dini." },
  { "en": "Apa Itu Linear Heat Detector?", "id": "Kabel Sensor Panas Sepanjang Jalur." },
  { "en": "Apa Itu Aspirating Smoke Detector?", "id": "Detektor Asap Sistem Hisap Udara." },
  { "en": "Apa Itu Flame Detector?", "id": "Sensor Optik Deteksi Lidah Api." },
  { "en": "Apa Itu Water Mist System?", "id": "Pemadam Api Kabut Air Halus." },
  { "en": "Apa Itu Nitrogen Injection System?", "id": "Suntikan Nitrogen Mencegah Ledakan Trafo." },
  { "en": "Apa Itu Transformer Explosion Prevention?", "id": "Teknologi Mencegah Tangki Trafo Meledak." },
  { "en": "Apa Itu Oil Pit Drainage?", "id": "Saluran Buang Minyak Dari Bak." },
  { "en": "Apa Itu Oil Water Separator?", "id": "Pemisah Minyak Dari Air Hujan." },
  { "en": "Apa Itu Bund Wall?", "id": "Tembok Penahan Tumpahan Minyak Trafo." },
  { "en": "Apa Itu Fire Wall Rating?", "id": "Ketahanan Waktu Tembok Terhadap Api." },
  { "en": "Apa Itu Blast Wall?", "id": "Tembok Penahan Gelombang Ledakan Trafo." },
  { "en": "Apa Itu Safety Clearance Distance?", "id": "Jarak Aman Minimum Bagi Manusia." },
  { "en": "Apa Itu Minimum Electrical Clearance?", "id": "Jarak Aman Terpendek Antar Konduktor." },
  { "en": "Apa Itu Phase To Earth Clearance?", "id": "Jarak Aman Fasa Ke Tanah." },
  { "en": "Apa Itu Phase To Phase Clearance?", "id": "Jarak Aman Antar Kabel Fasa." },
  { "en": "Apa Itu Section Clearance?", "id": "Jarak Aman Kerja Sekitar Peralatan." },
  { "en": "Apa Itu Ground Clearance SUTT?", "id": "Jarak Kabel Terendah Ke Tanah." },
  { "en": "Apa Itu Sagging Line?", "id": "Lendutan Kabel Akibat Berat Sendiri." },
  { "en": "Apa Itu Stringing Chart?", "id": "Tabel Hubungan Suhu Dan Sag." },
  { "en": "Apa Itu Ruling Span Design?", "id": "Jarak Bentang Rata-Rata Desain Tower." },
  { "en": "Apa Itu Wind Span?", "id": "Panjang Kabel Terkena Beban Angin." },
  { "en": "Apa Itu Weight Span?", "id": "Berat Kabel Ditanggung Satu Tower." },
  { "en": "Apa Itu Uplift Force Tower?", "id": "Gaya Angkat Kabel Pada Isolator." },
  { "en": "Apa Itu Counterweight Tower?", "id": "Pemberat Mencegah Gaya Angkat Kabel." },
  { "en": "Apa Itu Vibration Damper Placement?", "id": "Jarak Pasang Peredam Dari Klem." },
  { "en": "Apa Itu Armor Rods Function?", "id": "Melindungi Kabel Dari Kelelahan Tekukan." },
  { "en": "Apa Itu Spacer Damper Installation?", "id": "Pemasangan Pemisah Kabel Berkas Berjarak." },
  { "en": "Apa Itu Mid Span Joint?", "id": "Sambungan Kabel Di Tengah Bentangan." },
  { "en": "Apa Itu Compression Dead End?", "id": "Klem Tarik Kabel Metode Press." },
  { "en": "Apa Itu Jumper Connection Tower?", "id": "Kabel Penghubung Fasa Lewat Tower." },
  { "en": "Apa Itu Bird Guard Tower?", "id": "Alat Mencegah Burung Hinggap Tower." },
  { "en": "Apa Itu Aircraft Warning Sphere?", "id": "Bola Penanda Kabel Jalur Pesawat." },
  { "en": "Apa Itu Tower Painting?", "id": "Pengecatan Tower Anti Karat Pesawat." },
  { "en": "Apa Itu Tower Footing Resistance?", "id": "Nilai Tahanan Pentanahan Kaki Tower." },
  { "en": "Apa Itu Impulse Grounding Resistance?", "id": "Tahanan Tanah Saat Dialiri Petir." },
  { "en": "Apa Itu Counterpoise Wire Installation?", "id": "Menanam Kawat Tanah Sepanjang Jalur." },
  { "en": "Apa Itu Soil Ionization Effect?", "id": "Tanah Jadi Konduktif Saat Petir." },
  { "en": "Apa Itu Back Flashover Phenomenon?", "id": "Loncatan Api Dari Tower Kabel." },
  { "en": "Apa Itu Shielding Angle?", "id": "Sudut Perlindungan Kawat Tanah Atas." },
  { "en": "Apa Itu Number Of Insulator?", "id": "Jumlah Piringan Isolator Sesuai Tegangan." },
  { "en": "Apa Itu Pollution Level Design?", "id": "Desain Isolator Sesuai Tingkat Polusi." },
  { "en": "Apa Itu Leakage Distance Ratio?", "id": "Jarak Rambat Per Kilovolt Tegangan." },
  { "en": "Apa Itu Silicon Coating Insulator?", "id": "Melapisi Isolator Keramik Dengan Silikon." },
  { "en": "Apa Itu Booster Shed?", "id": "Sirip Tambahan Peningkat Jarak Rambat." },
  { "en": "Apa Itu RTV Silicone Coating?", "id": "Lapisan Karet Silikon Vulkanisir Suhu." },
  { "en": "Apa Itu Hydrophobicity Transfer?", "id": "Sifat Tolak Air Pindah Ke Kotoran." },
  { "en": "Apa Itu Dry Band Arcing?", "id": "Busur Api Kecil Permukaan Basah." },
  { "en": "Apa Itu Corona Ring Grading?", "id": "Cincin Perata Tegangan Ujung Isolator." },
  { "en": "Apa Itu Grading Ring Design?", "id": "Bentuk Cincin Kendali Medan Listrik." },
  { "en": "Apa Itu Arcing Horn Gap?", "id": "Jarak Sela Tanduk Api Pelindung." },
  { "en": "Apa Itu Tower Surge Impedance?", "id": "Impedansi Tower Terhadap Gelombang Petir." },
  { "en": "Apa Itu Gantry Substation?", "id": "Struktur Baja Penyangga Busbar Gantung." },
  { "en": "Apa Itu Busbar Sag?", "id": "Lendutan Kabel Busbar Gantung Gardu." },
  { "en": "Apa Itu Short Circuit Force?", "id": "Gaya Mekanis Saat Hubung Singkat." },
  { "en": "Apa Itu Rigid Busbar System?", "id": "Sistem Busbar Pipa Aluminium Kaku." },
  { "en": "Apa Itu Expansion Joint Busbar?", "id": "Sambungan Pipa Mengatasi Pemuaian Panas." },
  { "en": "Apa Itu Post Insulator Cantilever?", "id": "Kekuatan Tekuk Isolator Tumpu Busbar." },
  { "en": "Apa Itu Pantograph Reach Range?", "id": "Jangkauan Kontak Pemisah Model Gunting." },
  { "en": "Apa Itu Disconnector Ice Breaking?", "id": "Kemampuan Pemisah Memecah Lapisan Es." },
  { "en": "Apa Itu Motor Drive Mechanism?", "id": "Kotak Motor Penggerak Peralatan Gardu." },
  { "en": "Apa Itu Manual Operating Handle?", "id": "Tuas Operasi Manual Saat Darurat." },
  { "en": "Apa Itu Local Remote Selector?", "id": "Saklar Pilih Kendali Lokal Jauh." },
  { "en": "Apa Itu Heater Cubicle Panel?", "id": "Pemanas Anti Embun Dalam Panel." },
  { "en": "Apa Itu Thermostat Heater Control?", "id": "Pengatur Suhu Otomatis Pemanas Panel." },
  { "en": "Apa Itu Hygrostat Heater Control?", "id": "Pengatur Kelembaban Otomatis Pemanas Panel." },
  { "en": "Apa Itu Cable Trench Cover?", "id": "Penutup Beton Parit Kabel Gardu." },
  { "en": "Apa Itu Fire Stop Mortar?", "id": "Semen Tahan Api Penutup Lubang." },
  { "en": "Apa Itu Cable Transits System?", "id": "Sistem Penutup Lubang Kabel Modular." },
  { "en": "Apa Itu Raised Floor Room?", "id": "Lantai Panggung Ruang Kontrol Server." },
  { "en": "Apa Itu Mimic Mosaic Tile?", "id": "Kotak Gambar Diagram Papan Kontrol." },
  { "en": "Apa Itu Annunciator Alarm Window?", "id": "Jendela Lampu Peringatan Gangguan Panel." },
  { "en": "Apa Itu Horn Cancel Button?", "id": "Tombol Matikan Suara Sirine Alarm." },
  { "en": "Apa Itu Lamp Test Button?", "id": "Tombol Uji Nyala Semua Indikator." },
  { "en": "Apa Itu Semaphore Indicator Flag?", "id": "Bendera Mekanis Penunjuk Posisi Saklar." },
  { "en": "Apa Itu Control Switch Mismatch?", "id": "Posisi Saklar Beda Dengan Peralatan." },
  { "en": "Apa Itu Synchronizing Socket?", "id": "Lubang Colokan Alat Cek Sinkron." },
  { "en": "Apa Itu Test Terminal Block?", "id": "Terminal Uji Injeksi Arus Relay." },
  { "en": "Apa Itu Knife Disconnect Terminal?", "id": "Terminal Kabel Bisa Dibuka Pisau." },
  { "en": "Apa Itu Ferrule Wire Marker?", "id": "Nomor Penanda Ujung Kabel Kontrol." },
  { "en": "Apa Itu Spiral Wrapping Band?", "id": "Pelindung Kabel Kontrol Spiral Plastik." },
  { "en": "Apa Itu Cable Duct Wiring?", "id": "Jalur Kabel Plastik Dalam Panel." },
  { "en": "Apa Itu Din Rail Mounting?", "id": "Rel Standar Dudukan MCB Terminal." },
  { "en": "Apa Itu End Stopper Terminal?", "id": "Penahan Terminal Blok Agar Rapat." },
  { "en": "Apa Itu Busbar Comb Insulated?", "id": "Sisir Busbar Jamper MCB Panel." },
  { "en": "Apa Itu Step Down Trafo?", "id": "Trafo Penurun Tegangan Ke Pelanggan." },
  { "en": "Apa Itu Step Up Trafo?", "id": "Trafo Penaik Tegangan Dari Pembangkit." },
  { "en": "Apa Itu Shell Type Transformer?", "id": "Inti Besi Mengelilingi Lilitan Trafo." },
  { "en": "Apa Itu Core Type Transformer?", "id": "Lilitan Mengelilingi Inti Besi Trafo." },
  { "en": "Apa Itu Interleaved Winding?", "id": "Susunan Lilitan Primer Sekunder Seling." },
  { "en": "Apa Itu Continuous Disc Winding?", "id": "Lilitan Cakram Tanpa Sambungan Las." },
  { "en": "Apa Itu Shielded Winding Design?", "id": "Lilitan Dengan Pelindung Medan Elektrostatis." },
  { "en": "Apa Itu Transformer Tank Vacuum?", "id": "Kemampuan Tangki Tahan Tekanan Hampa." },
  { "en": "Apa Itu Skid Base Transformer?", "id": "Dudukan Dasar Trafo Bisa Geser." },
  { "en": "Apa Itu Impact Recorder Trafo?", "id": "Pencatat Guncangan Saat Pengiriman Trafo." },
  { "en": "Apa Itu N2 Injection System?", "id": "Suntik Nitrogen Mencegah Ledakan Trafo." },
  { "en": "Apa Itu Transformer Noise Level?", "id": "Tingkat Kebisingan Suara Dengung Trafo." },
  { "en": "Apa Itu Magnetostriction Effect?", "id": "Perubahan Fisik Inti Akibat Magnet." },
  { "en": "Apa Itu Sound Wall Barrier?", "id": "Tembok Peredam Suara Bising Trafo." },
  { "en": "Apa Itu Gravel Fire Suppression?", "id": "Batu Koral Pemadam Api Minyak." },
  { "en": "Apa Itu Fire Wall Separator?", "id": "Tembok Tahan Api Antar Trafo." },
  { "en": "Apa Itu Water Spray System?", "id": "Sistem Semprot Air Pemadam Trafo." },
  { "en": "Apa Itu Heat Detector Cable?", "id": "Kabel Sensor Panas Sekeliling Trafo." },
  { "en": "Apa Itu Buchholz Gas Sampling?", "id": "Ambil Gas Dari Relay Buchholz." },
  { "en": "Apa Itu Syringe Oil Sampling?", "id": "Ambil Minyak Pakai Suntikan Kaca." },
  { "en": "Apa Itu DGA Diagnostic Tool?", "id": "Software Analisis Gas Terlarut Minyak." },
  { "en": "Apa Itu Furfural Analysis Test?", "id": "Uji Degradasi Kertas Isolasi Trafo." },
  { "en": "Apa Itu Corrosive Sulfur Test?", "id": "Uji Potensi Korosi Tembaga Minyak." },
  { "en": "Apa Itu Passivator Additive Oil?", "id": "Zat Pencegah Korosi Pada Minyak." },
  { "en": "Apa Itu Sludge Content Test?", "id": "Uji Endapan Lumpur Dalam Minyak." },
  { "en": "Apa Itu Polarity Test Trafo?", "id": "Menentukan Arah Lilitan Primer Sekunder." },
  { "en": "Apa Itu Vector Group Test?", "id": "Memastikan Geseran Fasa Trafo Benar." },
  { "en": "Apa Itu Magnetizing Current Test?", "id": "Mendeteksi Masalah Inti Besi Trafo." },
  { "en": "Apa Itu Short Circuit Impedance Test?", "id": "Mengukur Impedansi Hubung Singkat Trafo." },
  { "en": "Apa Itu Zero Sequence Impedance Test?", "id": "Mengukur Impedansi Urutan Nol Trafo." },
  { "en": "Apa Itu Induced Voltage Test?", "id": "Uji Tahan Tegangan Antar Lilitan." },
  { "en": "Apa Itu Applied Voltage Test?", "id": "Uji Tahan Tegangan Ke Tanah." },
  { "en": "Apa Itu Heat Run Test Trafo?", "id": "Uji Kenaikan Suhu Beban Penuh." },
  { "en": "Apa Itu Sound Level Test Trafo?", "id": "Mengukur Kebisingan Suara Trafo." },
  { "en": "Apa Itu Harmonics Test Trafo?", "id": "Mengukur Arus Harmonisa Saat Eksitasi." },
  { "en": "Apa Itu Excitation Current Test?", "id": "Mengukur Arus Beban Nol Trafo." },
  { "en": "Apa Itu Core Ground Test?", "id": "Uji Tahanan Isolasi Inti Besi." },
  { "en": "Apa Itu Magnetic Balance Test?", "id": "Uji Keseimbangan Fluks Fasa Trafo." },
  { "en": "Apa Itu Capacitance Tan Delta Test?", "id": "Uji Faktor Daya Isolasi Trafo." },
  { "en": "Apa Itu Bushing Hot Collar Test?", "id": "Uji Isolasi Bushing Bagian Atas." },
  { "en": "Apa Itu Frequency Response Analysis? (SFRA)", "id": "Deteksi Deformasi Mekanis Lilitan Trafo." },
  { "en": "Apa Itu Dirana Test?", "id": "Uji Kadar Air Isolasi Kertas." },
  { "en": "Apa Itu Polarization Depolarization Current? (PDC)", "id": "Analisis Kondisi Isolasi Domain Waktu." },
  { "en": "Apa Itu Recovery Voltage Measurement? (RVM)", "id": "Metode Diagnostik Kelembaban Kertas Isolasi." },
  { "en": "Apa Itu Partial Discharge Inception Voltage?", "id": "Tegangan Awal Munculnya Peluahan Listrik." },
  { "en": "Apa Itu Partial Discharge Extinction Voltage?", "id": "Tegangan Berhentinya Peluahan Listrik." },
  { "en": "Apa Itu Generator Capability Curve?", "id": "Grafik Batas Operasi Aman Generator." },
  { "en": "Apa Itu Under Excited Limit? (UEL)", "id": "Batas Minimum Eksitasi Cegah Panas." },
  { "en": "Apa Itu Over Excited Limit? (OEL)", "id": "Batas Maksimum Eksitasi Cegah Overheat." },
  { "en": "Apa Itu Stator Current Limit?", "id": "Batas Arus Maksimum Lilitan Stator." },
  { "en": "Apa Itu Rotor Current Limit?", "id": "Batas Arus Maksimum Lilitan Medan." },
  { "en": "Apa Itu End Region Heating?", "id": "Panas Ujung Stator Saat Underexcited." },
  { "en": "Apa Itu Hydrogen Pressure Generator?", "id": "Tekanan Gas Pendingin Dalam Casing." },
  { "en": "Apa Itu Purity Hydrogen Gas?", "id": "Persentase Kemurnian Gas Hidrogen Pendingin." },
  { "en": "Apa Itu Seal Oil System?", "id": "Minyak Penahan Gas Hidrogen Bocor." },
  { "en": "Apa Itu Vacuum Treatment Seal Oil?", "id": "Membuang Udara Dari Minyak Seal." },
  { "en": "Apa Itu Differential Expansion Detector?", "id": "Deteksi Beda Muai Rotor Stator." },
  { "en": "Apa Itu Rotor Earth Fault Protection?", "id": "Deteksi Hubung Tanah Lilitan Rotor." },
  { "en": "Apa Itu Stator Earth Fault Protection?", "id": "Deteksi Hubung Tanah Lilitan Stator." },
  { "en": "Apa Itu 100 Percent Stator Ground?", "id": "Proteksi Tanah Melindungi Seluruh Lilitan." },
  { "en": "Apa Itu Third Harmonic Undervoltage?", "id": "Proteksi Tanah Titik Netral Generator." },
  { "en": "Apa Itu Injection Method Ground Fault?", "id": "Suntik Frekuensi Rendah Deteksi Tanah." },
  { "en": "Apa Itu Inadvertent Energization Logic?", "id": "Proteksi Trafo Menyuplai Generator Mati." },
  { "en": "Apa Itu Generator Motoring Protection?", "id": "Mencegah Generator Menjadi Motor Listrik." },
  { "en": "Apa Itu Loss of Prime Mover?", "id": "Hilangnya Tenaga Penggerak Utama Turbin." },
  { "en": "Apa Itu Sequential Tripping Generator?", "id": "Mencegah Overspeed Saat Trip Beban." },
  { "en": "Apa Itu Breaker Failure Protection?", "id": "Cadangan Jika Breaker Gagal Buka." },
  { "en": "Apa Itu Pole Slip Protection?", "id": "Deteksi Generator Lepas Langkah Sinkron." },
  { "en": "Apa Itu Out of Step Blocking?", "id": "Mencegah Trip Saat Ayunan Daya." },
  { "en": "Apa Itu Voltage Controlled Overcurrent?", "id": "Arus Lebih Dengan Penahan Tegangan." },
  { "en": "Apa Itu Voltage Restrained Overcurrent?", "id": "Setting Arus Turun Jika Tegangan Turun." },
  { "en": "Apa Itu Negative Sequence Protection?", "id": "Mencegah Pemanasan Rotor Akibat Unbalance." },
  { "en": "Apa Itu Vt Fuse Failure?", "id": "Deteksi Sekring Trafo Tegangan Putus." },
  { "en": "Apa Itu Dead Machine Protection?", "id": "Proteksi Saat Generator Tidak Operasi." },
  { "en": "Apa Itu Frequency Protection Generator?", "id": "Mencegah Operasi Frekuensi Tidak Normal." },
  { "en": "Apa Itu Overfluxing Protection Generator?", "id": "Mencegah Saturasi Inti Akibat V/Hz." },
  { "en": "Apa Itu Cable Sheath Test?", "id": "Uji Integritas Pelindung Luar Kabel." },
  { "en": "Apa Itu Cross Bonding Check?", "id": "Memastikan Sambungan Silang Sheath Benar." },
  { "en": "Apa Itu SVL Test Kabel?", "id": "Uji Arrester Pelindung Sheath Kabel." },
  { "en": "Apa Itu Contact Resistance Link Box?", "id": "Ukur Tahanan Kontak Terminal Box." },
  { "en": "Apa Itu Phase Phasing Check?", "id": "Memastikan Urutan Fasa Kabel Benar." },
  { "en": "Apa Itu High Voltage DC Test?", "id": "Uji Tegangan Tinggi Arus Searah." },
  { "en": "Apa Itu Very Low Frequency Test?", "id": "Uji Kabel AC Frekuensi 0.1Hz." },
  { "en": "Apa Itu Damped AC Test?", "id": "Uji Kabel Dengan Osilasi Teredam." },
  { "en": "Apa Itu Partial Discharge Mapping?", "id": "Lokalisasi Titik PD Sepanjang Kabel." },
  { "en": "Apa Itu Tan Delta Diagnosis?", "id": "Evaluasi Penuaan Global Isolasi Kabel." },
  { "en": "Apa Itu TDR Fault Location?", "id": "Radar Kabel Cari Jarak Gangguan." },
  { "en": "Apa Itu Impulse Current Method?", "id": "Metode Lokasi Gangguan Kabel Tanah." },
  { "en": "Apa Itu Arc Reflection Method?", "id": "Metode Pantulan Busur Cari Gangguan." },
  { "en": "Apa Itu Audio Frequency Method?", "id": "Pelacakan Jalur Kabel Dengan Suara." },
  { "en": "Apa Itu Pinpointing Fault Location?", "id": "Menentukan Titik Tepat Galian Kabel." },
  { "en": "Apa Itu Surge Generator Thumper?", "id": "Alat Hentakan Tegangan Cari Gangguan." },
  { "en": "Apa Itu Burning Fault Cable?", "id": "Membakar Isolasi Agar Tahanan Turun." },
  { "en": "Apa Itu Murray Loop Bridge?", "id": "Jembatan Ukur Lokasi Gangguan Kabel." },
  { "en": "Apa Itu Varley Loop Bridge?", "id": "Variasi Jembatan Ukur Lokasi Gangguan." },
  { "en": "Apa Itu Motor Locked Rotor Time?", "id": "Waktu Maksimum Tahan Arus Start." },
  { "en": "Apa Itu Motor Thermal Limit?", "id": "Batas Panas Isolasi Lilitan Motor." },
  { "en": "Apa Itu Number of Starts Limit?", "id": "Batas Jumlah Start Per Jam." },
  { "en": "Apa Itu Cooling Time Constant?", "id": "Waktu Motor Dingin Setelah Mati." },
  { "en": "Apa Itu Motor Acceleration Time?", "id": "Waktu Mencapai Kecepatan Penuh." },
  { "en": "Apa Itu Jamming Protection Motor?", "id": "Proteksi Macet Saat Motor Jalan." },
  { "en": "Apa Itu Undercurrent Protection Motor?", "id": "Deteksi Beban Hilang (Pompa Kosong)." },
  { "en": "Apa Itu Phase Reversal Protection?", "id": "Mencegah Motor Putar Terbalik." },
  { "en": "Apa Itu Zero Speed Switch?", "id": "Deteksi Poros Motor Benar Berhenti." },
  { "en": "Apa Itu Vibration Limit Motor?", "id": "Batas Getaran Aman Operasi Motor." },
  { "en": "Apa Itu Bearing Temperature Limit?", "id": "Batas Suhu Aman Bantalan Motor." },
  { "en": "Apa Itu Winding Temperature Limit?", "id": "Batas Suhu Aman Lilitan Stator." },
  { "en": "Apa Itu Motor Heater Control?", "id": "Mengaktifkan Pemanas Saat Motor Mati." },
  { "en": "Apa Itu Motor Feeder Cable?", "id": "Kabel Suplai Daya Ke Motor." },
  { "en": "Apa Itu Voltage Drop Motor Start?", "id": "Penurunan Tegangan Saat Motor Nyala." },
  { "en": "Apa Itu Remote Backup Protection?", "id": "Proteksi Cadangan Dari Gardu Sebelah." },
  { "en": "Apa Itu Local Backup Protection?", "id": "Proteksi Cadangan Di Gardu Sama." },
  { "en": "Apa Itu Duplicate Protection Scheme?", "id": "Dua Sistem Proteksi Identik Paralel." },
  { "en": "Apa Itu Main 1 Protection?", "id": "Sistem Proteksi Utama Pertama." },
  { "en": "Apa Itu Main 2 Protection?", "id": "Sistem Proteksi Utama Kedua." },
  { "en": "Apa Itu Busbar Protection Scheme?", "id": "Skema Proteksi Khusus Rel Daya." },
  { "en": "Apa Itu Check Zone Busbar?", "id": "Zona Verifikasi Gangguan Busbar." },
  { "en": "Apa Itu Discriminating Zone Busbar?", "id": "Zona Pemilih Bagian Busbar Terganggu." },
  { "en": "Apa Itu CT Supervision Busbar?", "id": "Deteksi Rangkaian CT Busbar Putus." },
  { "en": "Apa Itu Circuit Breaker Fail?", "id": "Logika Jika Breaker Gagal Buka." },
  { "en": "Apa Itu Retrip Logic?", "id": "Perintah Trip Ulang Ke Breaker." },
  { "en": "Apa Itu Backtrip Logic?", "id": "Perintah Trip Ke Breaker Lain." },
  { "en": "Apa Itu End Fault Logic?", "id": "Gangguan Antara CT Dan Breaker." },
  { "en": "Apa Itu Blind Spot Logic?", "id": "Area Tak Tercover CT Utama." },
  { "en": "Apa Itu Stability Voltage?", "id": "Tegangan Stabil Relay Diferensial High-Z." },
  { "en": "Apa Itu Konduktor ACSR/AW?", "id": "Konduktor Aluminium Penguat Baja Lapis Aluminium." },
  { "en": "Apa Itu Konduktor ACSR/TW?", "id": "Konduktor Bentuk Trapesium Padat (Trapezoidal)." },
  { "en": "Apa Itu Konduktor Gap-Type?", "id": "Konduktor Tahan Panas Celah Isi Gemuk." },
  { "en": "Apa Itu Konduktor Invar?", "id": "Konduktor Inti Paduan Besi Nikel Stabil." },
  { "en": "Apa Itu Konduktor TACSR?", "id": "Thermal Resistant Aluminium Alloy Conductor." },
  { "en": "Apa Itu Konduktor ZTACir?", "id": "Konduktor Zirkonium Tahan Suhu Ekstrem." },
  { "en": "Apa Itu Sub-Conductor Spacing?", "id": "Jarak Antar Kabel Dalam Satu Fasa." },
  { "en": "Apa Itu Bundle Collapsing?", "id": "Kabel Berkas Menempel Akibat Arus Hubung." },
  { "en": "Apa Itu Aeolian Vibration Frequency?", "id": "Frekuensi Getaran Angin 5 Hingga 100Hz." },
  { "en": "Apa Itu Wake Induced Oscillation?", "id": "Getaran Kabel Akibat Aliran Udara Turbulen." },
  { "en": "Apa Itu Ice Shedding?", "id": "Jatuhnya Es Dari Kabel Menyebabkan Lonjakan." },
  { "en": "Apa Itu Insulator Fog Type?", "id": "Isolator Sirip Dalam Untuk Daerah Polusi." },
  { "en": "Apa Itu Insulator Aerodynamic Type?", "id": "Isolator Sirip Datar Untuk Daerah Gurun." },
  { "en": "Apa Itu Insulator Zinc Sleeve?", "id": "Pelindung Pin Isolator Dari Korosi Elektrolisis." },
  { "en": "Apa Itu Glass Insulator Shell?", "id": "Bagian Kaca Piringan Isolator Gantung." },
  { "en": "Apa Itu Ball And Socket?", "id": "Mekanisme Sambungan Antar Piringan Isolator." },
  { "en": "Apa Itu Clevis And Tongue?", "id": "Mekanisme Sambungan Isolator Model Lidah." },
  { "en": "Apa Itu Cotter Pin Isolator?", "id": "Pin Pengunci Sambungan Ball Socket." },
  { "en": "Apa Itu Locking Key Isolator?", "id": "Kunci Pengaman Sambungan Isolator Gantung." },
  { "en": "Apa Itu Cap And Pin?", "id": "Konstruksi Dasar Isolator Piring." },
  { "en": "Apa Itu Cement Growth?", "id": "Pemuaian Semen Perusak Isolator Keramik." },
  { "en": "Apa Itu Puncture Voltage?", "id": "Tegangan Tembus Lewat Badan Isolator." },
  { "en": "Apa Itu Residual Strength Isolator?", "id": "Kekuatan Mekanis Sisa Setelah Pecah." },
  { "en": "Apa Itu Tower Body Extension?", "id": "Menambah Tinggi Badan Tower Transmisi." },
  { "en": "Apa Itu Hill Side Extension?", "id": "Kaki Tower Panjang Sebelah Di Lereng." },
  { "en": "Apa Itu Chimney Foundation?", "id": "Bagian Beton Pondasi Yang Muncul." },
  { "en": "Apa Itu Stub Setting Template?", "id": "Alat Bantu Posisikan Kaki Tower." },
  { "en": "Apa Itu Backfill Compaction?", "id": "Pemadatan Tanah Urug Pondasi Tower." },
  { "en": "Apa Itu Tower Verticality Check?", "id": "Memeriksa Ketegakan Tower Dengan Theodolite." },
  { "en": "Apa Itu Bolt Torque Check?", "id": "Memeriksa Kekencangan Baut Struktur Tower." },
  { "en": "Apa Itu Step Bolt Tower?", "id": "Baut Panjatan Kaki Pada Tower." },
  { "en": "Apa Itu Anti-Climbing Device?", "id": "Pagar Duri Mencegah Orang Memanjat." },
  { "en": "Apa Itu Number Plate Tower?", "id": "Plat Identitas Nomor Urut Tower." },
  { "en": "Apa Itu Danger Plate?", "id": "Plat Peringatan Bahaya Tegangan Tinggi." },
  { "en": "Apa Itu Phase Plate?", "id": "Plat Warna Penanda Fasa R-S-T." },
  { "en": "Apa Itu Bird Diverter?", "id": "Alat Visual Agar Burung Menghindar." },
  { "en": "Apa Itu Soil Resistivity Wenner?", "id": "Metode Standar Ukur Tahanan Jenis Tanah." },
  { "en": "Apa Itu Driven Rod Electrode?", "id": "Elektroda Batang Dipukul Masuk Tanah." },
  { "en": "Apa Itu Bentonite Treatment?", "id": "Bubuk Lempung Penurun Tahanan Tanah." },
  { "en": "Apa Itu Graphite Ground Module?", "id": "Modul Grounding Karbon Tahan Korosi." },
  { "en": "Apa Itu Electrolytic Grounding?", "id": "Sistem Grounding Pipa Garam Kimia." },
  { "en": "Apa Itu Mesh Conductor Size?", "id": "Ukuran Kawat Grid Pentanahan Gardu." },
  { "en": "Apa Itu Cadweld Connection?", "id": "Sambungan Las Eksotermik Kabel Tanah." },
  { "en": "Apa Itu Bolted Ground Connector?", "id": "Konektor Baut Kabel Grounding (Klem)." },
  { "en": "Apa Itu Compression Ground Tap?", "id": "Konektor Pres Kabel Grounding Huruf C." },
  { "en": "Apa Itu Grounding Riser?", "id": "Kabel Naik Dari Grid Ke Alat." },
  { "en": "Apa Itu Switchyard Surfacing?", "id": "Lapisan Batu Koral Atas Tanah Gardu." },
  { "en": "Apa Fungsi Batu Koral?", "id": "Meningkatkan Tahanan Kontak Kaki Manusia." },
  { "en": "Apa Itu Touch Potential Limit?", "id": "Batas Aman Tegangan Sentuh IEC." },
  { "en": "Apa Itu Step Potential Limit?", "id": "Batas Aman Tegangan Langkah IEC." },
  { "en": "Apa Itu Transferred Voltage?", "id": "Tegangan Tanah Pindah Lewat Kabel." },
  { "en": "Apa Itu Pilot Wire Grounding?", "id": "Pentanahan Kabel Pilot Relai Diferensial." },
  { "en": "Apa Itu Substation Fence Grounding?", "id": "Pentanahan Pagar Keliling Gardu Induk." },
  { "en": "Apa Itu Gate Bonding?", "id": "Menghubungkan Pintu Pagar Ke Ground." },
  { "en": "Apa Itu GIS Enclosure Grounding?", "id": "Pentanahan Casing Tabung Gas GIS." },
  { "en": "Apa Itu Multipoint Grounding?", "id": "Banyak Titik Pentanahan Pada Casing." },
  { "en": "Apa Itu Single Point Grounding?", "id": "Satu Titik Pentanahan Mencegah Arus Sirkulasi." },
  { "en": "Apa Itu Transient Ground Clamp?", "id": "Alat Ukur Arus Impuls Tanah." },
  { "en": "Apa Itu Relay Operating Time?", "id": "Waktu Relay Deteksi Hingga Kontak Tutup." },
  { "en": "Apa Itu Relay Reset Time?", "id": "Waktu Relay Kembali Normal Setelah Gangguan." },
  { "en": "Apa Itu Relay Overshoot?", "id": "Kecenderungan Relay Trip Meski Arus Turun." },
  { "en": "Apa Itu Impulse Limit Factor?", "id": "Faktor Batas Arus CT Tanpa Jenuh." },
  { "en": "Apa Itu Rated Short-Time Current?", "id": "Arus Hubung Singkat Tahan 1 Detik." },
  { "en": "Apa Itu Rated Dynamic Current?", "id": "Puncak Arus Pertama Tahan Gaya Mekanis." },
  { "en": "Apa Itu CT Secondary Resistance?", "id": "Tahanan Dalam Lilitan Sekunder CT." },
  { "en": "Apa Itu CT Exciting Current?", "id": "Arus Yang Hilang Untuk Magnetisasi Inti." },
  { "en": "Apa Itu CT Ratio Error?", "id": "Persen Kesalahan Besar Arus Sekunder." },
  { "en": "Apa Itu CT Phase Error?", "id": "Kesalahan Sudut Fasa Arus Sekunder." },
  { "en": "Apa Itu Burden Power Factor?", "id": "Faktor Daya Beban Rangkaian Sekunder." },
  { "en": "Apa Itu Summation CT?", "id": "CT Menjumlahkan Arus Beberapa Feeder." },
  { "en": "Apa Itu Core Balance CT?", "id": "CT Melingkari Tiga Fasa Kabel." },
  { "en": "Apa Itu Residual Connection?", "id": "Hubungan Tiga CT Untuk Arus Tanah." },
  { "en": "Apa Itu Open Delta VT?", "id": "Hubungan VT Untuk Deteksi Tegangan Nol." },
  { "en": "Apa Itu Broken Delta VT?", "id": "Istilah Lain Untuk Open Delta VT." },
  { "en": "Apa Itu VT Fuse Supervision?", "id": "Monitoring Kondisi Sekring Trafo Tegangan." },
  { "en": "Apa Itu VT MCB Supervision?", "id": "Monitoring Posisi MCB Trafo Tegangan." },
  { "en": "Apa Itu Voltage Selection Scheme?", "id": "Memilih Sumber Tegangan Sinkronisasi Otomatis." },
  { "en": "Apa Itu Busbar Voltage Reference?", "id": "Tegangan Busbar Untuk Referensi Relay." },
  { "en": "Apa Itu Line Voltage Reference?", "id": "Tegangan Saluran Untuk Referensi Relay." },
  { "en": "Apa Itu Synchrocheck Angle Limit?", "id": "Batas Beda Sudut Fasa Izin Tutup." },
  { "en": "Apa Itu Synchrocheck Voltage Diff?", "id": "Batas Beda Tegangan Izin Tutup." },
  { "en": "Apa Itu Synchrocheck Freq Diff?", "id": "Batas Beda Frekuensi Izin Tutup." },
  { "en": "Apa Itu Dead Bus Live Line?", "id": "Kondisi Bus Mati Saluran Hidup." },
  { "en": "Apa Itu Dead Line Live Bus?", "id": "Kondisi Saluran Mati Bus Hidup." },
  { "en": "Apa Itu Dead Dead Condition?", "id": "Kondisi Bus Dan Saluran Mati." },
  { "en": "Apa Itu Auto-Reclose Blocking?", "id": "Mencegah Reclose Pada Gangguan Permanen." },
  { "en": "Apa Itu Zone Selective Interlock?", "id": "Koordinasi Cepat Antar Relay Zona." },
  { "en": "Apa Itu Reverse Interlocking?", "id": "Sinyal Blokir Dari Hilir Ke Hulu." },
  { "en": "Apa Itu Transfer Trip Scheme?", "id": "Mengirim Perintah Trip Ke Breaker Jauh." },
  { "en": "Apa Itu Breaker Fail Initiate?", "id": "Sinyal Awal Timer Kegagalan Breaker." },
  { "en": "Apa Itu Trip Circuit Supervision?", "id": "Monitor Kontinuitas Koil Trip Breaker." },
  { "en": "Apa Itu Close Circuit Supervision?", "id": "Monitor Kontinuitas Koil Close Breaker." },
  { "en": "Apa Itu Protection Class 5P20?", "id": "Akurasi 5 Persen Pada 20x Arus." },
  { "en": "Apa Itu Protection Class 10P10?", "id": "Akurasi 10 Persen Pada 10x Arus." },
  { "en": "Apa Itu Instrument Security Factor?", "id": "Faktor Keamanan Melindungi Meter Dari Gangguan." },
  { "en": "Apa Itu Special Protection Scheme?", "id": "Sistem Proteksi Luas Jaga Kestabilan." },
  { "en": "Apa Itu Remedial Action Scheme?", "id": "Tindakan Perbaikan Otomatis Sistem Tenaga." },
  { "en": "Apa Itu Sampled Value Publisher?", "id": "Pengirim Data Arus Tegangan Digital." },
  { "en": "Apa Itu GOOSE Subscriber?", "id": "Penerima Pesan Cepat Antar Relay." },
  { "en": "Apa Itu Time Synchronization Accuracy?", "id": "Ketelitian Waktu Sinkronisasi Perangkat Digital." },
  { "en": "Apa Itu Logical Device IEC 61850?", "id": "Wadah Virtual Pengelompokan Logical Node." },
  { "en": "Apa Itu Logical Node GGIO?", "id": "Node Generik Input Output Umum." },
  { "en": "Apa Itu Logical Node XCBR?", "id": "Node Logika Untuk Circuit Breaker." },
  { "en": "Apa Itu Logical Node XSWI?", "id": "Node Logika Untuk Saklar Pemisah." },
  { "en": "Apa Itu Logical Node PDIS?", "id": "Node Logika Proteksi Jarak Digital." },
  { "en": "Apa Itu Logical Node PTOC?", "id": "Node Logika Proteksi Arus Lebih." },
  { "en": "Apa Itu CID File IEC 61850?", "id": "File Konfigurasi Spesifik Satu IED." },
  { "en": "Apa Itu SCD File IEC 61850?", "id": "File Konfigurasi Seluruh Gardu Induk." },
  { "en": "Apa Itu ICD File IEC 61850?", "id": "File Kapabilitas Standar Pabrikan IED." },
  { "en": "Apa Itu Client Server Communication?", "id": "Pola Komunikasi Vertikal Ke SCADA." },
  { "en": "Apa Itu Publisher Subscriber Communication?", "id": "Pola Komunikasi Horizontal Antar IED." },
  { "en": "Apa Itu VLAN Priority Tagging?", "id": "Prioritas Pengiriman Paket Data GOOSE." },
  { "en": "Apa Itu Multicast MAC Address?", "id": "Alamat Tujuan Banyak Perangkat Sekaligus." },
  { "en": "Apa Itu Fiber Optic Connector LC?", "id": "Konektor Fiber Kecil Densitas Tinggi." },
  { "en": "Apa Itu Fiber Optic Connector ST?", "id": "Konektor Fiber Bulat Model Bayonet." },
  { "en": "Apa Itu SFP Transceiver Module?", "id": "Modul Optik Plug-In Pada Switch." },
  { "en": "Apa Itu Redundant Ring Topology?", "id": "Topologi Jaringan Cincin Tahan Putus." },
  { "en": "Apa Itu Rapid Spanning Tree Protocol?", "id": "Protokol Pemulihan Jalur Jaringan Cepat." },
  { "en": "Apa Itu Media Redundancy Protocol? (MRP)", "id": "Protokol Redundansi Cincin Standar Industri." },
  { "en": "Apa Itu Parallel Redundancy Protocol? (PRP)", "id": "Kirim Data Lewat Dua Jaringan." },
  { "en": "Apa Itu High-availability Seamless Redundancy? (HSR)", "id": "Kirim Data Dua Arah Cincin." },
  { "en": "Apa Itu Latency Time Network?", "id": "Waktu Tempuh Data Dalam Jaringan." },
  { "en": "Apa Itu Jitter Network Delay?", "id": "Variasi Waktu Tunda Paket Data." },
  { "en": "Apa Itu Packet Loss Ratio?", "id": "Persentase Paket Data Yang Hilang." },
  { "en": "Apa Itu Cyber Security Key?", "id": "Kunci Enkripsi Amankan Data Digital." },
  { "en": "Apa Itu Role Based Access Control?", "id": "Hak Akses Sesuai Peran Pengguna." },
  { "en": "Apa Itu Syslog Server?", "id": "Server Penyimpan Catatan Log Sistem." },
  { "en": "Apa Itu SNMP Protocol?", "id": "Protokol Manajemen Perangkat Jaringan." },
  { "en": "Apa Itu Network Traffic Analyzer?", "id": "Alat Pantau Lalu Lintas Data." },
  { "en": "Apa Itu Wireshark Software?", "id": "Aplikasi Analisis Paket Data Jaringan." },
  { "en": "Apa Itu Digital Twin Substation?", "id": "Tiruan Digital Sistem Gardu Induk." },
  { "en": "Apa Itu Virtual Testing IEC 61850?", "id": "Uji Sistem Tanpa Perangkat Fisik." },
  { "en": "Apa Itu Relay Simulators?", "id": "Perangkat Lunak Simulasi Fungsi Relay." },
  { "en": "Apa Itu Substation Configuration Language? (SCL)", "id": "Bahasa Format File Konfigurasi IEC61850." },
  { "en": "Apa Itu Report Control Block?", "id": "Pengaturan Pengiriman Data Ke Klien." },
  { "en": "Apa Itu Buffered Report Control?", "id": "Simpan Data Jika Koneksi Putus." },
  { "en": "Apa Itu Unbuffered Report Control?", "id": "Data Hilang Jika Koneksi Putus." },
  { "en": "Apa Itu Dataset IEC 61850?", "id": "Kumpulan Data Yang Akan Dikirim." },
  { "en": "Apa Itu Dynamic Dataset?", "id": "Dataset Dibuat Saat Runtime Operasi." },
  { "en": "Apa Itu Static Dataset?", "id": "Dataset Dikonfigurasi Saat Desain Awal." },
  { "en": "Apa Itu Time Quality Bit?", "id": "Status Validitas Sinkronisasi Waktu Data." },
  { "en": "Apa Itu Simulation Bit?", "id": "Penanda Data Adalah Hasil Simulasi." },
  { "en": "Apa Itu Test Mode Relay?", "id": "Mode Relay Untuk Pengujian Fungsi." },
  { "en": "Apa Itu Local Remote Key?", "id": "Kunci Fisik Izin Kontrol Jarak Jauh." },
  { "en": "Apa Itu Interlocking Logic Digital?", "id": "Logika Kunci Antar Alat Via GOOSE." },
  { "en": "Apa Itu Bay Control Unit? (BCU)", "id": "Unit Kontrol Satu Bay Gardu." },
  { "en": "Apa Itu Gateway Substation?", "id": "Penghubung Protokol Gardu Ke Pusat." },
  { "en": "Apa Itu Protocol Converter?", "id": "Pengubah Bahasa Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Serial To Ethernet?", "id": "Konverter Perangkat Lama Ke Jaringan." },
  { "en": "Apa Itu Optical Splitter?", "id": "Pembagi Sinyal Cahaya Serat Optik." },
  { "en": "Apa Itu Fiber Patch Panel?", "id": "Rak Terminasi Kabel Serat Optik." },
  { "en": "Apa Itu Fusion Splicing Fiber?", "id": "Penyambungan Serat Optik Dengan Panas." },
  { "en": "Apa Itu OTDR Test Fiber?", "id": "Uji Refleksi Serat Optik Jarak." },
  { "en": "Apa Itu Optical Power Meter?", "id": "Alat Ukur Kekuatan Sinyal Cahaya." },
  { "en": "Apa Itu Splicing Loss?", "id": "Rugi Daya Pada Sambungan Fiber." },
  { "en": "Apa Itu Bending Radius Fiber?", "id": "Batas Tekukan Aman Kabel Optik." },
  { "en": "Apa Itu Armored Fiber Cable?", "id": "Kabel Optik Dengan Pelindung Baja." },
  { "en": "Apa Itu Dielectric Fiber Cable?", "id": "Kabel Optik Tanpa Unsur Logam." },
  { "en": "Apa Itu Multi Mode Fiber?", "id": "Serat Optik Inti Besar Jarak Pendek." },
  { "en": "Apa Itu Single Mode Fiber?", "id": "Serat Optik Inti Kecil Jarak Jauh." },
  { "en": "Apa Itu Wavelength Division Multiplexing?", "id": "Kirim Banyak Sinyal Satu Fiber." },
  { "en": "Apa Itu Dark Fiber?", "id": "Serat Optik Terpasang Belum Digunakan." },
  { "en": "Apa Itu Last Mile Connection?", "id": "Koneksi Ujung Jaringan Ke Pelanggan." },
  { "en": "Apa Itu Smart Grid Sensors?", "id": "Sensor Cerdas Pantau Kondisi Jaringan." },
  { "en": "Apa Itu Fault Passage Indicator?", "id": "Indikator Jalur Arus Gangguan Kabel." },
  { "en": "Apa Itu Overhead Line Sensor?", "id": "Sensor Arus Suhu Di Kabel Udara." },
  { "en": "Apa Itu Online Dissolved Gas?", "id": "Monitor Gas Trafo Secara Langsung." },
  { "en": "Apa Itu Transformer Bushing Monitor?", "id": "Pantau Kondisi Isolasi Bushing Trafo." },
  { "en": "Apa Itu Partial Discharge Monitor?", "id": "Pantau Peluahan Listrik Isolasi HV." },
  { "en": "Apa Itu Circuit Breaker Monitor?", "id": "Pantau Mekanik Dan Gas Breaker." },
  { "en": "Apa Itu Battery Health Monitor?", "id": "Pantau Impedansi Dan Tegangan Baterai." },
  { "en": "Apa Itu Infrared Thermography Monitor?", "id": "Pantau Suhu Panas Sambungan Busbar." },
  { "en": "Apa Itu Video Surveillance System?", "id": "Sistem Kamera Keamanan Gardu Induk." },
  { "en": "Apa Itu Motion Detection CCTV?", "id": "Deteksi Gerakan Pada Kamera Pengawas." },
  { "en": "Apa Itu Perimeter Protection?", "id": "Keamanan Pagar Keliling Gardu Induk." },
  { "en": "Apa Itu Access Control System?", "id": "Sistem Kunci Pintu Kartu Elektronik." },
  { "en": "Apa Itu Fire Alarm System?", "id": "Sistem Deteksi Dini Kebakaran Gardu." },
  { "en": "Apa Itu Smoke Detector?", "id": "Sensor Deteksi Asap Kebakaran." },
  { "en": "Apa Itu Heat Detector?", "id": "Sensor Deteksi Kenaikan Suhu Ruang." },
  { "en": "Apa Itu Flame Detector?", "id": "Sensor Deteksi Lidah Api Kebakaran." },
  { "en": "Apa Itu Aspirating Smoke Detector?", "id": "Detektor Asap Hisap Sensitivitas Tinggi." },
  { "en": "Apa Itu Linear Heat Detector?", "id": "Kabel Sensor Panas Sepanjang Tray." },
  { "en": "Apa Itu Manual Call Point?", "id": "Tombol Alarm Kebakaran Manual Pecah Kaca." },
  { "en": "Apa Itu Fire Extinguisher CO2?", "id": "Pemadam Api Gas Karbon Dioksida." },
  { "en": "Apa Itu Fire Extinguisher Powder?", "id": "Pemadam Api Serbuk Kimia Kering." },
  { "en": "Apa Itu Fire Extinguisher Foam?", "id": "Pemadam Api Busa (Tidak Untuk Listrik)." },
  { "en": "Apa Itu Transformer Deluge System?", "id": "Sistem Semprot Air Pemadam Trafo." },
  { "en": "Apa Itu Nitrogen Injection System?", "id": "Suntik Nitrogen Cegah Ledakan Trafo." },
  { "en": "Apa Itu Blast Wall?", "id": "Tembok Penahan Ledakan Trafo Daya." },
  { "en": "Apa Itu Oil Pit?", "id": "Bak Penampung Tumpahan Minyak Trafo." },
  { "en": "Apa Itu Gravel Bed?", "id": "Lapisan Batu Koral Pemadam Api." },
  { "en": "Apa Itu Emergency Lighting?", "id": "Lampu Darurat Saat Listrik Padam." },
  { "en": "Apa Itu Exit Sign Light?", "id": "Lampu Penunjuk Arah Keluar Darurat." },
  { "en": "Apa Itu Live Line Working?", "id": "Pekerjaan Listrik Dalam Keadaan Bertegangan." },
  { "en": "Apa Itu Hot Stick Method?", "id": "Kerja Bertegangan Menggunakan Tongkat Isolasi." },
  { "en": "Apa Itu Bare Hand Method?", "id": "Pekerja Menyentuh Kabel Bertegangan Langsung." },
  { "en": "Apa Itu Conductive Suit?", "id": "Baju Konduktif Pelindung Medan Listrik." },
  { "en": "Apa Itu Insulated Aerial Device?", "id": "Mobil Tangga Isolasi Kerja Bertegangan." },
  { "en": "Apa Itu Minimum Approach Distance?", "id": "Jarak Aman Minimum Kerja Bertegangan." },
  { "en": "Apa Itu Hotline Tools?", "id": "Peralatan Khusus Untuk Pekerjaan Bertegangan." },
  { "en": "Apa Itu Rubber Insulating Gloves?", "id": "Sarung Tangan Karet Tahan Listrik." },
  { "en": "Apa Itu Leather Protector Gloves?", "id": "Pelindung Kulit Luar Sarung Tangan." },
  { "en": "Apa Itu Insulating Blanket?", "id": "Selimut Karet Penutup Bagian Bertegangan." },
  { "en": "Apa Itu Line Hose?", "id": "Selang Karet Penutup Kabel Bertegangan." },
  { "en": "Apa Itu Conductor Cover?", "id": "Penutup Keras Isolasi Kabel Udara." },
  { "en": "Apa Itu Crossarm Cover?", "id": "Penutup Isolasi Lengan Tiang Listrik." },
  { "en": "Apa Itu Pole Cover?", "id": "Penutup Isolasi Tiang Listrik Kayu." },
  { "en": "Apa Itu Bypass Jumper Cable?", "id": "Kabel Pintas Arus Sementara Isolasi." },
  { "en": "Apa Itu Load Pickup Tool?", "id": "Alat Ambil Beban Saat Switching." },
  { "en": "Apa Itu Voltage Detector Stick?", "id": "Tongkat Deteksi Ada Tidaknya Tegangan." },
  { "en": "Apa Itu Phasing Stick?", "id": "Alat Cek Kesamaan Fasa Bertegangan." },
  { "en": "Apa Itu Universal Stick?", "id": "Tongkat Isolasi Kepala Bisa Diganti." },
  { "en": "Apa Itu Grip All Stick?", "id": "Tongkat Penjepit Serbaguna (Shotgun)." },
  { "en": "Apa Itu Wire Tong?", "id": "Tongkat Penahan Dan Pemindah Kabel." },
  { "en": "Apa Itu Tie Stick?", "id": "Tongkat Pembuka Kawat Pengikat Isolator." },
  { "en": "Apa Itu Cutter Stick?", "id": "Tongkat Pemotong Kabel Jarak Jauh." },
  { "en": "Apa Itu Ratchet Stick?", "id": "Tongkat Kunci Pas Putar Baut." },
  { "en": "Apa Itu Strain Carrier?", "id": "Alat Penahan Tarikan Isolator Tension." },
  { "en": "Apa Itu Platform Board?", "id": "Papan Pijakan Isolasi Di Tiang." },
  { "en": "Apa Itu Rope Block Tackle?", "id": "Katrol Tali Untuk Angkat Beban." },
  { "en": "Apa Itu Canvas Bag?", "id": "Tas Kanvas Wadah Peralatan Kerja." },
  { "en": "Apa Itu Silicone Cloth?", "id": "Kain Pembersih Peralatan Isolasi." },
  { "en": "Apa Itu Leakage Current Monitor?", "id": "Alat Ukur Arus Bocor Boom." },
  { "en": "Apa Itu Boom Current Monitor?", "id": "Monitor Arus Bocor Lengan Mobil." },
  { "en": "Apa Itu Outrigger Pad?", "id": "Alas Kaki Penstabil Mobil Tangga." },
  { "en": "Apa Itu Grounding Set?", "id": "Kabel Pentanahan Sementara Saat Padam." },
  { "en": "Apa Itu Cluster Bar?", "id": "Terminal Pentanahan Tiang Hubung Singkat." },
  { "en": "Apa Itu Ground Clamp?", "id": "Klem Penjepit Kabel Grounding." },
  { "en": "Apa Itu Voltage Regulator Bypass?", "id": "Saklar Pintas Regulator Tegangan Jaringan." },
  { "en": "Apa Itu Recloser Bypass Switch?", "id": "Saklar Pintas Pemeliharaan Recloser." },
  { "en": "Apa Itu Disconnect Switch Stick?", "id": "Tongkat Operasi Saklar Pemisah Gantung." },
  { "en": "Apa Itu Fuse Puller Stick?", "id": "Tongkat Pencabut Sekring Cut Out." },
  { "en": "Apa Itu Lamp Replacer Stick?", "id": "Tongkat Pengganti Lampu Jalan Umum." },
  { "en": "Apa Itu Tree Trimmer Stick?", "id": "Tongkat Gergaji Potong Dahan Pohon." },
  { "en": "Apa Itu Measuring Stick?", "id": "Tongkat Ukur Tinggi Kabel Udara." },
  { "en": "Apa Itu Cleaning Kit Stick?", "id": "Set Pembersih Tongkat Hot Stick." },
  { "en": "Apa Itu Wax Polishing?", "id": "Pelapis Lilin Pelindung Tongkat Isolasi." },
  { "en": "Apa Itu Dielectric Test Set?", "id": "Alat Uji Tahanan Isolasi Tongkat." },
  { "en": "Apa Itu Glove Inflator?", "id": "Alat Pompa Uji Sarung Tangan." },
  { "en": "Apa Itu Visual Inspection Glove?", "id": "Pemeriksaan Fisik Sarung Tangan Karet." },
  { "en": "Apa Itu Air Test Glove?", "id": "Uji Kebocoran Udara Sarung Tangan." },
  { "en": "Apa Itu Rubber Sleeve?", "id": "Pelindung Lengan Bahu Bahan Karet." },
  { "en": "Apa Itu Harness Safety Belt?", "id": "Sabuk Pengaman Panjat Tiang Listrik." },
  { "en": "Apa Itu Fall Arrester?", "id": "Alat Penahan Jatuh Pada Tali." },
  { "en": "Apa Itu Lanyard Safety?", "id": "Tali Pengait Sabuk Pengaman." },
  { "en": "Apa Itu Pole Strap?", "id": "Tali Pengikat Pinggang Ke Tiang." },
  { "en": "Apa Itu Climber Irons?", "id": "Besi Cakar Panjat Tiang Kayu." },
  { "en": "Apa Itu Step Ladder Fiberglass?", "id": "Tangga Lipat Bahan Isolasi Fiber." },
  { "en": "Apa Itu Extension Ladder?", "id": "Tangga Sorong Panjang Bahan Fiber." },
  { "en": "Apa Itu Rescue Hook?", "id": "Tongkat Penyelamat Korban Sengatan Listrik." },
  { "en": "Apa Itu CPR Manikin?", "id": "Boneka Latihan Resusitasi Jantung Paru." },
  { "en": "Apa Itu First Aid Kit?", "id": "Kotak Pertolongan Pertama Pada Kecelakaan." },
  { "en": "Apa Itu Danger Sign?", "id": "Rambu Peringatan Bahaya Tegangan Tinggi." },
  { "en": "Apa Itu Caution Tape?", "id": "Pita Kuning Pembatas Area Kerja." },
  { "en": "Apa Itu Traffic Cone?", "id": "Kerucut Pembatas Lalu Lintas Jalan." },
  { "en": "Apa Itu Work Permit System?", "id": "Sistem Izin Kerja Tertulis Resmi." },
  { "en": "Apa Itu Job Safety Analysis?", "id": "Analisis Potensi Bahaya Sebelum Kerja." },
  { "en": "Apa Itu Tailgate Meeting?", "id": "Brefing Keselamatan Sebelum Mulai Kerja." },
  { "en": "Apa Itu Lock Out Tag Out?", "id": "Prosedur Isolasi Energi Dan Pelabelan." },
  { "en": "Apa Itu Personal Protective Equipment?", "id": "Alat Pelindung Diri (APD) Lengkap." },
  { "en": "Apa Itu Arc Flash Shield?", "id": "Kaca Pelindung Wajah Dari Busur." },
  { "en": "Apa Itu Balaclava Fire Resistant?", "id": "Penutup Kepala Tahan Api." },
  { "en": "Apa Itu Ear Plug Protection?", "id": "Sumbat Telinga Pelindung Kebisingan." },
  { "en": "Apa Itu Safety Glasses?", "id": "Kacamata Pelindung Mata Dari Partikel." },
  { "en": "Apa Itu Safety Shoes Electrical?", "id": "Sepatu Safety Tahan Tegangan Listrik." },
  { "en": "Apa Itu Rain Coat Safety?", "id": "Jas Hujan Warna Terang Reflektif." },
  { "en": "Apa Itu Vest Reflective?", "id": "Rompi Keselamatan Dengan Pita Reflektor." },
  { "en": "Apa Itu Radio Communication?", "id": "Alat Komunikasi Handy Talkie Tim." },
  { "en": "Apa Itu Voltage Detector Audio?", "id": "Detektor Tegangan Dengan Suara Alarm." },
  { "en": "Apa Itu Phase Rotation Meter?", "id": "Alat Cek Urutan Putaran Fasa." },
  { "en": "Apa Itu Clamp Meter Digital?", "id": "Tang Ampere Ukur Arus Kabel." },
  { "en": "Apa Itu Insulation Tester Megger?", "id": "Alat Ukur Tahanan Isolasi Kabel." },
  { "en": "Apa Itu Earth Tester Digital?", "id": "Alat Ukur Tahanan Pentanahan." },
  { "en": "Apa Itu Thermal Imaging Camera?", "id": "Kamera Deteksi Titik Panas Sambungan." },
  { "en": "Apa Itu Corona Camera?", "id": "Kamera Deteksi Sinar UV Korona." },
  { "en": "Apa Itu Cable Locator?", "id": "Alat Deteksi Jalur Kabel Bawah Tanah." },
  { "en": "Apa Itu Fault Locator?", "id": "Alat Deteksi Titik Kerusakan Kabel." },
  { "en": "Apa Itu Thumper Surge Generator?", "id": "Alat Hentakan Tegangan Cari Gangguan." },
  { "en": "Apa Itu Time Domain Reflectometer?", "id": "Radar Kabel Ukur Jarak Gangguan." },
  { "en": "Apa Itu Transformer Turns Ratio?", "id": "Alat Ukur Rasio Belitan Trafo." },
  { "en": "Apa Itu Winding Resistance Meter?", "id": "Alat Ukur Tahanan Lilitan Trafo." },
  { "en": "Apa Itu Sweep Frequency Analyzer?", "id": "Alat Analisis Respon Frekuensi Trafo." },
  { "en": "Apa Itu Circuit Breaker Analyzer?", "id": "Alat Analisis Waktu Kerja Breaker." },
  { "en": "Apa Itu Battery Impedance Tester?", "id": "Alat Ukur Impedansi Internal Baterai." },
  { "en": "Apa Itu Relay Test Set?", "id": "Alat Uji Injeksi Relay Proteksi." },
  { "en": "Apa Itu Primary Injection Kit?", "id": "Alat Uji Injeksi Arus Primer." },
  { "en": "Apa Itu Secondary Injection Kit?", "id": "Alat Uji Injeksi Arus Sekunder." },
  { "en": "Apa Itu Lux Meter Digital?", "id": "Alat Ukur Intensitas Cahaya Lampu." },
  { "en": "Apa Itu Sound Level Meter?", "id": "Alat Ukur Tingkat Kebisingan Suara." },
  { "en": "Apa Itu Anemometer Digital?", "id": "Alat Ukur Kecepatan Angin Digital." },
  { "en": "Apa Itu Hygrometer Digital?", "id": "Alat Ukur Kelembaban Udara Ruangan." },
  { "en": "Apa Itu Contact Tachometer?", "id": "Alat Ukur Putaran Dengan Sentuhan." },
  { "en": "Apa Itu Laser Distance Meter?", "id": "Alat Ukur Jarak Sinar Laser." },
  { "en": "Apa Itu Ultrasonic Thickness Gauge?", "id": "Alat Ukur Ketebalan Plat Logam." },
  { "en": "Apa Itu Coating Thickness Gauge?", "id": "Alat Ukur Ketebalan Cat Logam." },
  { "en": "Apa Itu Vibration Analyzer?", "id": "Alat Analisis Spektrum Getaran Mesin." },
  { "en": "Apa Itu Oil Quality Analyzer?", "id": "Alat Analisis Kualitas Minyak Portable." },
  { "en": "Apa Itu Dew Point Meter?", "id": "Alat Ukur Titik Embun Gas." },
  { "en": "Apa Itu Gas Leak Detector?", "id": "Alat Deteksi Kebocoran Gas Pipa." },
  { "en": "Apa Itu Personal Gas Monitor?", "id": "Monitor Gas Beracun Untuk Pekerja." },
  { "en": "Apa Itu Oxygen Deficiency Monitor?", "id": "Deteksi Kekurangan Oksigen Ruang Terbatas." },
  { "en": "Apa Itu Confined Space Entry?", "id": "Prosedur Masuk Ruang Terbatas Berbahaya." },
  { "en": "Apa Itu Tripod Rescue System?", "id": "Alat Penyelamat Ruang Terbatas Vertikal." },
  { "en": "Apa Itu Blower Ventilasi Portable?", "id": "Kipas Sirkulasi Udara Ruang Terbatas." },
  { "en": "Apa Itu Full Body Harness?", "id": "Sabuk Pengaman Seluruh Tubuh Jatuh." },
  { "en": "Apa Itu Shock Absorber Lanyard?", "id": "Tali Pengait Peredam Kejut Jatuh." },
  { "en": "Apa Itu Self Retracting Lifeline?", "id": "Tali Keselamatan Gulung Otomatis Jatuh." },
  { "en": "Apa Itu Safety Helmet Chin Strap?", "id": "Tali Dagu Helm Keselamatan Kerja." },
  { "en": "Apa Itu Safety Glasses Anti-Fog?", "id": "Kacamata Pelindung Anti Kabut Embun." },
  { "en": "Apa Itu Ear Muff Protection?", "id": "Penutup Telinga Peredam Bising Luar." },
  { "en": "Apa Itu Ear Plug Protection?", "id": "Sumbat Telinga Masuk Lubang Telinga." },
  { "en": "Apa Itu Face Shield Visor?", "id": "Pelindung Wajah Dari Percikan Kimia." },
  { "en": "Apa Itu Welding Mask Auto-Darkening?", "id": "Topeng Las Gelap Otomatis Cahaya." },
  { "en": "Apa Itu Chemical Resistant Gloves?", "id": "Sarung Tangan Tahan Bahan Kimia." },
  { "en": "Apa Itu Cut Resistant Gloves?", "id": "Sarung Tangan Tahan Sayatan Pisau." },
  { "en": "Apa Itu Heat Resistant Gloves?", "id": "Sarung Tangan Tahan Panas Tinggi." },
  { "en": "Apa Itu Safety Shoes Steel Toe?", "id": "Sepatu Keselamatan Pelindung Jari Besi." },
  { "en": "Apa Itu Safety Boots Rubber?", "id": "Sepatu Bot Karet Anti Air." },
  { "en": "Apa Itu High Visibility Vest?", "id": "Rompi Keselamatan Warna Terang Pantul." },
  { "en": "Apa Itu Fire Retardant Coverall?", "id": "Baju Kerja Tahan Api Terpadu." },
  { "en": "Apa Itu Safety Sign Prohibition?", "id": "Rambu Larangan Simbol Lingkaran Merah." },
  { "en": "Apa Itu Safety Sign Mandatory?", "id": "Rambu Wajib Simbol Lingkaran Biru." },
  { "en": "Apa Itu Safety Sign Warning?", "id": "Rambu Peringatan Simbol Segitiga Kuning." },
  { "en": "Apa Itu Safety Sign Emergency?", "id": "Rambu Darurat Simbol Kotak Hijau." },
  { "en": "Apa Itu Safety Sign Fire?", "id": "Rambu Kebakaran Simbol Kotak Merah." },
  { "en": "Apa Itu Lockout Hasp Scissors?", "id": "Gunting Pengunci Banyak Gembok LOTO." },
  { "en": "Apa Itu Master Key System?", "id": "Kunci Induk Membuka Banyak Gembok." },
  { "en": "Apa Itu Key Management Box?", "id": "Kotak Manajemen Kunci Ruang Kontrol." },
  { "en": "Apa Itu Permit To Work?", "id": "Izin Kerja Tertulis Resmi Perusahaan." },
  { "en": "Apa Itu Hot Work Permit?", "id": "Izin Kerja Panas Las Gerinda." },
  { "en": "Apa Itu Cold Work Permit?", "id": "Izin Kerja Dingin Tanpa Api." },
  { "en": "Apa Itu Confined Space Permit?", "id": "Izin Masuk Ruang Terbatas Berbahaya." },
  { "en": "Apa Itu Working At Height Permit?", "id": "Izin Bekerja Di Ketinggian Tertentu." },
  { "en": "Apa Itu Electrical Isolation Certificate?", "id": "Sertifikat Isolasi Listrik Aman Kerja." },
  { "en": "Apa Itu Excavation Permit?", "id": "Izin Penggalian Tanah Area Pabrik." },
  { "en": "Apa Itu Lifting Plan?", "id": "Rencana Pengangkatan Beban Berat Crane." },
  { "en": "Apa Itu Job Safety Analysis? (JSA)", "id": "Analisis Bahaya Pekerjaan Langkah Demi Langkah." },
  { "en": "Apa Itu Hazard Identification Risk Assessment?", "id": "Identifikasi Bahaya Dan Penilaian Risiko." },
  { "en": "Apa Itu Method Statement Work?", "id": "Dokumen Metode Langkah Kerja Teknis." },
  { "en": "Apa Itu Toolbox Meeting?", "id": "Rapat Keselamatan Singkat Sebelum Kerja." },
  { "en": "Apa Itu Safety Induction Training?", "id": "Pelatihan Keselamatan Awal Masuk Kerja." },
  { "en": "Apa Itu Emergency Response Plan?", "id": "Rencana Tanggap Darurat Situasi Kritis." },
  { "en": "Apa Itu Evacuation Drill?", "id": "Latihan Evakuasi Keadaan Darurat Gedung." },
  { "en": "Apa Itu Muster Point?", "id": "Titik Kumpul Evakuasi Keadaan Darurat." },
  { "en": "Apa Itu Wind Sock Indicator?", "id": "Penunjuk Arah Angin Evakuasi Gas." },
  { "en": "Apa Itu First Aider?", "id": "Petugas Pertolongan Pertama Pada Kecelakaan." },
  { "en": "Apa Itu Eye Wash Station?", "id": "Stasiun Pencuci Mata Darurat Kimia." },
  { "en": "Apa Itu Safety Shower?", "id": "Pancuran Air Pembersih Badan Darurat." },
  { "en": "Apa Itu Spill Kit Oil?", "id": "Paket Pembersih Tumpahan Minyak Cepat." },
  { "en": "Apa Itu Absorbent Boom?", "id": "Busa Penyerap Minyak Di Air." },
  { "en": "Apa Itu Absorbent Granules?", "id": "Butiran Penyerap Tumpahan Minyak Lantai." },
  { "en": "Apa Itu Waste Management Plan?", "id": "Rencana Pengelolaan Limbah Proyek Industri." },
  { "en": "Apa Itu Hazardous Waste Label?", "id": "Label Limbah Bahan Berbahaya Beracun." },
  { "en": "Apa Itu Material Safety Data Sheet?", "id": "Lembar Data Keselamatan Bahan Kimia." },
  { "en": "Apa Itu Energy Audit Industrial?", "id": "Audit Pemakaian Energi Pabrik Industri." },
  { "en": "Apa Itu Energy Baseline?", "id": "Garis Dasar Konsumsi Energi Acuan." },
  { "en": "Apa Itu Energy Performance Indicator? (EnPI)", "id": "Indikator Kinerja Energi Terukur Spesifik." },
  { "en": "Apa Itu Specific Energy Consumption?", "id": "Konsumsi Energi Per Satuan Produk." },
  { "en": "Apa Itu Power Factor Penalty?", "id": "Denda Faktor Daya Rendah PLN." },
  { "en": "Apa Itu kVARh Charge?", "id": "Biaya Pemakaian Daya Reaktif Berlebih." },
  { "en": "Apa Itu Peak Load Shifting?", "id": "Menggeser Beban Puncak Ke Malam." },
  { "en": "Apa Itu Maximum Demand Control?", "id": "Mengendalikan Beban Agar Tidak Puncak." },
  { "en": "Apa Itu Variable Frequency Drive Savings?", "id": "Penghematan Energi Motor Pakai Inverter." },
  { "en": "Apa Itu High Efficiency Motor?", "id": "Motor Listrik Hemat Energi (IE3)." },
  { "en": "Apa Itu LED Lighting Retrofit?", "id": "Mengganti Lampu Lama Dengan LED." },
  { "en": "Apa Itu Occupancy Sensor Control?", "id": "Kontrol Lampu Sensor Gerak Hemat." },
  { "en": "Apa Itu Daylighting Control?", "id": "Memanfaatkan Cahaya Matahari Siang Hari." },
  { "en": "Apa Itu Building Management System? (BMS)", "id": "Sistem Otomasi Pengendali Utilitas Gedung." },
  { "en": "Apa Itu HVAC Energy Saving?", "id": "Penghematan Energi Sistem Pendingin Udara." },
  { "en": "Apa Itu Chiller Plant Optimization?", "id": "Optimasi Kinerja Mesin Pendingin Sentral." },
  { "en": "Apa Itu Compressed Air Leak?", "id": "Kebocoran Udara Tekan Boros Energi." },
  { "en": "Apa Itu Steam Trap Maintenance?", "id": "Perawatan Perangkap Uap Hemat Energi." },
  { "en": "Apa Itu Boiler Efficiency Tuning?", "id": "Penyetelan Pembakaran Boiler Hemat BBM." },
  { "en": "Apa Itu Waste Heat Recovery?", "id": "Pemanfaatan Panas Buang Untuk Energi." },
  { "en": "Apa Itu Regenerative Braking Elevator?", "id": "Lift Hasilkan Listrik Saat Turun." },
  { "en": "Apa Itu Smart Grid Technology?", "id": "Teknologi Jaringan Listrik Cerdas Digital." },
  { "en": "Apa Itu Distributed Generation System?", "id": "Pembangkit Listrik Tersebar Dekat Beban." },
  { "en": "Apa Itu Microgrid Controller?", "id": "Pengendali Jaringan Listrik Mikro Mandiri." },
  { "en": "Apa Itu Energy Storage System?", "id": "Sistem Penyimpanan Energi Skala Besar." },
  { "en": "Apa Itu Demand Response Program?", "id": "Program Pengurangan Beban Sukarela Pelanggan." },
  { "en": "Apa Itu Virtual Power Plant?", "id": "Pembangkit Maya Gabungan Banyak Sumber." },
  { "en": "Apa Itu Renewable Energy Certificate?", "id": "Sertifikat Bukti Energi Terbarukan Hijau." },
  { "en": "Apa Itu Carbon Footprint Calculation?", "id": "Perhitungan Jejak Emisi Karbon Operasi." },
  { "en": "Apa Itu Green Building Certification?", "id": "Sertifikasi Bangunan Hijau Ramah Lingkungan." },
  { "en": "Apa Itu LEED Certification?", "id": "Standar Sertifikasi Bangunan Hijau Amerika." },
  { "en": "Apa Itu Greenship Certification?", "id": "Standar Sertifikasi Bangunan Hijau Indonesia." },
  { "en": "Apa Itu ISO 50001 Standard?", "id": "Standar Internasional Sistem Manajemen Energi." },
  { "en": "Apa Itu Mode 1 Charging EV?", "id": "Cas Mobil Listrik Tanpa Proteksi." },
  { "en": "Apa Itu Mode 2 Charging EV?", "id": "Cas Mobil Kabel Ada Proteksi." },
  { "en": "Apa Itu Mode 3 Charging EV?", "id": "Cas Mobil Menggunakan Wallbox Khusus." },
  { "en": "Apa Itu Mode 4 Charging EV?", "id": "Cas Mobil Menggunakan DC Fast Charger." },
  { "en": "Apa Itu Connector Type 1 EV?", "id": "Konektor Cas Standar Amerika Jepang." },
  { "en": "Apa Itu Connector Type 2 EV?", "id": "Konektor Cas Standar Eropa Indonesia." },
  { "en": "Apa Itu Connector CCS 2?", "id": "Konektor Combo DC Standar Eropa." },
  { "en": "Apa Itu Connector CHAdeMO?", "id": "Konektor DC Fast Charging Jepang." },
  { "en": "Apa Itu Connector GB/T?", "id": "Konektor Cas Standar Negara China." },
  { "en": "Apa Itu Vehicle To Load? (V2L)", "id": "Mobil Listrik Suplai Alat Elektronik." },
  { "en": "Apa Itu Vehicle To Home? (V2H)", "id": "Mobil Listrik Suplai Listrik Rumah." },
  { "en": "Apa Itu Range Anxiety?", "id": "Kecemasan Kehabisan Baterai Di Jalan." },
  { "en": "Apa Itu Regenerative Braking Level?", "id": "Tingkat Kekuatan Pengereman Pengisian Baterai." },
  { "en": "Apa Itu One Pedal Driving?", "id": "Menyetir Hanya Menggunakan Pedal Gas." },
  { "en": "Apa Itu Battery Thermal Management?", "id": "Sistem Pengatur Suhu Baterai Mobil." },
  { "en": "Apa Itu Liquid Cooled Battery?", "id": "Pendingin Cair Sirkulasi Plat Baterai." },
  { "en": "Apa Itu Air Cooled Battery?", "id": "Pendingin Udara Kipas Baterai EV." },
  { "en": "Apa Itu Solid State Battery?", "id": "Baterai Elektrolit Padat Lebih Aman." },
  { "en": "Apa Itu Sodium Ion Battery?", "id": "Baterai Bahan Garam Natrium Murah." },
  { "en": "Apa Itu Hydrogen Fuel Cell EV?", "id": "Mobil Listrik Bahan Bakar Hidrogen." },
  { "en": "Apa Itu Well To Wheel?", "id": "Efisiensi Total Energi Hingga Roda." },
  { "en": "Apa Itu Tank To Wheel?", "id": "Efisiensi Energi Kendaraan Itu Sendiri." },
  { "en": "Apa Itu Homopolar Generator?", "id": "Generator DC Kutub Seragam Unipolar." },
  { "en": "Apa Itu Magnetohydrodynamic Generator?", "id": "Pembangkit Listrik Fluida Plasma Magnet." },
  { "en": "Apa Itu Piezoelectric Sensor?", "id": "Sensor Tekanan Hasilkan Muatan Listrik." },
  { "en": "Apa Itu Strain Gauge Sensor?", "id": "Sensor Regangan Berubah Nilai Tahanan." },
  { "en": "Apa Itu Wheatstone Bridge Circuit?", "id": "Rangkaian Jembatan Ukur Tahanan Presisi." },
  { "en": "Apa Itu Quarter Bridge Strain?", "id": "Satu Strain Gauge Dalam Jembatan." },
  { "en": "Apa Itu Half Bridge Strain?", "id": "Dua Strain Gauge Dalam Jembatan." },
  { "en": "Apa Itu Full Bridge Strain?", "id": "Empat Strain Gauge Dalam Jembatan." },
  { "en": "Apa Itu Load Cell Timbangan?", "id": "Sensor Berat Berbasis Strain Gauge." },
  { "en": "Apa Itu LVDT Position Sensor?", "id": "Trafo Diferensial Ukur Posisi Linier." },
  { "en": "Apa Itu RVDT Position Sensor?", "id": "Trafo Diferensial Ukur Posisi Sudut." },
  { "en": "Apa Itu Synchro Resolver?", "id": "Sensor Posisi Sudut Motor Servo." },
  { "en": "Apa Itu Optical Encoder?", "id": "Sensor Posisi Piringan Lubang Cahaya." },
  { "en": "Apa Itu Absolute Encoder?", "id": "Encoder Posisi Kode Biner Unik." },
  { "en": "Apa Itu Incremental Encoder?", "id": "Encoder Hitung Pulsa Perubahan Posisi." },
  { "en": "Apa Itu Gray Code Encoder?", "id": "Kode Biner Berubah Satu Bit." },
  { "en": "Apa Itu Quadrature Output?", "id": "Dua Sinyal Beda Fasa 90 Derajat." },
  { "en": "Apa Itu Hall Effect Sensor?", "id": "Sensor Deteksi Medan Magnet Semikonduktor." },
  { "en": "Apa Itu Reed Switch Sensor?", "id": "Saklar Kontak Kaca Deteksi Magnet." },
  { "en": "Apa Itu Inductive Proximity Sensor?", "id": "Sensor Deteksi Logam Tanpa Sentuh." },
  { "en": "Apa Itu Capacitive Proximity Sensor?", "id": "Sensor Deteksi Benda Non Logam." },
  { "en": "Apa Itu Ultrasonic Level Sensor?", "id": "Sensor Level Cairan Pantulan Suara." },
  { "en": "Apa Itu Radar Level Transmitter?", "id": "Sensor Level Cairan Gelombang Mikro." },
  { "en": "Apa Itu Guided Wave Radar?", "id": "Radar Level Menggunakan Kabel Pandu." },
  { "en": "Apa Itu Hydrostatic Level Sensor?", "id": "Sensor Level Berdasarkan Tekanan Air." },
  { "en": "Apa Itu Differential Pressure Transmitter?", "id": "Alat Ukur Beda Tekanan Pipa." },
  { "en": "Apa Itu Orifice Plate Flowmeter?", "id": "Pengukur Aliran Beda Tekanan Plat." },
  { "en": "Apa Itu Venturi Tube Flowmeter?", "id": "Pengukur Aliran Pipa Bentuk Corong." },
  { "en": "Apa Itu Magnetic Flowmeter?", "id": "Pengukur Aliran Cairan Konduktif Magnet." },
  { "en": "Apa Itu Turbine Flowmeter?", "id": "Pengukur Aliran Putaran Baling-Baling." },
  { "en": "Apa Itu Vortex Flowmeter?", "id": "Pengukur Aliran Pusaran Fluida Halangan." },
  { "en": "Apa Itu Coriolis Mass Flowmeter?", "id": "Pengukur Massa Aliran Pipa Getar." },
  { "en": "Apa Itu Thermal Mass Flowmeter?", "id": "Pengukur Aliran Gas Transfer Panas." },
  { "en": "Apa Itu Ultrasonic Flowmeter Clamp-On?", "id": "Pengukur Aliran Tempel Luar Pipa." },
  { "en": "Apa Itu Transit Time Ultrasonic?", "id": "Metode Ukur Waktu Tempuh Suara." },
  { "en": "Apa Itu Doppler Ultrasonic Flowmeter?", "id": "Metode Ukur Geseran Frekuensi Suara." },
  { "en": "Apa Itu Rotameter?", "id": "Pengukur Aliran Tabung Kaca Pelampung." },
  { "en": "Apa Itu Pitot Tube?", "id": "Tabung Ukur Kecepatan Aliran Udara." },
  { "en": "Apa Itu Thermocouple Type K?", "id": "Sensor Suhu Nikel-Kromium Paling Umum." },
  { "en": "Apa Itu Thermocouple Type J?", "id": "Sensor Suhu Besi-Konstantan Rentang Rendah." },
  { "en": "Apa Itu Thermocouple Type S?", "id": "Sensor Suhu Platinum Suhu Tinggi." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Koreksi Suhu Sambungan Referensi Termokopel." },
  { "en": "Apa Itu RTD Pt100?", "id": "Sensor Suhu Platinum 100 Ohm." },
  { "en": "Apa Itu 3-Wire RTD Connection?", "id": "Koneksi Kabel Kompensasi Tahanan Kabel." },
  { "en": "Apa Itu 4-Wire RTD Connection?", "id": "Koneksi Paling Akurat Eliminasi Kabel." },
  { "en": "Apa Itu Thermistor NTC?", "id": "Sensor Suhu Tahanan Turun Panas." },
  { "en": "Apa Itu Thermistor PTC?", "id": "Sensor Suhu Tahanan Naik Panas." },
  { "en": "Apa Itu Infrared Pyrometer?", "id": "Termometer Tembak Tanpa Kontak Fisik." },
  { "en": "Apa Itu Bimetal Thermometer?", "id": "Termometer Mekanis Dua Logam Lengkung." },
  { "en": "Apa Itu Signal Conditioner?", "id": "Pengolah Sinyal Sensor Agar Standar." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Sinyal Diferensial Presisi Tinggi." },
  { "en": "Apa Itu 4-20mA Current Loop?", "id": "Standar Sinyal Analog Industri Kebal Noise." },
  { "en": "Apa Itu HART Protocol?", "id": "Protokol Data Digital Tumpangan Analog." },
  { "en": "Apa Itu Fieldbus Foundation?", "id": "Jaringan Komunikasi Digital Instrumen Lapangan." },
  { "en": "Apa Itu Profibus PA?", "id": "Profibus Khusus Otomasi Proses Instrumen." },
  { "en": "Apa Itu WirelessHART?", "id": "Jaringan Mesh Nirkabel Instrumen Industri." },
  { "en": "Apa Itu ISA100.11a?", "id": "Standar Otomasi Nirkabel Industri ISA." },
  { "en": "Apa Itu PID Controller?", "id": "Pengendali Proporsional Integral Derivatif." },
  { "en": "Apa Itu Tuning PID?", "id": "Penyetelan Parameter Respon Kontroler PID." },
  { "en": "Apa Itu Setpoint?", "id": "Nilai Target Yang Diinginkan Proses." },
  { "en": "Apa Itu Manipulated Variable?", "id": "Sinyal Output Kontroler Ke Aktuator." },
  { "en": "Apa Itu Proportional Band?", "id": "Kebalikan Dari Gain Kontroler Proporsional." },
  { "en": "Apa Itu Integral Action time?", "id": "Waktu Koreksi Error Masa Lalu." },
  { "en": "Apa Itu Derivative Action time?", "id": "Waktu Prediksi Error Masa Depan." },
  { "en": "Apa Itu Overshoot?", "id": "Lonjakan Nilai Melebihi Setpoint Sesaat." },
  { "en": "Apa Itu Steady State Error?", "id": "Selisih Nilai Akhir Dan Setpoint." },
  { "en": "Apa Itu Control Valve?", "id": "Katup Pengatur Aliran Fluida Otomatis." },
  { "en": "Apa Itu Valve Positioner?", "id": "Alat Pengatur Posisi Bukaan Katup." },
  { "en": "Apa Itu I/P Converter?", "id": "Pengubah Arus Listrik Ke Tekanan." },
  { "en": "Apa Itu Solenoid Valve?", "id": "Katup Buka Tutup Elektromagnetik." },
  { "en": "Apa Itu Pneumatic Actuator?", "id": "Penggerak Katup Tenaga Angin Tekan." },
  { "en": "Apa Itu Electric Actuator?", "id": "Penggerak Katup Tenaga Motor Listrik." },
  { "en": "Apa Itu Hydraulic Actuator?", "id": "Penggerak Katup Tenaga Cairan Oli." },
  { "en": "Apa Itu Limit Switch Box?", "id": "Kotak Indikator Posisi Buka Tutup." },
  { "en": "Apa Itu Butterfly Valve?", "id": "Katup Piringan Putar Sederhana." },
  { "en": "Apa Itu Ball Valve?", "id": "Katup Bola Berlubang Putar." },
  { "en": "Apa Itu Gangguan Tiga Fasa Simetris?", "id": "Gangguan Melibatkan Ketiga Fasa Sama." },
  { "en": "Apa Itu Gangguan Satu Fasa Ke Tanah?", "id": "Gangguan Satu Fasa Langsung Ke Tanah." },
  { "en": "Apa Itu Gangguan Dua Fasa Ke Tanah?", "id": "Gangguan Dua Fasa Langsung Ke Tanah." },
  { "en": "Apa Itu Gangguan Dua Fasa Hubung Singkat?", "id": "Gangguan Antara Dua Fasa Tanpa Tanah." },
  { "en": "Apa Itu Urutan Positif Impedansi?", "id": "Impedansi Sistem Kondisi Operasi Normal." },
  { "en": "Apa Itu Urutan Negatif Impedansi?", "id": "Impedansi Sistem Aliran Arus Terbalik." },
  { "en": "Apa Itu Urutan Nol Impedansi?", "id": "Impedansi Sistem Aliran Arus Ke Tanah." },
  { "en": "Bagaimana Menghitung Arus Tiga Fasa?", "id": "Gunakan Urutan Positif Impedansi Saja." },
  { "en": "Bagaimana Menghitung Arus Satu Fasa Ke Tanah?", "id": "Tiga Urutan Impedansi Dihubungkan Seri." },
  { "en": "Bagaimana Menghitung Arus Dua Fasa Hubung Singkat?", "id": "Positif Negatif Impedansi Dihubungkan Paralel." },
  { "en": "Bagaimana Menghitung Arus Dua Fasa Ke Tanah?", "id": "Nol Urutan Diparalel Seri Urutan Lain." },
  { "en": "Apa Itu HVDC Bipole Configuration?", "id": "Dua Kutub Positif Negatif Dengan Tanah." },
  { "en": "Apa Itu HVDC Monopole Configuration?", "id": "Satu Kutub Dengan Konduktor Balik Tanah." },
  { "en": "Apa Itu Static Synchronous Compensator? (STATCOM)", "id": "Kompensator Daya Reaktif Berbasis Inverter." },
  { "en": "Apa Itu Thyristor Controlled Series Capacitor? (TCSC)", "id": "Kapasitor Seri Dikontrol Thyristor Cepat." },
  { "en": "Apa Itu Thyristor Controlled Reactor? (TCR)", "id": "Reaktor Paralel Dikontrol Thyristor Cepat." },
  { "en": "Apa Itu Static Var Compensator? (SVC)", "id": "Kompensator Reaktif Menggunakan Thyristor." },
  { "en": "Apa Itu UPFC? (Unified Power Flow Controller)", "id": "Pengontrol Daya Menggunakan Seri Paralel." },
  { "en": "Apa Itu Thyristor Switched Capacitor? (TSC)", "id": "Kapasitor Paralel Disaklar Oleh Thyristor." },
  { "en": "Apa Itu Air Filter Regulator?", "id": "Filter Dan Pengatur Tekanan Udara." },
  { "en": "Apa Itu Solenoid Valve Fail Safe?", "id": "Katup Kembali Aman Saat Listrik Mati." },
  { "en": "Apa Itu Valve Stem Packing?", "id": "Penyekat Mencegah Kebocoran Fluida Katup." },
  { "en": "Apa Itu Valve Gland Packing?", "id": "Penyekat Batang Katup Dari Luar." },
  { "en": "Apa Itu Valve Bonnet?", "id": "Penutup Atas Katup (Rumah Packing)." },
  { "en": "Apa Itu Valve Body?", "id": "Badan Utama Katup Dilalui Fluida." },
  { "en": "Apa Itu Valve Trim?", "id": "Komponen Pengontrol Aliran Dalam Katup." },
  { "en": "Apa Itu Valve Plug?", "id": "Komponen Bergerak Mengubah Aliran." },
  { "en": "Apa Itu Control Valve Cavitation?", "id": "Pembentukan Gelembung Uap Dalam Cairan." },
  { "en": "Apa Itu Control Valve Flashing?", "id": "Sebagian Cairan Berubah Menjadi Uap." },
  { "en": "Apa Itu Dielectric Loss Factor?", "id": "Faktor Rugi Daya Listrik Isolasi." },
  { "en": "Apa Itu Tip-Up Test Insulation?", "id": "Uji Kenaikan Rugi Dielektrik Tegangan." },
  { "en": "Apa Itu Power Factor Tip-Up Test?", "id": "Nama Lain Dari Tip-Up Test." },
  { "en": "Apa Itu Dielectric Spectroscopy?", "id": "Analisis Dielektrik Isolasi Domain Frekuensi." },
  { "en": "Apa Itu Dissipation Factor?", "id": "Ukuran Kerugian Energi Dalam Isolasi." },
  { "en": "Apa Itu Capacitance Measurement?", "id": "Mengukur Nilai Kapasitansi Isolasi." },
  { "en": "Apa Itu Gas Pressure Monitor SF6?", "id": "Alat Pantau Tekanan Gas SF6." },
  { "en": "Apa Itu Gas Density Monitor SF6?", "id": "Alat Pantau Kerapatan Gas SF6." },
  { "en": "Apa Itu SF6 Gas Recovery Unit?", "id": "Alat Mendaur Ulang Gas SF6 Bekas." },
  { "en": "Apa Itu SF6 Gas Analyzer?", "id": "Alat Uji Kemurnian Gas SF6." },
  { "en": "Apa Itu Motor Winding Surge Test?", "id": "Uji Tahan Tegangan Lonjakan Lilitan." },
  { "en": "Apa Itu Surge Comparison Test?", "id": "Uji Banding Bentuk Gelombang Lilitan." },
  { "en": "Apa Itu DC Overpotential Test?", "id": "Uji Tegangan DC Tinggi Untuk Isolasi." },
  { "en": "Apa Itu Winding Insulation Resistance?", "id": "Tahanan Isolasi Antara Lilitan Dan Ground." },
  { "en": "Apa Itu Phase-to-Phase Resistance?", "id": "Tahanan Antara Dua Lilitan Fasa." },
  { "en": "Apa Itu Winding Resistance Correction?", "id": "Koreksi Tahanan Ke Suhu Standar." },
  { "en": "Apa Itu Motor Thermal Overload Curve?", "id": "Batas Waktu Tahan Panas Motor." },
  { "en": "Apa Itu Maximize Inrush Current?", "id": "Arus Puncak Maksimum Saat Start." },
  { "en": "Apa Itu Motor Acceleration Time?", "id": "Waktu Motor Mencapai Kecepatan Penuh." },
  { "en": "Apa Itu Motor Safe Start Capability?", "id": "Kemampuan Motor Dinyalakan Dengan Aman." },
  { "en": "Apa Itu Boiler Water Level Gauge?", "id": "Alat Ukur Ketinggian Air Ketel." },
  { "en": "Apa Itu Steam Pressure Gauge?", "id": "Alat Ukur Tekanan Uap Ketel." },
  { "en": "Apa Itu Soot Blower System?", "id": "Pembersih Jelaga Pipa Pemanas Boiler." },
  { "en": "Apa Itu Superheater Safety Valve?", "id": "Katup Pengaman Tekanan Uap Lebih." },
  { "en": "Apa Itu Boiler Drum?", "id": "Bejana Pemisah Air Dan Uap Boiler." },
  { "en": "Apa Itu Turbine Bypass Valve?", "id": "Katup Pintas Uap Ke Kondensor." },
  { "en": "Apa Itu Condensate Extraction Pump?", "id": "Pompa Air Dari Kondensor Ke Deaerator." },
  { "en": "Apa Itu Cooling Water Intake?", "id": "Saluran Masuk Air Pendingin Kondensor." },
  { "en": "Apa Itu Circulating Water Pump?", "id": "Pompa Sirkulasi Air Pendingin Kondensor." },
  { "en": "Apa Itu Demineralizer Plant?", "id": "Instalasi Pemurnian Air Umpan Boiler." },
  { "en": "Apa Itu Chlorination Plant?", "id": "Instalasi Pembasmi Kuman Air Pendingin." },
  { "en": "Apa Itu Flue Gas Stack?", "id": "Cerobong Pembuangan Gas Sisa Pembakaran." },
  { "en": "Apa Itu Continuous Emission Monitoring?", "id": "Pantau Emisi Gas Buang Terus Menerus." },
  { "en": "Apa Itu Opacity Monitor?", "id": "Monitor Kepadatan Asap Cerobong Gas." },
  { "en": "Apa Itu Ammonia Injection Grid?", "id": "Penyuntik Amonia Sistem Pengurang NOx." },
  { "en": "Apa Itu Reactor Power Control?", "id": "Pengaturan Daya Reaktor Nuklir." },
  { "en": "Apa Itu Control Rod Drive?", "id": "Penggerak Batang Kendali Reaktor Nuklir." },
  { "en": "Apa Itu Containment Building?", "id": "Bangunan Perlindungan Reaktor Nuklir." },
  { "en": "Apa Itu Spent Fuel Pool?", "id": "Kolam Penyimpanan Bahan Bakar Bekas." },
  { "en": "Apa Itu Emergency Diesel Generator?", "id": "Genset Darurat Daya Reaktor Nuklir." },
  { "en": "Apa Itu Geothermal Brine Fluid?", "id": "Cairan Air Panas Asin Geotermal." },
  { "en": "Apa Itu Geothermal Flash Tank?", "id": "Tangki Pemisah Uap Dari Air Panas." },
  { "en": "Apa Itu Geothermal Injection Well?", "id": "Sumur Injeksi Air Bekas Pakai." },
  { "en": "Apa Itu Wind Turbine Pitch Motor?", "id": "Motor Penggerak Sudut Bilah Turbin." },
  { "en": "Apa Itu Wind Turbine Yaw Drive?", "id": "Penggerak Turbin Menghadap Arah Angin." },
  { "en": "Apa Itu Wind Turbine Anemometer?", "id": "Alat Ukur Kecepatan Angin Turbin." },
  { "en": "Apa Itu Solar PV Cell Efficiency?", "id": "Efisiensi Konversi Cahaya Menjadi Listrik." },
  { "en": "Apa Itu Fill Factor Panel Surya?", "id": "Faktor Kualitas Kurva I-V Panel." },
  { "en": "Apa Itu Solar Inverter Transformerless?", "id": "Inverter Tanpa Trafo Isolasi Output." },
  { "en": "Apa Itu String Inverter?", "id": "Inverter Untuk Rangkaian Panel Seri." },
  { "en": "Apa Itu Central Inverter?", "id": "Inverter Kapasitas Besar Satu Titik." },
  { "en": "Apa Itu Battery Cell Voltage?", "id": "Tegangan Satu Sel Baterai Tunggal." },
  { "en": "Apa Itu Battery State Of Health?", "id": "Kondisi Kesehatan Baterai Terhadap Baru." },
  { "en": "Apa Itu Battery Cycle Life?", "id": "Jumlah Siklus Pengisian Penuh Hingga Rusak." },
  { "en": "Apa Itu Battery Management System?", "id": "Sistem Pengelola Kesehatan Dan Keamanan Baterai." },
  { "en": "Apa Itu Electric Vehicle Range?", "id": "Jarak Tempuh Maksimum Sekali Isi." },
  { "en": "Apa Itu AC Charging Level 1?", "id": "Pengisian AC Paling Lambat (120 Volt)." },
  { "en": "Apa Itu AC Charging Level 2?", "id": "Pengisian AC Menengah (240 Volt)." },
  { "en": "Apa Itu DC Fast Charging?", "id": "Pengisian DC Daya Sangat Besar." },
  { "en": "Apa Itu Vehicle-to-Grid Technology?", "id": "Baterai Mobil Memasok Listrik Kembali." },
  { "en": "Apa Itu Trailing Edge Dimmer?", "id": "Potongan Fase Akhir Gelombang Sinus." },
  { "en": "Apa Itu Leading Edge Dimmer?", "id": "Potongan Fase Awal Gelombang Sinus." },
  { "en": "Apa Itu Luminous Efficacy?", "id": "Lumen Yang Dihasilkan Per Watt." },
  { "en": "Apa Itu Color Rendering Index?", "id": "Kemampuan Lampu Menampilkan Warna Asli." },
  { "en": "Apa Itu Correlated Color Temperature?", "id": "Suhu Warna Cahaya Dalam Kelvin." },
  { "en": "Apa Itu Lighting Control Protocol DALI?", "id": "Standar Kontrol Pencahayaan Digital Terpusat." },
  { "en": "Apa Itu Emergency Lighting Circuit?", "id": "Sirkuit Khusus Lampu Darurat Baterai." },
  { "en": "Apa Itu Daylight Harvesting System?", "id": "Sistem Redup Lampu Jika Ada Cahaya Alami." },
  { "en": "Apa Itu Lux Level Standard?", "id": "Intensitas Pencahayaan Minimum Ruangan." },
  { "en": "Apa Itu Per Unit System?", "id": "Satuan Relatif Untuk Analisis Sistem." },
  { "en": "Apa Itu Base MVA?", "id": "Nilai Daya Dasar Standar Sistem." },
  { "en": "Apa Itu Base Voltage?", "id": "Nilai Tegangan Dasar Standar Sistem." },
  { "en": "Apa Itu Impedance Diagram?", "id": "Diagram Rangkaian Representasi Sistem." },
  { "en": "Apa Itu Zero Sequence Impedance?", "id": "Impedansi Untuk Gangguan Tanah." },
  { "en": "Apa Itu Short Circuit Capacity?", "id": "Daya Hubung Singkat Busbar." },
  { "en": "Apa Itu Critical Clearing Angle?", "id": "Sudut Maksimum Trip Agar Stabil." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis Stabilitas Transient Generator." },
  { "en": "Apa Itu Zone Selective Interlocking?", "id": "Koordinasi Logika Cepat Antar Breaker." },
  { "en": "Apa Itu Breaker Failure Logic?", "id": "Logika Trip Breaker Cadangan." },
  { "en": "Apa Itu VFD Regenerative Drive?", "id": "Mengembalikan Energi Rem Ke Jaringan." },
  { "en": "Apa Itu Dynamic Braking Unit?", "id": "Modul Pembuang Energi Rem Ke Resistor." },
  { "en": "Apa Itu Anti-Plugging Relay?", "id": "Proteksi Mencegah Pembalikan Arah Cepat." },
  { "en": "Apa Itu Insulator Shed Profile?", "id": "Bentuk Sirip Isolator Tahan Polusi." },
  { "en": "Apa Itu Dry Power Frequency Test?", "id": "Uji Tahan Tegangan Kondisi Kering." },
  { "en": "Apa Itu Switching Impulse Voltage?", "id": "Tegangan Surja Akibat Operasi Saklar." },
  { "en": "Apa Itu Lightning Impulse Voltage?", "id": "Tegangan Surja Akibat Sambaran Petir." },
  { "en": "Apa Itu Corona Inception Voltage?", "id": "Tegangan Awal Munculnya Pelepasan Korona." },
  { "en": "Apa Itu Condenser Vacuum Pump?", "id": "Pompa Menjaga Ruang Hampa Kondensor." },
  { "en": "Apa Itu Deaerator Function?", "id": "Menghilangkan Oksigen Dari Air Umpan." },
  { "en": "Apa Itu Boiler Drum Function?", "id": "Pemisah Air Dan Uap Di Ketel." },
  { "en": "Apa Itu Feed Water Heater?", "id": "Pemanas Air Umpan Sebelum Ke Boiler." },
  { "en": "Apa Itu Dehydrating Breather?", "id": "Penyerap Kelembaban Udara Trafo." },
  { "en": "Apa Itu OLTC Diverter Switch?", "id": "Saklar Pemindah Arus Tap Changer." },
  { "en": "Apa Itu Trafo Auto Konfigurasi?", "id": "Satu Lilitan Bersama Primer Sekunder." },
  { "en": "Apa Itu Trafo Scott Connection?", "id": "Konversi Tiga Fasa Ke Dua Fasa." },
  { "en": "Apa Itu Trafo Open Delta?", "id": "Dua Trafo Konversi Tiga Fasa." },
  { "en": "Apa Itu Bushing Solid Type?", "id": "Bushing Isolasi Padat Tanpa Minyak." },
  { "en": "Apa Itu Bushing Oil Filled?", "id": "Bushing Isolasi Minyak Dan Kertas." },
  { "en": "Apa Itu Bushing RIP Type?", "id": "Isolasi Kertas Diresapi Resin Epoksi." },
  { "en": "Apa Itu Bushing RIS Type?", "id": "Isolasi Sintetis Diresapi Resin." },
  { "en": "Apa Itu Cable Sheath Test?", "id": "Uji Isolasi Selubung Kabel Tanah." },
  { "en": "Apa Itu Cable Fault Pre-Location?", "id": "Perkiraan Awal Jarak Gangguan Kabel." },
  { "en": "Apa Itu Cable Fault Pinpointing?", "id": "Menentukan Titik Tepat Gangguan Kabel." },
  { "en": "Apa Itu Time Domain Reflectometer? (TDR)", "id": "Radar Kabel Cari Jarak Gangguan." },
  { "en": "Apa Itu Impulse Current Method?", "id": "Metode Hentakan Arus Cari Gangguan." },
  { "en": "Apa Itu Cable Thumper?", "id": "Alat Pemukul Tegangan Cari Gangguan." },
  { "en": "Apa Itu Capacitor Discharge Unit?", "id": "Unit Penghasil Pulsa Tegangan Kabel." },
  { "en": "Apa Itu Surge Wave Generator?", "id": "Nama Lain Capacitor Discharge Unit." },
  { "en": "Apa Itu Cable Splicing Joint?", "id": "Sambungan Permanen Antara Dua Kabel." },
  { "en": "Apa Itu Cable Termination Kit?", "id": "Aksesoris Pemasangan Ujung Kabel." },
  { "en": "Apa Itu Stress Control Cone?", "id": "Kerucut Pengendali Medan Listrik Kabel." },
  { "en": "Apa Itu Cable Screen Earth?", "id": "Pentanahan Pelindung Kabel Tanah." },
  { "en": "Apa Itu Cross Bonding Sheath?", "id": "Menghilangkan Arus Sirkulasi Pelindung Kabel." },
  { "en": "Apa Itu SVL Protection?", "id": "Arrester Pelindung Tegangan Lebih Sheath." },
  { "en": "Apa Itu Link Box Function?", "id": "Kotak Sambungan Pentanahan Sheath Kabel." },
  { "en": "Apa Itu VLF Tester?", "id": "Alat Uji Kabel Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Tan Delta Measurement?", "id": "Ukur Rugi Dielektrik Isolasi Kabel." },
  { "en": "Apa Itu Dielectric Response Analysis?", "id": "Analisis Respon Isolasi Terhadap Tegangan." },
  { "en": "Apa Itu Partial Discharge Mapping?", "id": "Peta Lokasi PD Sepanjang Kabel." },
  { "en": "Apa Itu Resonant Test System?", "id": "Sistem Uji Kabel Frekuensi Resonansi." },
  { "en": "Apa Itu Synchronous Condenser?", "id": "Motor Sinkron Pengatur Daya Reaktif Grid." },
  { "en": "Apa Itu Static Var Compensator? (SVC)", "id": "Kompensator Reaktif Berbasis Thyristor." },
  { "en": "Apa Itu Thyristor Controlled Reactor?", "id": "Reaktor Variabel Dikendalikan Thyristor." },
  { "en": "Apa Itu Thyristor Switched Capacitor?", "id": "Kapasitor Disaklar Oleh Thyristor." },
  { "en": "Apa Itu Static Synchronous Compensator? (STATCOM)", "id": "Kompensator Reaktif Berbasis Inverter." },
  { "en": "Apa Kelebihan STATCOM?", "id": "Respon Sangat Cepat Dibanding SVC." },
  { "en": "Apa Itu Unified Power Flow Controller? (UPFC)", "id": "Pengontrol Daya Paling Fleksibel." },
  { "en": "Apa Itu Flexible AC Transmission Systems? (FACTS)", "id": "Peralatan Mengontrol Parameter Transmisi AC." },
  { "en": "Apa Itu HVDC Converter Station?", "id": "Stasiun Pengubah AC Ke DC Atau Sebaliknya." },
  { "en": "Apa Itu Bipolar HVDC System?", "id": "Sistem HVDC Dua Kutub Plus Minus." },
  { "en": "Apa Itu Monopolar HVDC System?", "id": "Sistem HVDC Satu Kutub Dengan Tanah." },
  { "en": "Apa Itu Filter Harmonisa HVDC?", "id": "Penyaring Harmonisa Pada Kedua Sisi." },
  { "en": "Apa Itu Smoothing Reactor HVDC?", "id": "Reaktor Perata Arus DC Transmisi." },
  { "en": "Apa Itu Commutation Failure HVDC?", "id": "Kegagalan Thyristor Padam Saat Beroperasi." },
  { "en": "Apa Itu Voltage Source Converter? (VSC)", "id": "Konverter Sumber Tegangan IGBT/IGCT." },
  { "en": "Apa Itu Line Commutated Converter? (LCC)", "id": "Konverter Sumber Arus Berbasis Thyristor." },
  { "en": "Apa Keuntungan VSC HVDC?", "id": "Kontrol Independen Daya Aktif Reaktif." },
  { "en": "Apa Itu Generator Winding Factor?", "id": "Faktor Kisar Kali Faktor Distribusi." },
  { "en": "Apa Itu Generator Capability Curve?", "id": "Batas Operasi Aktif Reaktif Generator." },
  { "en": "Apa Itu Governor Droop Setting?", "id": "Persentase Turun Speed Terhadap Beban." },
  { "en": "Apa Itu AVR Voltage Limiter?", "id": "Pembatas Tegangan Maksimum Output AVR." },
  { "en": "Apa Itu PSS Damping Control?", "id": "Sinyal Tambahan Peredam Osilasi Daya." },
  { "en": "Apa Itu Inter-Area Oscillation?", "id": "Osilasi Daya Antar Wilayah Sistem Tenaga." },
  { "en": "Apa Itu Critical Fault Clearing Time?", "id": "Waktu Maksimum Hapus Gangguan Stabil." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis Analisis Stabilitas Generator." },
  { "en": "Apa Itu Torque Angle Characteristic?", "id": "Kurva Daya Terhadap Sudut Daya Generator." },
  { "en": "Apa Itu Generator Subtransient Reactance?", "id": "Impedansi Saat Awal Hubung Singkat." },
  { "en": "Apa Itu Generator Transient Reactance?", "id": "Impedansi Beberapa Siklus Setelah Gangguan." },
  { "en": "Apa Itu Generator Synchronous Reactance?", "id": "Impedansi Generator Kondisi Steady State." },
  { "en": "Apa Itu Generator Zero Sequence Reactance?", "id": "Impedansi Generator Saat Gangguan Tanah." },
  { "en": "Apa Itu Generator Negative Sequence Reactance?", "id": "Impedansi Generator Saat Arus Terbalik." },
  { "en": "Apa Itu Generator Inertia Constant?", "id": "Energi Kinetik Rotor Per MVA." },
  { "en": "Apa Itu Governor Speed Droop?", "id": "Penurunan Putaran Proporsional Kenaikan Beban." },
  { "en": "Apa Itu AVR Excitation Limiter?", "id": "Pembatas Arus Medan Generator." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Alat Peredam Osilasi Daya Sistem." },
  { "en": "Apa Itu Black System?", "id": "Kondisi Padam Total Sistem Tenaga Listrik." },
  { "en": "Apa Itu House Load Operation?", "id": "Pembangkit Hanya Suplai Pemakaian Sendiri." },
  { "en": "Apa Itu Load Restoration?", "id": "Penormalan Beban Setelah Gangguan Teratasi." },
  { "en": "Apa Itu Cold Load Pickup?", "id": "Arus Tinggi Saat Beban Lama Mati." },
  { "en": "Apa Itu Inrush Current Trafo?", "id": "Arus Magnetisasi Awal Saat Trafo Nyala." },
  { "en": "Apa Itu Sympathetic Inrush?", "id": "Arus Inrush Akibat Trafo Tetangga Nyala." },
  { "en": "Apa Itu Harmonic Restraint Relay?", "id": "Mencegah Trip Akibat Harmonisa Arus Inrush." },
  { "en": "Apa Itu Fifth Harmonic Blocking?", "id": "Mencegah Trip Akibat Overfluxing Trafo." },
  { "en": "Apa Itu Buchholz Relay Surge?", "id": "Operasi Relay Akibat Aliran Minyak Cepat." },
  { "en": "Apa Itu Oil Level Low Alarm?", "id": "Peringatan Minyak Trafo Kurang." },
  { "en": "Apa Itu Winding Temperature Trip?", "id": "Pemutusan Beban Akibat Lilitan Panas." },
  { "en": "Apa Itu Oil Temperature Trip?", "id": "Pemutusan Beban Akibat Minyak Panas." },
  { "en": "Apa Itu Pressure Relief Operation?", "id": "Pembuangan Tekanan Lebih Tangki Trafo." },
  { "en": "Apa Itu Cooler Control Cabinet?", "id": "Panel Kontrol Kipas Dan Pompa Trafo." },
  { "en": "Apa Itu OLTC Motor Drive?", "id": "Motor Penggerak Tap Changer Trafo." },
  { "en": "Apa Itu Step Voltage Regulator?", "id": "Trafo Pengatur Tegangan Saluran Distribusi." },
  { "en": "Apa Itu Line Drop Compensator?", "id": "Kompensasi Tegangan Jatuh Ujung Saluran." },
  { "en": "Apa Itu Capacitor Bank Switching?", "id": "Proses Masuk Keluar Kapasitor Jaringan." },
  { "en": "Apa Itu Back-to-Back Switching?", "id": "Switching Kapasitor Saat Bank Lain Aktif." },
  { "en": "Apa Itu Inrush Current Reactor?", "id": "Pembatas Arus Masuk Kapasitor Bank." },
  { "en": "Apa Itu Discharge Coil Kapasitor?", "id": "Pembuang Muatan Sisa Kapasitor Bank." },
  { "en": "Apa Itu Detuned Reactor 7 Persen?", "id": "Reaktor Anti Resonansi Frekuensi 189Hz." },
  { "en": "Apa Itu Detuned Reactor 14 Persen?", "id": "Reaktor Anti Resonansi Frekuensi 134Hz." },
  { "en": "Apa Itu Power Factor Controller?", "id": "Alat Pengatur Langkah Kapasitor Otomatis." },
  { "en": "Apa Itu Target Power Factor?", "id": "Nilai Cos Phi Yang Diinginkan." },
  { "en": "Apa Itu Ratio C/K Setting?", "id": "Sensitivitas Arus Kontroler Kapasitor." },
  { "en": "Apa Itu Hunting Kapasitor Bank?", "id": "Kapasitor Masuk Keluar Berulang Cepat." },
  { "en": "Apa Itu Voltage Harmonic Distortion?", "id": "Cacat Gelombang Tegangan Akibat Beban Non-Linier." },
  { "en": "Apa Itu Current Harmonic Distortion?", "id": "Cacat Gelombang Arus Akibat Beban Non-Linier." },
  { "en": "Apa Itu Point of Common Coupling?", "id": "Titik Sambung Pelanggan Dengan Jaringan PLN." },
  { "en": "Apa Itu Total Demand Distortion?", "id": "Harmonisa Arus Terhadap Beban Puncak." },
  { "en": "Apa Itu IEEE 519 Standard?", "id": "Standar Batas Harmonisa Sistem Tenaga." },
  { "en": "Apa Itu Passive Harmonic Filter?", "id": "Filter RLC Meredam Frekuensi Tertentu." },
  { "en": "Apa Itu Active Harmonic Filter?", "id": "Inverter Penyuntik Arus Anti Harmonisa." },
  { "en": "Apa Itu Hybrid Harmonic Filter?", "id": "Gabungan Filter Pasif Dan Aktif." },
  { "en": "Apa Itu Zero Sequence Filter?", "id": "Trafo Zigzag Pemblokir Harmonisa Ketiga." },
  { "en": "Apa Itu K-Factor Transformer Rating?", "id": "Kemampuan Trafo Menahan Panas Harmonisa." },
  { "en": "Apa Itu Skin Effect Harmonic?", "id": "Kenaikan Tahanan Kabel Frekuensi Tinggi." },
  { "en": "Apa Itu Derating Factor Trafo?", "id": "Penurunan Kapasitas Akibat Beban Harmonisa." },
  { "en": "Apa Itu Telephone Interference Factor?", "id": "Gangguan Harmonisa Pada Saluran Telepon." },
  { "en": "Apa Itu Electromagnetic Interference? (EMI)", "id": "Gangguan Gelombang Elektromagnetik Pada Peralatan." },
  { "en": "Apa Itu Electromagnetic Compatibility? (EMC)", "id": "Kemampuan Alat Bekerja Tanpa Mengganggu." },
  { "en": "Apa Itu Radiated Emission Test?", "id": "Uji Pancaran Gelombang Elektromagnetik Alat." },
  { "en": "Apa Itu Conducted Emission Test?", "id": "Uji Gangguan Sinyal Lewat Kabel." },
  { "en": "Apa Itu Electrostatic Discharge Test?", "id": "Uji Ketahanan Terhadap Listrik Statis." },
  { "en": "Apa Itu Electrical Fast Transient?", "id": "Uji Ketahanan Terhadap Denyut Cepat." },
  { "en": "Apa Itu Surge Immunity Test?", "id": "Uji Ketahanan Terhadap Lonjakan Tegangan." },
  { "en": "Apa Itu Voltage Dip Immunity?", "id": "Uji Ketahanan Terhadap Kedipan Tegangan." },
  { "en": "Apa Itu Digital Substation Architecture?", "id": "Struktur Gardu Induk Berbasis Komunikasi Digital." },
  { "en": "Apa Itu IEC 61850 Standard?", "id": "Standar Komunikasi Otomasi Gardu Induk." },
  { "en": "Apa Itu Station Bus Network?", "id": "Jaringan Komunikasi Antar IED Dan SCADA." },
  { "en": "Apa Itu Process Bus Network?", "id": "Jaringan Komunikasi Data Arus Tegangan." },
  { "en": "Apa Itu Merging Unit Device?", "id": "Pengubah Sinyal Analog Ke Digital Sampled." },
  { "en": "Apa Itu Intelligent Electronic Device?", "id": "Perangkat Proteksi Dan Kontrol Digital." },
  { "en": "Apa Itu GOOSE Communication Message?", "id": "Pesan Cepat Antar Relay Via Ethernet." },
  { "en": "Apa Itu Sampled Measured Value?", "id": "Data Digital Arus Tegangan Real-Time." },
  { "en": "Apa Itu Manufacturing Message Specification?", "id": "Protokol Komunikasi IED Ke HMI SCADA." },
  { "en": "Apa Itu Precision Time Protocol?", "id": "Sinkronisasi Waktu Akurat Lewat Ethernet." },
  { "en": "Apa Itu Redundant Network Topology?", "id": "Jaringan Ganda Mencegah Kegagalan Komunikasi." },
  { "en": "Apa Itu Parallel Redundancy Protocol?", "id": "Kirim Data Lewat Dua Jaringan Bersamaan." },
  { "en": "Apa Itu High-availability Seamless Redundancy?", "id": "Kirim Data Lewat Jaringan Cincin Ganda." },
  { "en": "Apa Itu Substation Configuration Language?", "id": "Bahasa Format File Konfigurasi Gardu Digital." },
  { "en": "Apa Itu IED Capability Description?", "id": "File Deskripsi Kemampuan Perangkat IED." },
  { "en": "Apa Itu Substation Configuration Description?", "id": "File Konfigurasi Seluruh Sistem Gardu." },
  { "en": "Apa Itu Configured IED Description?", "id": "File Konfigurasi Spesifik Satu IED." },
  { "en": "Apa Itu Cyber Security Substation?", "id": "Perlindungan Sistem Digital Dari Serangan Siber." },
  { "en": "Apa Itu Role Based Access?", "id": "Hak Akses Sesuai Peran Pengguna." },
  { "en": "Apa Itu Firewall Protection System?", "id": "Penyaring Lalu Lintas Data Jaringan." },
  { "en": "Apa Itu Intrusion Detection System?", "id": "Pendeteksi Aktivitas Mencurigakan Pada Jaringan." },
  { "en": "Apa Itu Security Logging Audit?", "id": "Pencatatan Jejak Akses Dan Perubahan." },
  { "en": "Apa Itu Fiber Optic Sensor?", "id": "Sensor Berbasis Cahaya Tahan Interferensi." },
  { "en": "Apa Itu Optical Voltage Transformer?", "id": "VT Menggunakan Efek Pockels Cahaya." },
  { "en": "Apa Itu Distributed Acoustic Sensing?", "id": "Sensor Getaran Sepanjang Kabel Optik." },
  { "en": "Apa Itu Distributed Temperature Sensing?", "id": "Sensor Suhu Sepanjang Kabel Optik." },
  { "en": "Apa Itu Smart Grid System?", "id": "Jaringan Listrik Cerdas Terintegrasi Digital." },
  { "en": "Apa Itu Demand Response Management?", "id": "Pengelolaan Beban Sesuai Ketersediaan Daya." },
  { "en": "Apa Itu Advanced Metering Infrastructure?", "id": "Sistem Meter Cerdas Komunikasi Dua Arah." },
  { "en": "Apa Itu Meter Data Management?", "id": "Sistem Pengolah Data Meter Pelanggan." },
  { "en": "Apa Itu Home Area Network?", "id": "Jaringan Perangkat Cerdas Dalam Rumah." },
  { "en": "Apa Itu Wide Area Network?", "id": "Jaringan Komunikasi Jarak Jauh Utilitas." },
  { "en": "Apa Itu Distribution Management System?", "id": "Sistem Pengelola Jaringan Distribusi Cerdas." },
  { "en": "Apa Itu Outage Management System?", "id": "Sistem Manajemen Pemadaman Dan Pemulihan." },
  { "en": "Apa Itu Geographic Information System?", "id": "Sistem Peta Aset Jaringan Listrik." },
  { "en": "Apa Itu Fault Location Isolation?", "id": "Lokalisasi Gangguan Dan Pemulihan Otomatis." },
  { "en": "Apa Itu Volt-Var Optimization?", "id": "Optimasi Tegangan Dan Daya Reaktif Jaringan." },
  { "en": "Apa Itu Conservation Voltage Reduction?", "id": "Menurunkan Tegangan Untuk Hemat Energi." },
  { "en": "Apa Itu Electric Vehicle Integration?", "id": "Integrasi Mobil Listrik Ke Grid." },
  { "en": "Apa Itu Vehicle to Grid?", "id": "Mobil Listrik Suplai Daya Ke Jaringan." },
  { "en": "Apa Itu Vehicle to Home?", "id": "Mobil Listrik Suplai Daya Ke Rumah." },
  { "en": "Apa Itu Smart Charging Station?", "id": "Stasiun Cas Mobil Terkontrol Jaringan." },
  { "en": "Apa Itu Renewable Energy Integration?", "id": "Integrasi Pembangkit Terbarukan Ke Grid." },
  { "en": "Apa Itu Grid Forming Inverter?", "id": "Inverter Pembentuk Tegangan Referensi Grid." },
  { "en": "Apa Itu Grid Following Inverter?", "id": "Inverter Mengikuti Tegangan Jaringan Ada." },
  { "en": "Apa Itu Virtual Inertia Control?", "id": "Simulasi Inersia Generator Pada Inverter." },
  { "en": "Apa Itu Microgrid Controller System?", "id": "Pengendali Operasi Pulau Jaringan Mikro." },
  { "en": "Apa Itu Battery Energy Storage?", "id": "Sistem Penyimpanan Energi Baterai Besar." },
  { "en": "Apa Itu Peak Shaving Application?", "id": "Mengurangi Beban Puncak Dengan Baterai." },
  { "en": "Apa Itu Frequency Regulation Battery?", "id": "Baterai Menjaga Stabilitas Frekuensi Grid." },
  { "en": "Apa Itu Generator Negative Sequence Capability?", "id": "Kemampuan Rotor Tahan Panas Arus Unbalance." },
  { "en": "Apa Itu Konstanta I2t Generator?", "id": "Batas Energi Panas Urutan Negatif." },
  { "en": "Apa Itu Generator Motoring Action?", "id": "Generator Berputar Mengambil Daya Aktif Jaringan." },
  { "en": "Apa Itu Reverse Power Protection 32?", "id": "Proteksi Mencegah Generator Menjadi Motor." },
  { "en": "Apa Itu Loss Of Excitation 40?", "id": "Hilangnya Medan Magnet Penguat Rotor." },
  { "en": "Apa Dampak Hilang Eksitasi Generator?", "id": "Generator Menyerap Daya Reaktif Besar." },
  { "en": "Apa Itu Inadvertent Energization 50/27?", "id": "Trafo Menyuplai Tegangan Ke Generator Mati." },
  { "en": "Apa Itu Voltage Controlled Overcurrent 51V?", "id": "Arus Lebih Sensitif Saat Tegangan Turun." },
  { "en": "Apa Itu Generator Differential 87G?", "id": "Proteksi Hubung Singkat Lilitan Stator." },
  { "en": "Apa Itu Stator Earth Fault 64S?", "id": "Proteksi Gangguan Tanah Lilitan Stator." },
  { "en": "Apa Itu Rotor Earth Fault 64R?", "id": "Proteksi Gangguan Tanah Lilitan Rotor." },
  { "en": "Apa Itu 100 Percent Stator Ground?", "id": "Proteksi Tanah Meliputi Titik Netral." },
  { "en": "Apa Itu Third Harmonic Undervoltage?", "id": "Metode Proteksi Tanah Titik Netral." },
  { "en": "Apa Itu Pole Slip Protection 78?", "id": "Deteksi Lepas Langkah Sinkronisasi Generator." },
  { "en": "Apa Itu Out Of Step Blocking?", "id": "Mencegah Trip Saat Ayunan Daya Stabil." },
  { "en": "Apa Itu Volts Per Hertz 24?", "id": "Proteksi Overfluxing Inti Besi Generator." },
  { "en": "Apa Itu Frequency Protection 81?", "id": "Mencegah Operasi Frekuensi Tidak Normal." },
  { "en": "Apa Itu Rate Of Change Frequency?", "id": "Deteksi Perubahan Frekuensi Sangat Cepat." },
  { "en": "Apa Itu Vector Shift Relay?", "id": "Deteksi Pergeseran Sudut Fasa Tegangan." },
  { "en": "Apa Itu Breaker Failure Protection 50BF?", "id": "Trip Busbar Jika Breaker Macet." },
  { "en": "Apa Itu Trip Circuit Supervision 74?", "id": "Memantau Kontinuitas Rangkaian Koil Trip." },
  { "en": "Apa Itu Lockout Relay 86?", "id": "Mengunci Sistem Setelah Gangguan Berat." },
  { "en": "Apa Itu Check Synchronizing 25?", "id": "Izin Menutup Breaker Saat Sinkron." },
  { "en": "Apa Itu Phase Angle Difference?", "id": "Beda Sudut Fasa Generator Dan Grid." },
  { "en": "Apa Itu Voltage Difference Check?", "id": "Beda Tegangan Generator Dan Grid." },
  { "en": "Apa Itu Slip Frequency Check?", "id": "Beda Frekuensi Generator Dan Grid." },
  { "en": "Apa Itu Dead Bus Charging?", "id": "Generator Menyuplai Busbar Yang Mati." },
  { "en": "Apa Itu Auto Reclose 79?", "id": "Menutup Kembali Breaker Transmisi Otomatis." },
  { "en": "Apa Itu Dead Time Reclose?", "id": "Jeda Waktu Sebelum Menutup Kembali." },
  { "en": "Apa Itu Reclaim Time Reclose?", "id": "Waktu Reset Setelah Sukses Menutup." },
  { "en": "Apa Itu Blocking Reclose?", "id": "Mencegah Reclose Pada Gangguan Permanen." },
  { "en": "Apa Itu Distance Protection 21?", "id": "Proteksi Impedansi Saluran Transmisi." },
  { "en": "Apa Itu Mho Characteristic?", "id": "Kurva Impedansi Berbentuk Lingkaran." },
  { "en": "Apa Itu Quadrilateral Characteristic?", "id": "Kurva Impedansi Berbentuk Segi Empat." },
  { "en": "Apa Itu Load Encroachment?", "id": "Impedansi Beban Masuk Zona Trip." },
  { "en": "Apa Itu Switch Onto Fault?", "id": "Trip Instan Menutup Ke Gangguan." },
  { "en": "Apa Itu Stub Bus Protection?", "id": "Proteksi Area Buta CT Breaker." },
  { "en": "Apa Itu Directional Overcurrent 67?", "id": "Arus Lebih Hanya Satu Arah." },
  { "en": "Apa Itu Polarization Voltage?", "id": "Referensi Arah Untuk Relay Directional." },
  { "en": "Apa Itu Teleprotection Scheme?", "id": "Komunikasi Antar Relay Ujung Saluran." },
  { "en": "Apa Itu Permissive Underreach Transfer Trip?", "id": "Trip Diizinkan Sinyal Zona 1 Lawan." },
  { "en": "Apa Itu Permissive Overreach Transfer Trip?", "id": "Trip Diizinkan Sinyal Zona 2 Lawan." },
  { "en": "Apa Itu Direct Transfer Trip?", "id": "Trip Langsung Tanpa Syarat Tambahan." },
  { "en": "Apa Itu Current Differential 87L?", "id": "Banding Arus Masuk Keluar Saluran." },
  { "en": "Apa Itu Fiber Optic Differential?", "id": "Komunikasi Diferensial Lewat Serat Optik." },
  { "en": "Apa Itu Busbar Differential 87B?", "id": "Proteksi Hubung Singkat Rel Daya." },
  { "en": "Apa Itu Low Impedance Differential?", "id": "Diferensial Berbasis Algoritma Relay Numerik." },
  { "en": "Apa Itu Check Zone Busbar?", "id": "Zona Verifikasi Keseluruhan Busbar." },
  { "en": "Apa Itu CT Supervision 87B?", "id": "Deteksi Rangkaian CT Busbar Terbuka." },
  { "en": "Apa Itu Zone Selection Logic?", "id": "Logika Memilih Busbar Yang Terganggu." },
  { "en": "Apa Itu Circuit Breaker Monitor?", "id": "Alat Pantau Kondisi Mekanis Breaker." },
  { "en": "Apa Itu SF6 Gas Monitor?", "id": "Alat Pantau Tekanan Gas Isolasi." },
  { "en": "Apa Itu I2t Summation?", "id": "Akumulasi Energi Busur Kontak Breaker." },
  { "en": "Apa Itu Operating Time Monitor?", "id": "Pantau Kecepatan Buka Tutup Breaker." },
  { "en": "Apa Itu Coil Current Signature?", "id": "Profil Arus Koil Trip Breaker." },
  { "en": "Apa Itu Partial Discharge Monitor?", "id": "Pantau Peluahan Listrik Isolasi GIS." },
  { "en": "Apa Itu Acoustic PD Sensor?", "id": "Sensor Suara Ultrasonik PD." },
  { "en": "Apa Itu Dissolved Gas Monitor?", "id": "Pantau Gas Terlarut Minyak Trafo." },
  { "en": "Apa Itu Moisture In Oil Sensor?", "id": "Sensor Kadar Air Minyak Trafo." },
  { "en": "Apa Itu Bushing Monitor?", "id": "Pantau Tan Delta Bushing Trafo." },
  { "en": "Apa Itu OLTC Monitor?", "id": "Pantau Kondisi Tap Changer Trafo." },
  { "en": "Apa Itu Motor Current Signature?", "id": "Analisis Spektrum Arus Motor OLTC." },
  { "en": "Apa Itu Thermography Monitor?", "id": "Pantau Suhu Panas Sambungan Busbar." },
  { "en": "Apa Itu Battery Monitor System?", "id": "Pantau Tegangan Dan Impedansi Baterai." },
  { "en": "Apa Itu Grounding System Monitor?", "id": "Pantau Kontinuitas Kabel Pentanahan." },
  { "en": "Apa Itu Digital Substation?", "id": "Gardu Induk Berbasis Komunikasi IEC61850." },
  { "en": "Apa Itu Station Bus IEC61850?", "id": "Jaringan Komunikasi Kontrol Antar IED." },
  { "en": "Apa Itu Process Bus IEC61850?", "id": "Jaringan Data Arus Tegangan Digital." },
  { "en": "Apa Itu Merging Unit?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Sampled Values?", "id": "Data Digital Arus Dan Tegangan." },
  { "en": "Apa Itu Redundant Network PRP?", "id": "Jaringan Ganda Paralel Tanpa Jeda." },
  { "en": "Apa Itu Redundant Network HSR?", "id": "Jaringan Cincin Ganda Tanpa Jeda." },
  { "en": "Apa Itu Cyber Security IEC62351?", "id": "Standar Keamanan Protokol Sistem Kuasa." },
  { "en": "Apa Itu Role Based Access?", "id": "Hak Akses Sesuai Peran User." },
  { "en": "Apa Itu Fiber Optic Sensor?", "id": "Sensor Pasif Tahan Interferensi Elektromagnetik." },
  { "en": "Apa Itu Optical CT?", "id": "Trafo Arus Menggunakan Efek Faraday." },
  { "en": "Apa Itu Optical VT?", "id": "Trafo Tegangan Menggunakan Efek Pockels." },
  { "en": "Apa Itu Dynamic Line Rating?", "id": "Kapasitas Kabel Berubah Sesuai Suhu." },
  { "en": "Apa Itu Sag Monitoring?", "id": "Pantau Lendutan Kabel Transmisi." },
  { "en": "Apa Itu Ice Load Monitor?", "id": "Pantau Beban Es Pada Kabel." },
  { "en": "Apa Itu Wide Area Monitoring?", "id": "Pantau Grid Menggunakan Phasor Unit." },
  { "en": "Apa Itu Phasor Measurement Unit?", "id": "Alat Ukur Fasor Sinkron GPS." },
  { "en": "Apa Itu Phasor Data Concentrator?", "id": "Pengumpul Data Dari Banyak PMU." },
  { "en": "Apa Itu Voltage Stability Monitor?", "id": "Pantau Batas Kestabilan Tegangan Grid." },
  { "en": "Apa Itu Oscillation Monitor?", "id": "Pantau Ayunan Daya Antar Area." },
  { "en": "Apa Itu Islanding Detection?", "id": "Deteksi Pecahnya Grid Menjadi Pulau." },
  { "en": "Apa Itu Black Start Capability?", "id": "Kemampuan Start Tanpa Listrik Luar." },
  { "en": "Apa Itu Islanding Operation Mode?", "id": "Operasi Terpisah Dari Jaringan Utama." },
  { "en": "Apa Itu Grid Connected Mode?", "id": "Operasi Terhubung Dengan Jaringan Utama." },
  { "en": "Apa Itu Voltage Control Mode?", "id": "Inverter Mengatur Tegangan Keluaran Stabil." },
  { "en": "Apa Itu Current Control Mode?", "id": "Inverter Mengatur Arus Keluaran Stabil." },
  { "en": "Apa Itu Droop Control Strategy?", "id": "Berbagi Beban Tanpa Komunikasi Data." },
  { "en": "Apa Itu Master Slave Control?", "id": "Satu Unit Mengatur Unit Lainnya." },
  { "en": "Apa Itu Peer to Peer Control?", "id": "Kontrol Terdistribusi Tanpa Pusat Pengendali." },
  { "en": "Apa Itu Virtual Synchronous Machine?", "id": "Inverter Meniru Karakteristik Generator Sinkron." },
  { "en": "Apa Itu Microgrid Energy Management?", "id": "Sistem Pengelola Energi Jaringan Mikro." },
  { "en": "Apa Itu Distributed Energy Resource? (DER)", "id": "Sumber Energi Tersebar Skala Kecil." },
  { "en": "Apa Itu Point of Common Coupling? (PCC)", "id": "Titik Sambung Pelanggan Dengan Grid." },
  { "en": "Apa Itu Hosting Capacity Grid?", "id": "Kapasitas Maksimum Menerima Pembangkit Tersebar." },
  { "en": "Apa Itu Reverse Power Flow?", "id": "Aliran Daya Balik Ke Subtransmisi." },
  { "en": "Apa Itu Voltage Rise Feeder?", "id": "Kenaikan Tegangan Akibat Pembangkit PV." },
  { "en": "Apa Itu Tap Changer Hunting?", "id": "Perubahan Tap Berulang Akibat Fluktuasi." },
  { "en": "Apa Itu Flicker Emission PV?", "id": "Kedipan Cahaya Akibat Awan Lewat." },
  { "en": "Apa Itu Harmonic Injection PV?", "id": "Inverter Menyuntikkan Harmonisa Ke Grid." },
  { "en": "Apa Itu DC Injection PV?", "id": "Inverter Menyuntikkan Arus DC Bahaya." },
  { "en": "Apa Itu Anti-Islanding Protection?", "id": "Mencegah Operasi Pulau Saat Gangguan." },
  { "en": "Apa Itu Loss of Mains Protection?", "id": "Deteksi Hilangnya Sumber Utama Grid." },
  { "en": "Apa Itu Rate of Change Frequency? (ROCOF)", "id": "Deteksi Perubahan Cepat Frekuensi Sistem." },
  { "en": "Apa Itu Vector Shift Protection?", "id": "Deteksi Pergeseran Sudut Fasa Tegangan." },
  { "en": "Apa Itu Intertrip Signal Protection?", "id": "Sinyal Trip Kiriman Dari Gardu." },
  { "en": "Apa Itu Fault Ride Through?", "id": "Kemampuan Bertahan Saat Grid Gangguan." },
  { "en": "Apa Itu Low Voltage Ride Through? (LVRT)", "id": "Bertahan Saat Tegangan Grid Turun." },
  { "en": "Apa Itu High Voltage Ride Through? (HVRT)", "id": "Bertahan Saat Tegangan Grid Naik." },
  { "en": "Apa Itu Reactive Power Support?", "id": "Inverter Membantu Tegangan Saat Gangguan." },
  { "en": "Apa Itu Dynamic Grid Support?", "id": "Respon Cepat Inverter Menstabilkan Grid." },
  { "en": "Apa Itu Ramp Rate Control?", "id": "Membatasi Kecepatan Perubahan Daya Output." },
  { "en": "Apa Itu Frequency Watt Response?", "id": "Mengurangi Daya Saat Frekuensi Naik." },
  { "en": "Apa Itu Volt Var Response?", "id": "Mengatur Var Berdasarkan Tegangan Grid." },
  { "en": "Apa Itu Volt Watt Response?", "id": "Mengurangi Watt Saat Tegangan Tinggi." },
  { "en": "Apa Itu Power Factor Control?", "id": "Menjaga Faktor Daya Tetap Konstan." },
  { "en": "Apa Itu Reactive Power Priority?", "id": "Mengutamakan Var Daripada Watt Inverter." },
  { "en": "Apa Itu Solar Forecast System?", "id": "Prediksi Produksi Surya Berdasarkan Cuaca." },
  { "en": "Apa Itu Wind Power Forecast?", "id": "Prediksi Produksi Angin Berdasarkan Cuaca." },
  { "en": "Apa Itu Net Load Curve?", "id": "Beban Total Kurangi Produksi Terbarukan." },
  { "en": "Apa Itu Duck Curve Phenomenon?", "id": "Beban Siang Rendah Akibat Surya." },
  { "en": "Apa Itu Ramping Requirement Grid?", "id": "Kebutuhan Pembangkit Cepat Sore Hari." },
  { "en": "Apa Itu Curtailment Energy?", "id": "Pemangkasan Produksi Energi Terbarukan Berlebih." },
  { "en": "Apa Itu Energy Storage Integration?", "id": "Menggunakan Baterai Untuk Simpan Lebih." },
  { "en": "Apa Itu Battery Round Trip Efficiency?", "id": "Efisiensi Siklus Cas Dan Pakai." },
  { "en": "Apa Itu Battery Calendar Life?", "id": "Umur Baterai Berdasarkan Waktu Simpan." },
  { "en": "Apa Itu Battery Cycle Life?", "id": "Umur Baterai Berdasarkan Jumlah Pakai." },
  { "en": "Apa Itu Depth of Discharge? (DOD)", "id": "Persentase Kapasitas Baterai Yang Dipakai." },
  { "en": "Apa Itu State of Charge? (SOC)", "id": "Persentase Sisa Energi Dalam Baterai." },
  { "en": "Apa Itu State of Health? (SOH)", "id": "Kondisi Kesehatan Baterai Banding Baru." },
  { "en": "Apa Itu Battery Management System? (BMS)", "id": "Elektronik Pengaman Dan Penyeimbang Baterai." },
  { "en": "Apa Itu Cell Balancing Passive?", "id": "Membuang Energi Sel Tinggi Resistor." },
  { "en": "Apa Itu Cell Balancing Active?", "id": "Memindah Energi Sel Tinggi Rendah." },
  { "en": "Apa Itu Thermal Runaway Battery?", "id": "Kenaikan Suhu Baterai Tak Terkendali." },
  { "en": "Apa Itu Lithium Plating?", "id": "Endapan Lithium Logam Saat Cas." },
  { "en": "Apa Itu Dendrite Growth?", "id": "Pertumbuhan Kristal Tajam Tembus Separator." },
  { "en": "Apa Itu Battery Container System?", "id": "Sistem Baterai Skala Besar Kontainer." },
  { "en": "Apa Itu Fire Suppression Battery?", "id": "Sistem Pemadam Api Khusus Baterai." },
  { "en": "Apa Itu HVAC Battery Container?", "id": "Pendingin Udara Jaga Suhu Baterai." },
  { "en": "Apa Itu SCADA Battery System?", "id": "Sistem Monitor Baterai Jarak Jauh." },
  { "en": "Apa Itu PCS? (Power Conversion System)", "id": "Inverter Dua Arah Untuk Baterai." },
  { "en": "Apa Itu Grid Forming PCS?", "id": "PCS Membentuk Tegangan Referensi Grid." },
  { "en": "Apa Itu Black Start BESS?", "id": "Baterai Menyalakan Grid Yang Mati." },
  { "en": "Apa Itu Frequency Regulation BESS?", "id": "Baterai Menjaga Stabilitas Frekuensi Grid." },
  { "en": "Apa Itu Peak Shaving BESS?", "id": "Baterai Memotong Beban Puncak Listrik." },
  { "en": "Apa Itu Arbitrage Energy BESS?", "id": "Simpan Murah Jual Mahal Listrik." },
  { "en": "Apa Itu Capacity Firming BESS?", "id": "Menstabilkan Output Pembangkit Energi Terbarukan." },
  { "en": "Apa Itu Transmission Deferral?", "id": "Menunda Investasi Kabel Baru Pakai Baterai." },
  { "en": "Apa Itu Voltage Support BESS?", "id": "Baterai Menyuntik Var Jaga Tegangan." },
  { "en": "Apa Itu Subsynchronous Resonance Damping?", "id": "Meredam Osilasi Frekuensi Rendah Generator." },
  { "en": "Apa Itu Power Oscillation Damping?", "id": "Meredam Ayunan Daya Antar Area." },
  { "en": "Apa Itu Synthetic Inertia BESS?", "id": "Baterai Simulasi Inersia Generator Putar." },
  { "en": "Apa Itu Fast Frequency Response?", "id": "Respon Baterai Sangat Cepat Detik." },
  { "en": "Apa Itu Demand Charge Reduction?", "id": "Mengurangi Biaya Beban Puncak Pelanggan." },
  { "en": "Apa Itu Backup Power BESS?", "id": "Baterai Sebagai Cadangan Saat Padam." },
  { "en": "Apa Itu UPS Uninterruptible Power Supply?", "id": "Suplai Daya Tanpa Jeda Baterai." },
  { "en": "Apa Itu Double Conversion UPS?", "id": "UPS Konversi AC-DC-AC Penuh." },
  { "en": "Apa Itu Line Interactive UPS?", "id": "UPS Dengan Trafo Regulator Tegangan." },
  { "en": "Apa Itu Offline Standby UPS?", "id": "UPS Pindah Baterai Saat Padam." },
  { "en": "Apa Itu Static Bypass Switch?", "id": "Saklar Elektronik Pintas Jalur UPS." },
  { "en": "Apa Itu Maintenance Bypass Switch?", "id": "Saklar Manual Pintas Servis UPS." },
  { "en": "Apa Itu Input Harmonic Filter?", "id": "Filter Harmonisa Sisi Masukan UPS." },
  { "en": "Apa Itu Output Isolation Transformer?", "id": "Trafo Isolasi Sisi Keluaran UPS." },
  { "en": "Apa Itu Battery Autonomy Time?", "id": "Durasi Baterai Suplai Beban Penuh." },
  { "en": "Apa Itu End of Discharge Voltage?", "id": "Tegangan Batas Bawah Baterai Kosong." },
  { "en": "Apa Itu Battery Recharge Time?", "id": "Waktu Isi Ulang Baterai Penuh." },
  { "en": "Apa Itu Crest Factor Load?", "id": "Rasio Arus Puncak Terhadap RMS." },
  { "en": "Apa Itu Inrush Current Load?", "id": "Arus Awal Besar Saat Nyala." },
  { "en": "Apa Itu Short Circuit Clearance?", "id": "Kemampuan UPS Trip Breaker Hilir." },
  { "en": "Apa Itu Parallel Redundant UPS?", "id": "Dua UPS Berbagi Beban Bersama." },
  { "en": "Apa Itu Hot Standby UPS?", "id": "Satu UPS Jalan Satu Siaga." },
  { "en": "Apa Itu Modular UPS System?", "id": "UPS Terdiri Dari Modul-Modul Kecil." },
  { "en": "Apa Itu Hot Swappable Module?", "id": "Ganti Modul Tanpa Matikan UPS." },
  { "en": "Apa Itu SNMP Network Card?", "id": "Kartu Monitor UPS Lewat Jaringan." },
  { "en": "Apa Itu Environmental Sensor Probe?", "id": "Sensor Suhu Kelembaban Ruang Server." },
  { "en": "Apa Itu Emergency Power Off? (EPO)", "id": "Tombol Matikan UPS Saat Darurat." },
  { "en": "Apa Itu Generator Walk-in?", "id": "Genset Sinkronisasi Dengan Input UPS." },
  { "en": "Apa Itu Slew Rate Synchronization?", "id": "Kecepatan Penyesuaian Frekuensi UPS Generator." },
  { "en": "Apa Itu Input Power Factor?", "id": "Faktor Daya Sisi Masukan UPS." },
  { "en": "Apa Itu Output Power Factor?", "id": "Faktor Daya Kemampuan Beban UPS." },
  { "en": "Apa Itu Efficiency Curve UPS?", "id": "Grafik Efisiensi Terhadap Persen Beban." },
  { "en": "Apa Itu Eco Mode UPS?", "id": "Mode Hemat Energi Bypass Normal." },
  { "en": "Apa Itu Buck Converter?", "id": "Penurun Tegangan DC Ke DC." },
  { "en": "Apa Itu Boost Converter?", "id": "Penaik Tegangan DC Ke DC." },
  { "en": "Apa Itu Buck-Boost Converter?", "id": "Bisa Naik Dan Turunkan Tegangan DC." },
  { "en": "Apa Itu Cuk Converter?", "id": "Konverter DC Output Polaritas Terbalik." },
  { "en": "Apa Itu Duty Cycle?", "id": "Persentase Waktu Saklar Posisi On." },
  { "en": "Apa Itu Switching Loss?", "id": "Rugi Daya Saat Transisi Saklar." },
  { "en": "Apa Itu Conduction Loss?", "id": "Rugi Daya Saat Arus Mengalir." },
  { "en": "Apa Itu Reverse Recovery Time?", "id": "Waktu Pemulihan Dioda Saat Mati." },
  { "en": "Apa Itu Dead Time Inverter?", "id": "Jeda Waktu Mencegah Hubung Singkat." },
  { "en": "Apa Itu Gate Driver?", "id": "Penguat Sinyal Pemicu Gerbang MOSFET." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Peredam Lonjakan Tegangan Pada Saklar." },
  { "en": "Apa Itu Flyback Diode?", "id": "Dioda Pengaman Beban Induktif." },
  { "en": "Apa Itu Pulse Width Modulation? (PWM)", "id": "Metode Kontrol Lebar Pulsa Sinyal." },
  { "en": "Apa Itu Space Vector PWM?", "id": "Teknik PWM Optimalkan Tegangan Bus." },
  { "en": "Apa Itu Power Factor Correction? (PFC)", "id": "Perbaikan Faktor Daya Sisi Input." },
  { "en": "Apa Itu Active PFC?", "id": "PFC Menggunakan Rangkaian Elektronik Switching." },
  { "en": "Apa Itu Passive PFC?", "id": "PFC Menggunakan Induktor Dan Kapasitor." },
  { "en": "Apa Itu Galvanic Isolation?", "id": "Pemisahan Listrik Antara Dua Sirkuit." },
  { "en": "Apa Itu Optocoupler?", "id": "Isolator Sinyal Menggunakan Cahaya." },
  { "en": "Apa Itu Heat Sink?", "id": "Logam Pembuang Panas Komponen Elektronik." },
  { "en": "Apa Itu Thermal Paste?", "id": "Pasta Penghantar Panas Ke Heatsink." },
  { "en": "Apa Itu Junction Temperature?", "id": "Suhu Inti Chip Semikonduktor." },
  { "en": "Apa Itu Safe Operating Area? (SOA)", "id": "Batas Aman Tegangan Arus Transistor." },
  { "en": "Apa Itu IGBT? (Insulated Gate Bipolar Transistor)", "id": "Gabungan Keunggulan MOSFET Dan BJT." },
  { "en": "Apa Itu MOSFET?", "id": "Transistor Efek Medan Logam Oksida." },
  { "en": "Apa Itu Thyristor SCR?", "id": "Saklar Semikonduktor Terkendali Satu Arah." },
  { "en": "Apa Itu TRIAC?", "id": "Saklar Semikonduktor AC Dua Arah." },
  { "en": "Apa Itu DIAC?", "id": "Pemicu Gate TRIAC Tanpa Polaritas." },
  { "en": "Apa Itu Zener Diode?", "id": "Dioda Penstabil Tegangan Arah Mundur." },
  { "en": "Apa Itu Schottky Diode?", "id": "Dioda Tegangan Jatuh Rendah Cepat." },
  { "en": "Apa Itu Varistor MOV?", "id": "Resistor Variabel Pelindung Surja Tegangan." },
  { "en": "Apa Itu Thermistor NTC?", "id": "Resistor Turun Nilai Saat Panas." },
  { "en": "Apa Itu Thermistor PTC?", "id": "Resistor Naik Nilai Saat Panas." },
  { "en": "Apa Itu MPPT Perturb And Observe?", "id": "Algoritma Pelacak Daya Maksimum Sederhana." },
  { "en": "Apa Itu MPPT Incremental Conductance?", "id": "Algoritma Pelacak Daya Maksimum Presisi." },
  { "en": "Apa Itu Grid Forming Inverter?", "id": "Inverter Membentuk Tegangan Referensi Grid." },
  { "en": "Apa Itu Virtual Inertia?", "id": "Simulasi Inersia Generator Pada Inverter." },
  { "en": "Apa Itu Low Voltage Ride Through?", "id": "Bertahan Saat Tegangan Grid Turun." },
  { "en": "Apa Itu Regenerative Braking?", "id": "Mengembalikan Energi Pengereman Ke Sumber." },
  { "en": "Apa Itu Dynamic Braking Resistor?", "id": "Resistor Pembuang Energi Rem Panas." },
  { "en": "Apa Itu Slip Compensation?", "id": "Menjaga Kecepatan Motor Saat Beban." },
  { "en": "Apa Itu Torque Boost?", "id": "Menambah Tegangan Start Torsi Tinggi." },
  { "en": "Apa Itu Vector Control?", "id": "Kontrol Terpisah Torsi Dan Fluks." },
  { "en": "Apa Itu V/f Control?", "id": "Kontrol Tegangan Frekuensi Konstan Sederhana." },
  { "en": "Apa Itu Soft Starter Bypass?", "id": "Kontaktor Pintas Setelah Motor Jalan." },
  { "en": "Apa Itu Motor Service Factor?", "id": "Kemampuan Beban Lebih Motor." },
  { "en": "Apa Itu Insulation Class F?", "id": "Tahan Suhu Hingga 155 Derajat." },
  { "en": "Apa Itu Insulation Class H?", "id": "Tahan Suhu Hingga 180 Derajat." },
  { "en": "Apa Itu Frame Size Motor?", "id": "Ukuran Fisik Standar Rangka Motor." },
  { "en": "Apa Itu Totally Enclosed Fan Cooled? (TEFC)", "id": "Motor Tertutup Rapat Pendingin Kipas." },
  { "en": "Apa Itu Explosion Proof Motor?", "id": "Motor Tahan Ledakan Lingkungan Berbahaya." },
  { "en": "Apa Itu Locked Rotor Current?", "id": "Arus Saat Rotor Motor Macet." },
  { "en": "Apa Itu Cogging Torque?", "id": "Torsi Riak Akibat Slot Stator." },
  { "en": "Apa Itu Single Phasing Motor?", "id": "Hilangnya Satu Fasa Motor 3-Fasa." },
  { "en": "Apa Itu Phase Imbalance?", "id": "Ketidakseimbangan Tegangan Antar Fasa." },
  { "en": "Apa Itu Bearing Fluting?", "id": "Kerusakan Bantalan Akibat Arus Poros." },
  { "en": "Apa Itu Shaft Grounding Ring?", "id": "Cincin Pentanahan Poros Motor VFD." },
  { "en": "Apa Itu Hi-Pot Test?", "id": "Uji Tahan Tegangan Isolasi Tinggi." },
  { "en": "Apa Itu Megger Test?", "id": "Uji Tahanan Isolasi Tegangan DC." },
  { "en": "Apa Itu Polarization Index? (PI)", "id": "Rasio Megger 10 Menit 1 Menit." },
  { "en": "Apa Itu Winding Resistance Test?", "id": "Ukur Tahanan Tembaga Lilitan Motor." },
  { "en": "Apa Itu Vibration Analysis?", "id": "Analisis Kesehatan Mesin Lewat Getaran." },
  { "en": "Apa Itu Misalignment Poros?", "id": "Ketidaklurusan Sumbu Poros Motor Beban." },
  { "en": "Apa Itu Soft Foot?", "id": "Kaki Motor Tidak Menapak Rata." },
  { "en": "Apa Itu Unbalance Rotor?", "id": "Ketidakseimbangan Massa Putaran Rotor." },
  { "en": "Apa Itu Thermography Inspection?", "id": "Deteksi Panas Berlebih Sambungan Listrik." },
  { "en": "Apa Itu Oil Analysis?", "id": "Cek Kualitas Pelumas Deteksi Aus." },
  { "en": "Apa Itu Ultrasound Detection?", "id": "Deteksi Kebocoran Atau Gesekan Bantalan." },
  { "en": "Apa Itu Laser Alignment Tool?", "id": "Alat Luruskan Poros Presisi Tinggi." },
  { "en": "Apa Itu Stroboscope?", "id": "Alat Ukur Kecepatan Putar Optik." },
  { "en": "Apa Itu Voltage Sag?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Voltage Swell?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Flicker Tegangan?", "id": "Kedipan Cahaya Akibat Fluktuasi Tegangan." },
  { "en": "Apa Itu Notch Tegangan?", "id": "Cacat Gelombang Akibat Komutasi Penyearah." },
  { "en": "Apa Itu Interharmonics?", "id": "Frekuensi Bukan Kelipatan Bulat Dasar." },
  { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Trafo Tahan Panas Harmonisa." },
  { "en": "Apa Itu Passive Filter?", "id": "Filter Harmonisa RLC Sederhana." },
  { "en": "Apa Itu Active Filter?", "id": "Filter Harmonisa Elektronik Umpan Balik." },
  { "en": "Apa Itu Detuned Reactor?", "id": "Reaktor Cegah Resonansi Kapasitor Bank." },
  { "en": "Apa Itu Zero Sequence Filter?", "id": "Trafo Zigzag Blokir Harmonisa Ketiga." },
  { "en": "Apa Itu UPS Online?", "id": "UPS Konversi Ganda Tanpa Jeda." },
  { "en": "Apa Itu UPS Line Interactive?", "id": "UPS Dengan Stabilizer Trafo AVR." },
  { "en": "Apa Itu Static Bypass?", "id": "Jalur Pintas UPS Saklar Elektronik." },
  { "en": "Apa Itu Maintenance Bypass?", "id": "Saklar Pintas Manual Servis UPS." },
  { "en": "Apa Itu Battery Autonomy?", "id": "Durasi Baterai Suplai Beban Penuh." },
  { "en": "Apa Itu Valve Regulated Lead Acid? (VRLA)", "id": "Aki Kering Bebas Perawatan." },
  { "en": "Apa Itu Lithium Iron Phosphate? (LiFePO4)", "id": "Baterai Lithium Aman Tahan Lama." },
  { "en": "Apa Itu Circuit Breaker Trip Coil?", "id": "Kumparan Pembuka Kontak Pemutus Tenaga." },
  { "en": "Apa Itu Close Coil Circuit Breaker?", "id": "Kumparan Penutup Kontak Pemutus Tenaga." },
  { "en": "Apa Itu Anti Pumping Relay Function?", "id": "Mencegah Breaker Tutup Buka Berulang." },
  { "en": "Apa Itu Trip Circuit Supervision Relay?", "id": "Memantau Kesiapan Rangkaian Trip Coil." },
  { "en": "Apa Itu Lockout Relay Reset Function?", "id": "Mengembalikan Relay Ke Posisi Normal." },
  { "en": "Apa Itu HVDC Converter Transformer?", "id": "Trafo Khusus Sistem Transmisi DC." },
  { "en": "Apa Itu Valve Hall HVDC Station?", "id": "Ruangan Berisi Katup Thyristor Besar." },
  { "en": "Apa Itu Smoothing Reactor HVDC Link?", "id": "Induktor Perata Arus DC Transmisi." },
  { "en": "Apa Itu Smart Meter Tamper Switch?", "id": "Saklar Deteksi Pembukaan Tutup Meter." },
  { "en": "Apa Itu Power Line Carrier Communication?", "id": "Komunikasi Data Lewat Kabel Listrik." },
  { "en": "Apa Itu Wave Trap Blocking Coil?", "id": "Induktor Penahan Frekuensi Tinggi PLC." },
  { "en": "Apa Itu Transformer Turns Ratio Test?", "id": "Mengukur Perbandingan Lilitan Primer Sekunder." },
  { "en": "Apa Itu Winding Resistance Measurement Test?", "id": "Mengukur Tahanan Tembaga Lilitan Trafo." },
  { "en": "Apa Itu Buchholz Relay Gas Sampling?", "id": "Mengambil Contoh Gas Dari Relay." },
  { "en": "Apa Itu Silica Gel Breather Color?", "id": "Biru Saat Kering Pink Basah." },
  { "en": "Apa Itu Conservator Tank Oil Level?", "id": "Indikator Volume Minyak Dalam Tangki." },
  { "en": "Apa Itu Total Harmonic Distortion Voltage?", "id": "Persentase Cacat Gelombang Tegangan Total." },
  { "en": "Apa Itu Short Circuit Ratio Grid?", "id": "Kekuatan Sistem Di Titik Sambung." },
  { "en": "Apa Itu Weak Grid Impedance Level?", "id": "Sistem Dengan Impedansi Hubung Tinggi." },
  { "en": "Apa Itu Strong Grid Impedance Level?", "id": "Sistem Dengan Impedansi Hubung Rendah." },
  { "en": "Apa Itu Voltage Sag Ride Through?", "id": "Kemampuan Bertahan Saat Tegangan Turun." },
  { "en": "Apa Itu Dynamic Voltage Restorer Function?", "id": "Inverter Seri Kompensasi Tegangan Sag." },
  { "en": "Apa Itu Static Transfer Switch Function?", "id": "Saklar Pindah Sumber Sangat Cepat." },
  { "en": "Apa Itu UPS Double Conversion Mode?", "id": "Konversi AC Ke DC Ke AC." },
  { "en": "Apa Itu UPS Static Bypass Switch?", "id": "Jalur Pintas Elektronik Saat Gangguan." },
  { "en": "Apa Itu Maintenance Bypass Switch UPS?", "id": "Saklar Manual Untuk Servis UPS." },
  { "en": "Apa Itu Battery Autonomy Time UPS?", "id": "Durasi Baterai Suplai Beban Penuh." },
  { "en": "Apa Itu Valve Regulated Lead Acid?", "id": "Aki Kering Bebas Perawatan Rutin." },
  { "en": "Apa Itu Lithium Iron Phosphate Battery?", "id": "Baterai Lithium Aman Tahan Lama." },
  { "en": "Apa Itu Battery Management System Function?", "id": "Menyeimbangkan Dan Melindungi Sel Baterai." },
  { "en": "Apa Itu Thermal Runaway Battery Hazard?", "id": "Kenaikan Suhu Baterai Tak Terkendali." },
  { "en": "Apa Itu Electric Vehicle Charging Station?", "id": "Tempat Pengisian Baterai Mobil Listrik." },
  { "en": "Apa Itu DC Fast Charging Station?", "id": "Pengisian Cepat Arus Searah Besar." },
  { "en": "Apa Itu Vehicle To Grid Technology?", "id": "Mobil Listrik Suplai Daya Jaringan." },
  { "en": "Apa Itu Solar Inverter MPPT Function?", "id": "Melacak Titik Daya Maksimum Panel." },
  { "en": "Apa Itu Grid Tie Solar Inverter?", "id": "Inverter Sinkron Dengan Jaringan PLN." },
  { "en": "Apa Itu Off Grid Solar Inverter?", "id": "Inverter Mandiri Menggunakan Baterai Bank." },
  { "en": "Apa Itu Hybrid Solar Inverter Mode?", "id": "Gabungan Fungsi On-Grid Dan Off-Grid." },
  { "en": "Apa Itu Anti Islanding Protection Function?", "id": "Mencegah Inverter Operasi Saat Padam." },
  { "en": "Apa Itu Rapid Shutdown Solar System?", "id": "Mematikan Tegangan Panel Saat Darurat." },
  { "en": "Apa Itu String Combiner Box DC?", "id": "Kotak Penggabung Jalur Panel Surya." },
  { "en": "Apa Itu DC Surge Protection Device?", "id": "Pelindung Petir Sisi Arus Searah." },
  { "en": "Apa Itu Solar Panel Efficiency Rating?", "id": "Persentase Konversi Cahaya Ke Listrik." },
  { "en": "Apa Itu Wind Turbine Pitch Control?", "id": "Mengatur Sudut Bilah Terhadap Angin." },
  { "en": "Apa Itu Wind Turbine Yaw Control?", "id": "Mengatur Arah Turbin Menghadap Angin." },
  { "en": "Apa Itu Cut In Wind Speed?", "id": "Kecepatan Angin Minimum Turbin Mula." },
  { "en": "Apa Itu Cut Out Wind Speed?", "id": "Kecepatan Angin Maksimum Turbin Berhenti." },
  { "en": "Apa Itu Doubly Fed Induction Generator?", "id": "Generator Rotor Stator Terhubung Grid." },
  { "en": "Apa Itu Geothermal Power Plant Cycle?", "id": "Siklus Uap Panas Bumi Pembangkit." },
  { "en": "Apa Itu Dry Steam Geothermal Plant?", "id": "Pembangkit Menggunakan Uap Kering Langsung." },
  { "en": "Apa Itu Flash Steam Geothermal Plant?", "id": "Pembangkit Menggunakan Air Panas Tekanan." },
  { "en": "Apa Itu Binary Cycle Geothermal Plant?", "id": "Pembangkit Menggunakan Fluida Kerja Sekunder." },
  { "en": "Apa Itu Cooling Tower Function Plant?", "id": "Mendinginkan Air Sirkulasi Kondensor Uap." },
  { "en": "Apa Itu Boiler Feed Water Pump?", "id": "Pompa Air Umpan Tekanan Tinggi." },
  { "en": "Apa Itu Steam Turbine Governor Valve?", "id": "Katup Pengatur Aliran Uap Masuk." },
  { "en": "Apa Itu Condenser Vacuum Pressure Level?", "id": "Tekanan Hampa Udara Ruang Kondensor." },
  { "en": "Apa Itu Electrostatic Precipitator Dust Collector?", "id": "Penangkap Debu Listrik Tegangan Tinggi." },
  { "en": "Apa Itu Flue Gas Desulfurization System?", "id": "Penyerap Sulfur Dari Gas Buang." },
  { "en": "Apa Itu SCADA System Master Station?", "id": "Pusat Kontrol Dan Pengumpulan Data." },
  { "en": "Apa Itu Remote Terminal Unit SCADA?", "id": "Perangkat Keras Pengumpul Data Lapangan." },
  { "en": "Apa Itu Human Machine Interface Display?", "id": "Layar Grafis Operator Sistem Kontrol." },
  { "en": "Apa Itu Modbus Communication Protocol RTU?", "id": "Protokol Serial Standar Industri Terbuka." },
  { "en": "Apa Itu DNP3 Communication Protocol Grid?", "id": "Protokol Telemetri Standar Utilitas Listrik." },
  { "en": "Apa Itu IEC 61850 Communication Standard?", "id": "Standar Komunikasi Digital Gardu Induk." },
  { "en": "Apa Itu GOOSE Message IEC 61850?", "id": "Sinyal Cepat Antar Relay Proteksi." },
  { "en": "Apa Itu Ethernet Switch Managed Type?", "id": "Switch Jaringan Yang Bisa Dikonfigurasi." },
  { "en": "Apa Itu Cyber Security Firewall Device?", "id": "Perangkat Keamanan Jaringan Dari Serangan." },
  { "en": "Apa Itu Time Synchronization GPS Clock?", "id": "Sinkronisasi Waktu Menggunakan Satelit GPS." },
  { "en": "Apa Itu Relay Differential Protection Zone?", "id": "Zona Proteksi Antara Trafo Arus." },
  { "en": "Apa Itu Relay Distance Protection Zone?", "id": "Zona Proteksi Berdasarkan Impedansi Kabel." },
  { "en": "Apa Itu Relay Overcurrent Protection Curve?", "id": "Kurva Waktu Trip Terhadap Arus." },
  { "en": "Apa Itu Relay Earth Fault Protection?", "id": "Proteksi Gangguan Fasa Ke Tanah." },
  { "en": "Apa Itu Relay Negative Sequence Protection?", "id": "Proteksi Beban Tidak Seimbang Generator." },
  { "en": "Apa Itu Relay Reverse Power Protection?", "id": "Mencegah Daya Balik Ke Generator." },
  { "en": "Apa Itu Relay Loss Of Excitation?", "id": "Proteksi Hilang Medan Magnet Rotor." },
  { "en": "Apa Itu Relay Under Frequency Protection?", "id": "Lepas Beban Saat Frekuensi Turun." },
  { "en": "Apa Itu Relay Over Voltage Protection?", "id": "Mencegah Kerusakan Isolasi Tegangan Tinggi." },
  { "en": "Apa Itu Circuit Breaker Failure Protection?", "id": "Cadangan Jika Breaker Gagal Buka." },
  { "en": "Apa Itu Auto Recloser Operation Sequence?", "id": "Urutan Buka Tutup Otomatis Breaker." },
  { "en": "Apa Itu Busbar Protection Check Zone?", "id": "Zona Verifikasi Keseluruhan Rel Daya." },
  { "en": "Apa Itu Current Transformer Saturation Point?", "id": "Titik Jenuh Inti Magnetik CT." },
  { "en": "Apa Itu Potential Transformer Fuse Failure?", "id": "Deteksi Sekring Trafo Tegangan Putus." },
  { "en": "Apa Itu Capacitor Bank Inrush Reactor?", "id": "Pembatas Arus Masuk Kapasitor Bank." },
  { "en": "Apa Itu Power Factor Regulator Controller?", "id": "Alat Pengatur Langkah Kapasitor Otomatis." },
  { "en": "Apa Itu Harmonic Filter Passive Type?", "id": "Rangkaian LC Meredam Frekuensi Tertentu." },
  { "en": "Apa Itu Active Harmonic Filter Function?", "id": "Inverter Penyuntik Arus Anti Harmonisa." },
  { "en": "Apa Itu Grounding Grid Mesh Design?", "id": "Desain Jaring Kawat Tembaga Tanah." },
  { "en": "Apa Itu Step Voltage Danger Limit?", "id": "Batas Aman Tegangan Antara Kaki." },
  { "en": "Apa Itu Touch Voltage Danger Limit?", "id": "Batas Aman Tegangan Sentuh Tangan." },
  { "en": "Apa Itu Soil Resistivity Test Method?", "id": "Pengukuran Tahanan Jenis Tanah Lokasi." },
  { "en": "Apa Itu Lightning Protection Rolling Sphere?", "id": "Metode Desain Radius Proteksi Petir." },
  { "en": "Apa Itu Surge Arrester Discharge Current?", "id": "Arus Buang Maksimum Saat Petir." },
  { "en": "Apa Itu Insulation Resistance Test Megger?", "id": "Uji Tahanan Isolasi Tegangan DC." },
  { "en": "Apa Itu Polarization Index Test Result?", "id": "Rasio Megger 10 Menit 1 Menit." },
  { "en": "Apa Itu Tan Delta Test Insulation?", "id": "Uji Faktor Daya Rugi Dielektrik." },
  { "en": "Apa Itu Partial Discharge Test Cable?", "id": "Deteksi Peluahan Listrik Isolasi Kabel." },
  { "en": "Apa Itu VLF Hipot Test Cable?", "id": "Uji Kabel Tegangan Frekuensi Rendah." },
  { "en": "Apa Itu Tahanan Kontak Breaker?", "id": "Nilai Resistansi Pertemuan Kontak Utama." },
  { "en": "Apa Itu Vacuum Interrupter Bottle?", "id": "Tabung Hampa Udara Pemutus Arus." },
  { "en": "Apa Itu Contact Erosion Limit?", "id": "Batas Aus Kontak Boleh Dipakai." },
  { "en": "Apa Itu Spring Charging Motor?", "id": "Motor Pengisi Energi Pegas Breaker." },
  { "en": "Apa Itu Minimum Oil Breaker?", "id": "Pemutus Menggunakan Sedikit Minyak Isolasi." },
  { "en": "Apa Itu Air Blast Breaker?", "id": "Pemutus Menggunakan Udara Bertekanan Tinggi." },
  { "en": "Apa Itu Sulphur Hexafluoride Gas?", "id": "Gas Isolasi Dan Pemadam Busur." },
  { "en": "Apa Itu Gas Puffing Principle?", "id": "Prinsip Tiupan Gas Padamkan Busur." },
  { "en": "Apa Itu Thermal Magnetic Trip?", "id": "Mekanisme Trip Panas Dan Magnet." },
  { "en": "Apa Itu Electronic Trip Unit?", "id": "Unit Trip Berbasis Mikroprosesor Canggih." },
  { "en": "Apa Itu Shunt Trip Coil?", "id": "Kumparan Pemicu Buka Jarak Jauh." },
  { "en": "Apa Itu Motor Operator Mechanism?", "id": "Mekanisme Penggerak Motor Untuk Breaker." },
  { "en": "Apa Itu Rotary Handle Mechanism?", "id": "Tuas Putar Pengoperasian Breaker Molded." },
  { "en": "Apa Itu Drawout Circuit Breaker?", "id": "Breaker Bisa Ditarik Keluar Rak." },
  { "en": "Apa Itu Fixed Circuit Breaker?", "id": "Breaker Terpasang Tetap Pada Panel." },
  { "en": "Apa Itu Racking In Position?", "id": "Posisi Breaker Terhubung Dengan Busbar." },
  { "en": "Apa Itu Test Position Breaker?", "id": "Posisi Uji Sirkuit Kontrol Breaker." },
  { "en": "Apa Itu Disconnected Position Breaker?", "id": "Posisi Breaker Terpisah Dari Busbar." },
  { "en": "Apa Itu Safety Shutter Panel?", "id": "Penutup Otomatis Terminal Saat Drawout." },
  { "en": "Apa Itu Arc Chute Chamber?", "id": "Ruang Pemecah Dan Pendingin Busur." },
  { "en": "Apa Itu Moving Contact Breaker?", "id": "Kontak Yang Bergerak Membuka Menutup." },
  { "en": "Apa Itu Fixed Contact Breaker?", "id": "Kontak Diam Pasangan Moving Contact." },
  { "en": "Apa Itu Finger Cluster Contact?", "id": "Kontak Bentuk Jari Mencengkram Terminal." },
  { "en": "Apa Itu Tulip Contact Design?", "id": "Kontak Bentuk Bunga Tulip Kuncup." },
  { "en": "Apa Itu Busbar Support Insulator?", "id": "Isolator Penyangga Rel Tembaga Panel." },
  { "en": "Apa Itu Heat Shrink Busbar?", "id": "Selongsong Isolasi Bakar Busbar Tembaga." },
  { "en": "Apa Itu Phase Barrier Insulation?", "id": "Sekat Isolasi Antar Fasa Breaker." },
  { "en": "Apa Itu Earthing Switch Panel?", "id": "Saklar Pentanahan Kabel Output Panel." },
  { "en": "Apa Itu Cable Entry Gland?", "id": "Penyekat Kabel Masuk Ke Panel." },
  { "en": "Apa Itu Anti-Condensation Heater?", "id": "Pemanas Mencegah Embun Dalam Panel." },
  { "en": "Apa Itu Thermostat Control Heater?", "id": "Pengatur Suhu Otomatis Pemanas Panel." },
  { "en": "Apa Itu Hygrostat Control Heater?", "id": "Pengatur Kelembaban Otomatis Pemanas Panel." },
  { "en": "Apa Itu Ventilation Fan Filter?", "id": "Penyaring Debu Kipas Pendingin Panel." },
  { "en": "Apa Itu IP Protection Rating?", "id": "Tingkat Perlindungan Debu Dan Air." },
  { "en": "Apa Itu IK Impact Rating?", "id": "Tingkat Ketahanan Terhadap Benturan Mekanis." },
  { "en": "Apa Itu Form Separation Panel?", "id": "Standar Pemisahan Ruang Dalam Panel." },
  { "en": "Apa Itu Form 4b Panel?", "id": "Pemisahan Busbar, Kabel, Dan Breaker." },
  { "en": "Apa Itu Withdrawable Module Unit?", "id": "Modul Laci Yang Bisa Ditarik." },
  { "en": "Apa Itu Motor Control Center?", "id": "Panel Pusat Pengendalian Motor Listrik." },
  { "en": "Apa Itu Variable Speed Drive?", "id": "Pengendali Kecepatan Motor Frekuensi Variabel." },
  { "en": "Apa Itu Soft Starter Motor?", "id": "Pengendali Start Motor Tegangan Bertahap." },
  { "en": "Apa Itu Direct On Line?", "id": "Metode Start Motor Langsung Jaringan." },
  { "en": "Apa Itu Star Delta Starter?", "id": "Start Motor Pindah Hubungan Kumparan." },
  { "en": "Apa Itu Contactor AC3 Rating?", "id": "Kategori Beban Motor Induksi Sangkar." },
  { "en": "Apa Itu Thermal Overload Relay?", "id": "Proteksi Beban Lebih Bimetal Panas." },
  { "en": "Apa Itu Electronic Overload Relay?", "id": "Proteksi Beban Lebih Berbasis Elektronik." },
  { "en": "Apa Itu Phase Failure Relay?", "id": "Proteksi Hilang Salah Satu Fasa." },
  { "en": "Apa Itu Earth Leakage Relay?", "id": "Proteksi Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Core Balance CT?", "id": "Trafo Arus Lingkaran Tiga Fasa." },
  { "en": "Apa Itu Pilot Lamp Indicator?", "id": "Lampu Tanda Status Operasi Panel." },
  { "en": "Apa Itu Push Button Switch?", "id": "Tombol Tekan Operasi Manual Panel." },
  { "en": "Apa Itu Selector Switch Panel?", "id": "Saklar Pilih Mode Operasi Panel." },
  { "en": "Apa Itu Emergency Stop Button?", "id": "Tombol Berhenti Darurat Warna Merah." },
  { "en": "Apa Itu Terminal Block Panel?", "id": "Terminal Sambungan Kabel Kontrol Panel." },
  { "en": "Apa Itu Wiring Duct Panel?", "id": "Jalur Kabel Rapi Dalam Panel." },
  { "en": "Apa Itu Ferrule Numbering Wire?", "id": "Nomor Penanda Ujung Kabel Kontrol." },
  { "en": "Apa Itu Mimic Diagram Panel?", "id": "Gambar Alur Daya Depan Panel." },
  { "en": "Apa Itu Annunciator Alarm Panel?", "id": "Panel Indikator Alarm Gangguan Visual." },
  { "en": "Apa Itu Digital Power Meter?", "id": "Alat Ukur Daya Listrik Digital." },
  { "en": "Apa Itu Analogue Ampere Meter?", "id": "Alat Ukur Arus Jarum Penunjuk." },
  { "en": "Apa Itu Analogue Volt Meter?", "id": "Alat Ukur Tegangan Jarum Penunjuk." },
  { "en": "Apa Itu Frequency Meter Panel?", "id": "Alat Ukur Frekuensi Listrik Panel." },
  { "en": "Apa Itu Power Factor Meter?", "id": "Alat Ukur Faktor Daya Panel." },
  { "en": "Apa Itu Hour Meter Counter?", "id": "Penghitung Jam Kerja Operasi Mesin." },
  { "en": "Apa Itu KWH Meter Panel?", "id": "Pengukur Energi Listrik Terpakai Panel." },
  { "en": "Apa Itu CT Ratio Metering?", "id": "Rasio Arus Untuk Alat Ukur." },
  { "en": "Apa Itu PT Ratio Metering?", "id": "Rasio Tegangan Untuk Alat Ukur." },
  { "en": "Apa Itu Test Terminal Block?", "id": "Terminal Uji Arus Tegangan Meter." },
  { "en": "Apa Itu Voltage Select Switch?", "id": "Saklar Pilih Tampilan Tegangan Fasa." },
  { "en": "Apa Itu Ampere Select Switch?", "id": "Saklar Pilih Tampilan Arus Fasa." },
  { "en": "Apa Itu Capacitor Bank Panel?", "id": "Panel Perbaikan Faktor Daya Otomatis." },
  { "en": "Apa Itu Power Factor Controller?", "id": "Otak Pengendali Langkah Kapasitor Bank." },
  { "en": "Apa Itu Capacitor Duty Contactor?", "id": "Kontaktor Khusus Beban Kapasitif Resistif." },
  { "en": "Apa Itu Inrush Limiting Inductor?", "id": "Induktor Pembatas Arus Awal Kapasitor." },
  { "en": "Apa Itu Harmonic Detuned Reactor?", "id": "Reaktor Penahan Resonansi Harmonisa Kapasitor." },
  { "en": "Apa Itu Discharge Resistor Capacitor?", "id": "Resistor Pembuang Muatan Sisa Kapasitor." },
  { "en": "Apa Itu Step Capacitor Bank?", "id": "Langkah Besaran Daya Reaktif Kapasitor." },
  { "en": "Apa Itu Automatic Transfer Switch?", "id": "Saklar Pindah Sumber Daya Otomatis." },
  { "en": "Apa Itu Automatic Main Failure?", "id": "Modul Start Genset Saat Padam." },
  { "en": "Apa Itu Generator Control Unit?", "id": "Modul Kontrol Dan Proteksi Genset." },
  { "en": "Apa Itu Battery Charger Panel?", "id": "Pengisi Baterai Otomatis Dalam Panel." },
  { "en": "Apa Itu DC Power Supply?", "id": "Sumber Daya Arus Searah Kontrol." },
  { "en": "Apa Itu Control Fuse Panel?", "id": "Sekring Pengaman Rangkaian Kontrol Panel." },
  { "en": "Apa Itu Control MCB Panel?", "id": "Pemutus Pengaman Rangkaian Kontrol Panel." },
  { "en": "Apa Itu Relay Base Socket?", "id": "Dudukan Pasang Relay Bantu Panel." },
  { "en": "Apa Itu Timer Delay Relay?", "id": "Relay Penunda Waktu Operasi Panel." },
  { "en": "Apa Itu Latching Relay Panel?", "id": "Relay Pengunci Posisi Kontak Mekanis." },
  { "en": "Apa Itu Auxiliary Contact Block?", "id": "Blok Kontak Bantu Tambahan Kontaktor." },
  { "en": "Apa Itu Mechanical Interlock Device?", "id": "Pengunci Mekanis Antara Dua Kontaktor." },
  { "en": "Apa Itu Busbar Chamber Box?", "id": "Kotak Distribusi Busbar Tembaga Utama." },
  { "en": "Apa Itu Neutral Earth Link?", "id": "Penghubung Netral Dan Ground Panel." },
  { "en": "Apa Itu Bonding Jumper Wire?", "id": "Kabel Penghubung Grounding Pintu Panel." },
  { "en": "Apa Itu Panel Mounting Plate?", "id": "Plat Dasar Pemasangan Komponen Panel." },
  { "en": "Apa Itu Door Hinge Panel?", "id": "Engsel Pintu Panel Listrik Besi." },
  { "en": "Apa Itu Panel Lock Handle?", "id": "Tuas Kunci Pintu Panel Listrik." },
  { "en": "Apa Itu Heat Shrink Breakout?", "id": "Segel Percabangan Kabel Tiga Fasa." },
  { "en": "Apa Itu Cable End Cap?", "id": "Penutup Ujung Kabel Tahan Air." },
  { "en": "Apa Itu Stress Relief Mastic?", "id": "Dempul Perata Medan Listrik Sambungan." },
  { "en": "Apa Itu Tinned Copper Braid?", "id": "Anyaman Tembaga Pelindung Grounding Fleksibel." },
  { "en": "Apa Itu Constant Force Spring?", "id": "Pegas Pengikat Grounding Tanpa Solder." },
  { "en": "Apa Itu Shear Bolt Connector?", "id": "Baut Putus Saat Torsi Tepat." },
  { "en": "Apa Itu Torque Shear Lug?", "id": "Sepatu Kabel Baut Putus Otomatis." },
  { "en": "Apa Itu Cold Shrink Tube?", "id": "Selongsong Karet Susut Tanpa Api." },
  { "en": "Apa Itu Zipper Tubing?", "id": "Selongsong Kabel Dengan Resleting Penutup." },
  { "en": "Apa Itu Cable Gland Shroud?", "id": "Karet Pelindung Luar Klem Kabel." },
  { "en": "Apa Itu Armoured Cable Gland?", "id": "Klem Khusus Kabel Perisai Kawat Baja." },
  { "en": "Apa Itu Unarmoured Cable Gland?", "id": "Klem Kabel Tanpa Perisai Baja." },
  { "en": "Apa Itu Explosive Atmosphere Gland?", "id": "Klem Kabel Tahan Ledakan Gas." },
  { "en": "Apa Itu Cable Cleat Trefoil?", "id": "Klem Penahan Tiga Kabel Segitiga." },
  { "en": "Apa Itu Short Circuit Force?", "id": "Gaya Tolak Kabel Saat Gangguan." },
  { "en": "Apa Itu Fire Stop Pillow?", "id": "Bantal Tahan Api Penutup Lubang." },
  { "en": "Apa Itu Fire Stop Foam?", "id": "Busa Penutup Celah Anti Api." },
  { "en": "Apa Itu Busbar Insulating Shroud?", "id": "Penutup Sambungan Busbar Plastik Fleksibel." },
  { "en": "Apa Itu Phase Indicator Tape?", "id": "Isolasi Warna Penanda Fasa Kabel." },
  { "en": "Apa Itu Wire Marker Tube?", "id": "Selongsong Nomor Identitas Kabel Kontrol." },
  { "en": "Apa Itu Spiral Cable Wrap?", "id": "Pelindung Kabel Spiral Plastik Fleksibel." },
  { "en": "Apa Itu Braided Sleeving?", "id": "Selongsong Anyaman Pelindung Gesekan Kabel." },
  { "en": "Apa Itu Heat Shrink Label?", "id": "Label Kabel Cetak Tahan Panas." },
  { "en": "Apa Itu Cable Tie Stainless?", "id": "Pengikat Kabel Baja Tahan Karat." },
  { "en": "Apa Itu Cable Tray Cover?", "id": "Penutup Atas Rak Kabel Logam." },
  { "en": "Apa Itu Cable Tray Divider?", "id": "Penyekat Pemisah Kabel Dalam Rak." },
  { "en": "Apa Itu Grounding Busbar Panel?", "id": "Rel Tembaga Terminal Kabel Tanah." },
  { "en": "Apa Itu Neutral Link Panel?", "id": "Terminal Penghubung Kabel Netral Panel." },
  { "en": "Apa Itu Distribution Block?", "id": "Terminal Pembagi Daya Arus Besar." },
  { "en": "Apa Itu Step Down Chopper?", "id": "Konverter DC Penurun Tegangan Output." },
  { "en": "Apa Itu Step Up Chopper?", "id": "Konverter DC Penaik Tegangan Output." },
  { "en": "Apa Itu Buck-Boost Chopper?", "id": "Konverter DC Bisa Naik Turun." },
  { "en": "Apa Itu Duty Ratio Chopper?", "id": "Perbandingan Waktu On Dan Periode." },
  { "en": "Apa Itu Voltage Commutation?", "id": "Mematikan Thyristor Dengan Tegangan Balik." },
  { "en": "Apa Itu Current Commutation?", "id": "Mematikan Thyristor Dengan Arus Lawan." },
  { "en": "Apa Itu Load Commutation?", "id": "Komutasi Alami Oleh Sifat Beban." },
  { "en": "Apa Itu Line Commutation?", "id": "Komutasi Alami Oleh Tegangan Jaringan." },
  { "en": "Apa Itu Forced Commutation?", "id": "Komutasi Paksa Menggunakan Komponen Tambahan." },
  { "en": "Apa Itu Freewheeling Action?", "id": "Arus Beban Induktif Terus Mengalir." },
  { "en": "Apa Itu Controlled Rectifier?", "id": "Penyearah Terkendali Menggunakan Thyristor SCR." },
  { "en": "Apa Itu Half Controlled Rectifier?", "id": "Penyearah Gabungan Thyristor Dan Dioda." },
  { "en": "Apa Itu Fully Controlled Rectifier?", "id": "Penyearah Menggunakan Thyristor Sepenuhnya." },
  { "en": "Apa Itu Firing Angle Alpha?", "id": "Sudut Tunda Penyalaan Gerbang Thyristor." },
  { "en": "Apa Itu Extinction Angle Beta?", "id": "Sudut Saat Arus Berhenti Mengalir." },
  { "en": "Apa Itu Overlap Angle Mu?", "id": "Sudut Peralihan Arus Antar Fasa." },
  { "en": "Apa Itu Inverter Voltage Control?", "id": "Mengatur Tegangan Output Inverter AC." },
  { "en": "Apa Itu Pulse Width Modulation?", "id": "Teknik Modulasi Lebar Pulsa Sinyal." },
  { "en": "Apa Itu Sinusoidal PWM?", "id": "PWM Referensi Gelombang Sinus Segitiga." },
  { "en": "Apa Itu Hysteresis Current Control?", "id": "Kontrol Arus Dalam Batas Pita." },
  { "en": "Apa Itu Harmonics Reduction?", "id": "Mengurangi Kandungan Frekuensi Tinggi Sinyal." },
  { "en": "Apa Itu Selective Harmonic Elimination?", "id": "Menghapus Harmonisa Tertentu Lewat PWM." },
  { "en": "Apa Itu Multilevel Inverter Topology?", "id": "Inverter Output Tegangan Banyak Tingkat." },
  { "en": "Apa Itu Neutral Point Clamped?", "id": "Topologi Inverter Tiga Level Umum." },
  { "en": "Apa Itu Flying Capacitor Topology?", "id": "Topologi Inverter Menggunakan Kapasitor Mengambang." },
  { "en": "Apa Itu Cascaded H-Bridge?", "id": "Topologi Inverter Seri Modul Jembatan." },
  { "en": "Apa Itu Z-Source Inverter?", "id": "Inverter Dengan Impedansi Jaringan Z." },
  { "en": "Apa Itu Resonant Converter?", "id": "Konverter Switching Saat Gelombang Nol." },
  { "en": "Apa Itu Series Resonant Converter?", "id": "Rangkaian Resonansi Seri Dengan Beban." },
  { "en": "Apa Itu Parallel Resonant Converter?", "id": "Rangkaian Resonansi Paralel Dengan Beban." },
  { "en": "Apa Itu Zero Voltage Switching?", "id": "Saklar On Saat Tegangan Nol." },
  { "en": "Apa Itu Zero Current Switching?", "id": "Saklar Off Saat Arus Nol." },
  { "en": "Apa Itu Soft Switching?", "id": "Switching Tanpa Rugi Daya Besar." },
  { "en": "Apa Itu Hard Switching?", "id": "Switching Paksa Tegangan Arus Tinggi." },
  { "en": "Apa Itu Snubber Circuit Design?", "id": "Desain Peredam Lonjakan Tegangan Saklar." },
  { "en": "Apa Itu Gate Drive Optocoupler?", "id": "Pengisolasi Sinyal Pemicu Gerbang Saklar." },
  { "en": "Apa Itu Bootstrap Circuit?", "id": "Rangkaian Suplai Tegangan Gate Atas." },
  { "en": "Apa Itu Dead Time Control?", "id": "Jeda Waktu Mencegah Hubung Singkat." },
  { "en": "Apa Itu Shoot Through Fault?", "id": "Hubung Singkat Vertikal Kaki Inverter." },
  { "en": "Apa Itu Desaturation Protection?", "id": "Proteksi IGBT Dari Arus Lebih." },
  { "en": "Apa Itu Thermal Runaway IGBT?", "id": "Kerusakan IGBT Akibat Panas Berlebih." },
  { "en": "Apa Itu Miller Effect IGBT?", "id": "Kapasitansi Parasitik Mengganggu Switching Gate." },
  { "en": "Apa Itu Reverse Recovery Current?", "id": "Arus Balik Dioda Saat Mati." },
  { "en": "Apa Itu Forward Voltage Drop?", "id": "Tegangan Jatuh Saat Arus Mengalir." },
  { "en": "Apa Itu Leakage Current Semiconductor?", "id": "Arus Bocor Saat Saklar Mati." },
  { "en": "Apa Itu Blocking Voltage Rating?", "id": "Tegangan Maksimum Tahan Saklar Mati." },
  { "en": "Apa Itu On-State Resistance Rds?", "id": "Hambatan MOSFET Saat Kondisi On." },
  { "en": "Apa Itu Junction Temperature Tj?", "id": "Suhu Sambungan Semikonduktor Silikon." },
  { "en": "Apa Itu Thermal Resistance Rth?", "id": "Hambatan Aliran Panas Ke Pendingin." },
  { "en": "Apa Itu Heatsink Cooling Fan?", "id": "Kipas Pendingin Sirip Aluminium Heatsink." },
  { "en": "Apa Itu Liquid Cold Plate?", "id": "Pendingin Cair Tempel Modul Daya." },
  { "en": "Apa Itu Phase Phase Fault?", "id": "Gangguan Hubung Singkat Antar Fasa." },
  { "en": "Apa Itu Phase Ground Fault?", "id": "Gangguan Hubung Singkat Fasa Tanah." },
  { "en": "Apa Itu Three Phase Fault?", "id": "Gangguan Hubung Singkat Tiga Fasa." },
  { "en": "Apa Itu Double Earth Fault?", "id": "Dua Fasa Terhubung Ke Tanah." },
  { "en": "Apa Itu Arc Resistance?", "id": "Tahanan Listrik Busur Api Gangguan." },
  { "en": "Apa Itu Fault Impedance?", "id": "Total Impedansi Jalur Arus Gangguan." },
  { "en": "Apa Itu Source Impedance?", "id": "Impedansi Sistem Di Belakang Relay." },
  { "en": "Apa Itu Load Impedance?", "id": "Impedansi Beban Normal Sistem." },
  { "en": "Apa Itu Relay Reach?", "id": "Jarak Jangkauan Deteksi Relay Jarak." },
  { "en": "Apa Itu Overreach Relay?", "id": "Jangkauan Relay Melebihi Panjang Saluran." },
  { "en": "Apa Itu Underreach Relay?", "id": "Jangkauan Relay Kurang Dari Saluran." },
  { "en": "Apa Itu Zone 1 Extension?", "id": "Memperluas Zona 1 Sementara Waktu." },
  { "en": "Apa Itu Power Swing Locus?", "id": "Lintasan Impedansi Saat Ayunan Daya." },
  { "en": "Apa Itu Blinder Impedance?", "id": "Garis Batas Area Trip Relay." },
  { "en": "Apa Itu Load Encroachment Logic?", "id": "Mencegah Relay Trip Akibat Beban." },
  { "en": "Apa Itu Voltage Memory?", "id": "Menyimpan Nilai Tegangan Sebelum Gangguan." },
  { "en": "Apa Itu Cross Polarization?", "id": "Menggunakan Fasa Sehat Referensi Arah." },
  { "en": "Apa Itu Self Polarization?", "id": "Menggunakan Fasa Gangguan Referensi Arah." },
  { "en": "Apa Itu Memory Voltage Action?", "id": "Mempertahankan Arah Trip Saat Tegangan Nol." },
  { "en": "Apa Itu VT Fuse Fail Blocking?", "id": "Mencegah Trip Salah Saat Sekring Putus." },
  { "en": "Apa Itu Power Swing Detection?", "id": "Mendeteksi Ayunan Daya Melalui Impedansi." },
  { "en": "Apa Itu Out Of Step Tripping?", "id": "Memisahkan Sistem Saat Lepas Sinkron." },
  { "en": "Apa Itu Frequency Trend Element?", "id": "Elemen Prediksi Perubahan Frekuensi Cepat." },
  { "en": "Apa Itu Under Frequency Load Shedding?", "id": "Melepas Beban Saat Frekuensi Turun." },
  { "en": "Apa Itu Islanding Detection Method?", "id": "Metode Deteksi Operasi Terpisah Grid." },
  { "en": "Apa Itu Vector Shift Detection?", "id": "Deteksi Pergeseran Sudut Fasa Tegangan." },
  { "en": "Apa Itu ROCOF Detection?", "id": "Deteksi Laju Perubahan Frekuensi Cepat." },
  { "en": "Apa Itu Intertrip Signal?", "id": "Sinyal Trip Kiriman Dari Gardu Lain." },
  { "en": "Apa Itu Permissive Transfer Trip?", "id": "Trip Membutuhkan Izin Relay Lokal." },
  { "en": "Apa Itu Blocking Signal?", "id": "Sinyal Mencegah Relay Lokal Trip." },
  { "en": "Apa Itu Unblocking Signal?", "id": "Sinyal Mengizinkan Trip Saat Channel Rusak." },
  { "en": "Apa Itu Current Differential Principle?", "id": "Hukum Kirchhoff Arus Pada Zona Proteksi." },
  { "en": "Apa Itu Charge Comparison Protection?", "id": "Membandingkan Muatan Fasa Tiap Siklus." },
  { "en": "Apa Itu Directional Comparison Protection?", "id": "Membandingkan Arah Gangguan Kedua Ujung." },
  { "en": "Apa Itu Segregated Phase Comparison?", "id": "Perbandingan Fasa Tiap Kabel Terpisah." },
  { "en": "Apa Itu Composite Phase Comparison?", "id": "Perbandingan Kombinasi Sinyal Urutan Fasa." },
  { "en": "Apa Itu Propagation Delay Time?", "id": "Waktu Tempuh Sinyal Komunikasi Proteksi." },
  { "en": "Apa Itu Channel asymmetry?", "id": "Beda Waktu Kirim Dan Terima Sinyal." },
  { "en": "Apa Itu GPS Synchronization?", "id": "Sinkronisasi Waktu Menggunakan Satelit Global." },
  { "en": "Apa Itu Ping Pong Method?", "id": "Metode Sinkronisasi Waktu Tanpa GPS." },
  { "en": "Apa Itu Bit Error Rate?", "id": "Tingkat Kesalahan Data Komunikasi Digital." },
  { "en": "Apa Itu Signal To Noise Ratio?", "id": "Perbandingan Kekuatan Sinyal Dan Noise." },
  { "en": "Apa Itu Fiber Optic Interface?", "id": "Antarmuka Komunikasi Cahaya Relay Proteksi." },
  { "en": "Apa Itu Multiplexer Protection Channel?", "id": "Penggabung Banyak Sinyal Satu Saluran." },
  { "en": "Apa Itu C37.94 Standard?", "id": "Standar Antarmuka Optik Teleproteksi IEEE." },
  { "en": "Apa Itu G.703 Standard?", "id": "Standar Fisik Dan Elektrik Transmisi Digital." },
  { "en": "Apa Itu Pilot Wire Differential?", "id": "Proteksi Diferensial Kabel Tembaga Khusus." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
