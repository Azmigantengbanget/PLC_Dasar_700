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

  { "en": "Apa itu PLC?", "id": "Kontroler logika terprogram industri." },
  { "en": "Kepanjangan PLC?", "id": "Programmable Logic Controller." },
  { "en": "Fungsi utama PLC?", "id": "Mengotomatiskan proses industri." },
  { "en": "Bagian utama PLC?", "id": "CPU, modul I/O, power supply." },
  { "en": "Otak dari PLC?", "id": "Central Processing Unit (CPU)." },
  { "en": "Fungsi modul input?", "id": "Menerima sinyal dari sensor." },
  { "en": "Fungsi modul output?", "id": "Mengirim sinyal ke aktuator." },
  { "en": "Sumber daya PLC?", "id": "Power supply unit." },
  { "en": "Bahasa program PLC populer?", "id": "Ladder Diagram (LD)." },
  { "en": "Apa itu sensor?", "id": "Perangkat pendeteksi sinyal fisik." },
  { "en": "Apa itu aktuator?", "id": "Perangkat penggerak mesin." },
  { "en": "Contoh perangkat input?", "id": "Tombol, saklar, sensor." },
  { "en": "Contoh perangkat output?", "id": "Lampu, motor, solenoid." },
  { "en": "Apa itu 'scan cycle'?", "id": "Proses eksekusi program PLC." },
  { "en": "Tahap pertama scan cycle?", "id": "Membaca status semua input." },
  { "en": "Tahap kedua scan cycle?", "id": "Mengeksekusi program logika." },
  { "en": "Tahap ketiga scan cycle?", "id": "Memperbarui status semua output." },
  { "en": "Apa itu sinyal digital?", "id": "Sinyal dengan dua kondisi (ON/OFF)." },
  { "en": "Apa itu sinyal analog?", "id": "Sinyal dengan rentang nilai." },
  { "en": "Contoh sensor digital?", "id": "Proximity switch, limit switch." },
  { "en": "Contoh sensor analog?", "id": "Sensor suhu, sensor tekanan." },
  { "en": "Apa itu 'rung'?", "id": "Satu baris logika pada diagram ladder." },
  { "en": "Simbol kontak NO?", "id": "Normally Open, terbuka normal." },
  { "en": "Simbol kontak NC?", "id": "Normally Closed, tertutup normal." },
  { "en": "Simbol output di ladder?", "id": "Coil atau koil." },
  { "en": "Fungsi Timer di PLC?", "id": "Menghitung durasi waktu tertentu." },
  { "en": "Fungsi Counter di PLC?", "id": "Menghitung jumlah kejadian/event." },
  { "en": "Tipe data PLC dasar?", "id": "Boolean (BIT), integer, float." },
  { "en": "Apa itu HMI?", "id": "Human Machine Interface." },
  { "en": "Fungsi HMI?", "id": "Antarmuka operator dengan mesin." },
  { "en": "Apa itu 'forcing'?", "id": "Memaksa status I/O secara manual." },
  { "en": "Mode operasi PLC?", "id": "RUN, PROG, dan REM." },
  { "en": "Apa fungsi mode RUN?", "id": "PLC mengeksekusi program." },
  { "en": "Apa fungsi mode PROG?", "id": "Memasukkan atau mengedit program." },
  { "en": "PLC modular vs compact?", "id": "Modular bisa diperluas, compact tidak." },
  { "en": "Apa itu 'addressing'?", "id": "Alamat unik untuk setiap I/O." },
  { "en": "Contoh alamat input?", "id": "I:0/0, %IX0.0." },
  { "en": "Contoh alamat output?", "id": "O:0/1, %QX0.1." },
  { "en": "Siapa penemu PLC?", "id": "Dick Morley pada tahun 1968." },
  { "en": "Perusahaan pertama pengguna PLC?", "id": "General Motors (GM)." },
  { "en": "Apa itu logika AND?", "id": "Output ON jika semua input ON." },
  { "en": "Apa itu logika OR?", "id": "Output ON jika salah satu input ON." },
  { "en": "Apa itu logika NOT?", "id": "Membalik kondisi input." },
  { "en": "Unit memori terkecil?", "id": "Bit (0 atau 1)." },
  { "en": "Kumpulan 8 bit?", "id": "Satu Byte." },
  { "en": "Kumpulan 16 bit?", "id": "Satu Word." },
  { "en": "Jenis memori di PLC?", "id": "RAM, ROM, EEPROM." },
  { "en": "Fungsi baterai cadangan?", "id": "Menyimpan program saat listrik mati." },
  { "en": "Bahasa program selain ladder?", "id": "FBD, SFC, ST, IL." },
  { "en": "Kepanjangan FBD?", "id": "Function Block Diagram." },
  { "en": "Kepanjangan SFC?", "id": "Sequential Function Chart." },
  { "en": "Kepanjangan ST?", "id": "Structured Text." },
  { "en": "Kepanjangan IL?", "id": "Instruction List." },
  { "en": "Apa itu 'latching'?", "id": "Menahan output tetap ON." },
  { "en": "Bagaimana cara 'latching'?", "id": "Menggunakan kontak output paralel." },
  { "en": "Apa itu 'interlocking'?", "id": "Mencegah dua output aktif bersamaan." },
  { "en": "Mengapa perlu 'grounding'?", "id": "Untuk keamanan dan stabilitas sinyal." },
  { "en": "Apa itu 'noise' listrik?", "id": "Gangguan sinyal listrik yang tidak diinginkan." },
  { "en": "Penyebab 'noise' listrik?", "id": "Motor besar, frekuensi radio." },
  { "en": "Fungsi 'watchdog timer'?", "id": "Memantau kesalahan siklus scan." },
  { "en": "PLC menggantikan apa?", "id": "Panel kontrol berbasis relay." },
  { "en": "Keuntungan PLC dari relay?", "id": "Fleksibel, mudah diubah, andal." },
  { "en": "Apa itu 'sinking' input?", "id": "Modul input menerima arus." },
  { "en": "Apa itu 'sourcing' input?", "id": "Modul input memberi arus." },
  { "en": "Apa itu 'sinking' output?", "id": "Modul output menyerap arus." },
  { "en": "Apa itu 'sourcing' output?", "id": "Modul output memasok arus." },
  { "en": "Port komunikasi umum PLC?", "id": "Ethernet, Serial (RS-232/485)." },
  { "en": "Apa itu 'uploading' program?", "id": "Transfer program dari PLC ke PC." },
  { "en": "Apa itu 'downloading' program?", "id": "Transfer program dari PC ke PLC." },
  { "en": "Fungsi instruksi 'MOVE'?", "id": "Memindahkan nilai data." },
  { "en": "Fungsi instruksi komparasi?", "id": "Membandingkan dua nilai data." },
  { "en": "Contoh instruksi komparasi?", "id": "Sama dengan (EQU), lebih besar (GRT)." },
  { "en": "Apa itu I/O terdistribusi?", "id": "Modul I/O jauh dari CPU." },
  { "en": "Keuntungan I/O terdistribusi?", "id": "Menghemat biaya kabel panjang." },
  { "en": "Apa itu 'tag name'?", "id": "Nama deskriptif untuk alamat memori." },
  { "en": "Mengapa menggunakan komentar program?", "id": "Mempermudah pemahaman logika program." },
  { "en": "Apa itu 'firmware' PLC?", "id": "Sistem operasi internal PLC." },
  { "en": "Bisakah 'firmware' di-update?", "id": "Ya, untuk perbaikan atau fitur baru." },
  { "en": "Apa itu 'rack' PLC?", "id": "Kerangka untuk menampung modul PLC." },
  { "en": "Apa itu 'slot' PLC?", "id": "Posisi spesifik untuk modul di rack." },
  { "en": "Apa itu 'hot-swappable'?", "id": "Modul bisa diganti saat PLC ON." },
  { "en": "Apa itu 'retentive timer'?", "id": "Timer yang menyimpan nilai akumulasi." },
  { "en": "Apa itu 'non-retentive timer'?", "id": "Timer yang mereset nilai saat mati." },
  { "en": "Fungsi 'master control reset'?", "id": "Mematikan sekelompok output serentak." },
  { "en": "Apa itu 'one-shot'?", "id": "Instruksi yang aktif satu scan." },
  { "en": "Apa itu 'fault routine'?", "id": "Program yang berjalan saat ada eror." },
  { "en": "Apa itu PID control?", "id": "Kontrol loop tertutup presisi." },
  { "en": "Kepanjangan PID?", "id": "Proportional, Integral, Derivative." },
  { "en": "Merek PLC terkenal?", "id": "Siemens, Allen-Bradley, Omron." },
  { "en": "Apa itu SCADA?", "id": "Sistem kontrol dan akuisisi data." },
  { "en": "Hubungan PLC dan SCADA?", "id": "SCADA memvisualisasikan data dari PLC." },
  { "en": "Perbedaan PLC dan PC?", "id": "PLC lebih tangguh untuk industri." },
  { "en": "Apa itu 'online editing'?", "id": "Mengubah program saat PLC berjalan." },
  { "en": "Risiko 'online editing'?", "id": "Bisa menyebabkan mesin berhenti tak terduga." },
  { "en": "Apa itu simulasi PLC?", "id": "Menguji program tanpa hardware fisik." },
  { "en": "Mengapa simulasi penting?", "id": "Mengurangi risiko kesalahan di lapangan." },
  { "en": "Apa itu 'safety PLC'?", "id": "PLC untuk aplikasi keselamatan kritis." },
  { "en": "Warna 'safety PLC'?", "id": "Biasanya merah atau kuning." },
  { "en": "Apa itu 'troubleshooting'?", "id": "Proses mencari dan memperbaiki masalah." },
  { "en": "Indikator LED 'FAULT'?", "id": "Menandakan adanya kesalahan sistem." },
  { "en": "Indikator LED 'RUN'?", "id": "Menandakan PLC sedang mengeksekusi program." },
  { "en": "Apa itu 'cross reference'?", "id": "Mencari lokasi penggunaan suatu alamat." },
  { "en": "Fungsi 'cross reference'?", "id": "Memudahkan pelacakan dan debugging." },
  { "en": "Apa itu 'firmware corruption'?", "id": "Kerusakan pada sistem operasi PLC." },
  { "en": "Langkah pertama troubleshooting PLC?", "id": "Periksa indikator status LED." },
  { "en": "Apa itu modul output relay?", "id": "Output menggunakan kontak mekanis." },
  { "en": "Apa itu modul output transistor?", "id": "Output menggunakan komponen solid-state." },
  { "en": "Kelebihan output transistor?", "id": "Kecepatan switching sangat tinggi." },
  { "en": "Kelebihan output relay?", "id": "Bisa menangani tegangan AC/DC." },
  { "en": "Apa itu 'scan time'?", "id": "Waktu untuk satu siklus scan." },
  { "en": "Mengapa 'scan time' penting?", "id": "Mempengaruhi kecepatan respons sistem." },
  { "en": "Apa itu 'watchdog timer fault'?", "id": "Kesalahan jika scan time terlalu lama." },
  { "en": "Apa itu 'subroutine'?", "id": "Bagian program yang bisa dipanggil." },
  { "en": "Keuntungan menggunakan 'subroutine'?", "id": "Membuat program lebih terstruktur." },
  { "en": "Instruksi JSR berarti?", "id": "Jump to Subroutine." },
  { "en": "Instruksi RET berarti?", "id": "Return from Subroutine." },
  { "en": "Apa itu 'electrical noise immunity'?", "id": "Kemampuan PLC menahan gangguan listrik." },
  { "en": "Apa itu 'opto-isolator'?", "id": "Mengisolasi sirkuit input/output secara elektrik." },
  { "en": "Fungsi 'opto-isolator'?", "id": "Melindungi CPU dari lonjakan tegangan." },
  { "en": "Apa itu protokol komunikasi?", "id": "Aturan untuk pertukaran data." },
  { "en": "Contoh protokol serial?", "id": "Modbus RTU, ASCII." },
  { "en": "Contoh protokol berbasis Ethernet?", "id": "EtherNet/IP, Modbus TCP, PROFINET." },
  { "en": "Apa itu Modbus?", "id": "Protokol komunikasi industri populer." },
  { "en": "Apa itu Profibus?", "id": "Standar fieldbus untuk otomasi." },
  { "en": "Apa itu 'backplane' PLC?", "id": "Papan sirkuit di belakang modul." },
  { "en": "Fungsi 'backplane'?", "id": "Menghubungkan komunikasi antar modul." },
  { "en": "Apa itu 'high-speed counter'?", "id": "Penghitung untuk sinyal frekuensi tinggi." },
  { "en": "Aplikasi 'high-speed counter'?", "id": "Membaca putaran motor dari encoder." },
  { "en": "Apa itu 'encoder'?", "id": "Sensor untuk mengukur posisi/kecepatan." },
  { "en": "Apa itu instruksi JUMP (JMP)?", "id": "Melompati bagian program tertentu." },
  { "en": "Pasangan instruksi JUMP?", "id": "Label (LBL) sebagai tujuan lompatan." },
  { "en": "Apa itu 'retentive memory'?", "id": "Memori yang menyimpan data tanpa daya." },
  { "en": "Apa itu 'non-volatile memory'?", "id": "Memori yang datanya tidak hilang." },
  { "en": "Contoh 'non-volatile memory'?", "id": "Flash memory, EEPROM." },
  { "en": "Apa itu 'I/O forcing'?", "id": "Memaksa input/output ke kondisi tertentu." },
  { "en": "Kapan 'forcing' digunakan?", "id": "Selama pengujian atau simulasi." },
  { "en": "Apa itu 'bit shift register'?", "id": "Menggeser data bit demi bit." },
  { "en": "Aplikasi 'bit shift register'?", "id": "Melacak produk di jalur konveyor." },
  { "en": "Instruksi matematika dasar?", "id": "Tambah (ADD), Kurang (SUB)." },
  { "en": "Instruksi logika boolean?", "id": "AND, OR, XOR, NOT." },
  { "en": "Apa itu 'I/O mapping'?", "id": "Pemetaan alamat fisik ke tag." },
  { "en": "Apa itu 'documentation' program?", "id": "Komentar, deskripsi, dan tag name." },
  { "en": "Mengapa 'documentation' penting?", "id": "Memudahkan perawatan dan modifikasi." },
  { "en": "Apa itu resolusi modul analog?", "id": "Tingkat presisi pengukuran sinyal." },
  { "en": "Apa arti resolusi 12-bit?", "id": "Sinyal dibagi menjadi 4096 level." },
  { "en": "Apa itu 'wiring diagram'?", "id": "Gambar pengkabelan sistem kontrol." },
  { "en": "Apa itu 'terminal block'?", "id": "Titik koneksi untuk kabel-kabel." },
  { "en": "Apa itu 'DIN rail'?", "id": "Rel standar untuk memasang komponen." },
  { "en": "Mengapa pelabelan kabel penting?", "id": "Memudahkan identifikasi dan perbaikan." },
  { "en": "Apa itu 'inductive sensor'?", "id": "Sensor yang mendeteksi objek logam." },
  { "en": "Apa itu 'capacitive sensor'?", "id": "Sensor yang mendeteksi berbagai material." },
  { "en": "Apa itu 'photoelectric sensor'?", "id": "Sensor yang menggunakan cahaya." },
  { "en": "Tipe 'photoelectric sensor'?", "id": "Thru-beam, retro-reflective, diffuse." },
  { "en": "Apa itu 'thermocouple'?", "id": "Sensor suhu berbasis dua logam." },
  { "en": "Apa itu RTD?", "id": "Resistance Temperature Detector." },
  { "en": "Prinsip kerja RTD?", "id": "Resistansi berubah sesuai suhu." },
  { "en": "RTD atau 'thermocouple' lebih akurat?", "id": "RTD umumnya lebih akurat." },
  { "en": "Apa itu 'solenoid valve'?", "id": "Katup yang dikontrol secara elektrik." },
  { "en": "Apa itu 'contactor'?", "id": "Relay besar untuk beban tinggi." },
  { "en": "Beban yang dikontrol 'contactor'?", "id": "Motor listrik tiga fasa." },
  { "en": "Apa itu 'redundancy' PLC?", "id": "Memiliki sistem cadangan (backup)." },
  { "en": "Tujuan 'redundancy'?", "id": "Meningkatkan keandalan sistem (uptime)." },
  { "en": "Apa itu 'hot redundancy'?", "id": "Sistem cadangan langsung mengambil alih." },
  { "en": "Apa itu P&ID?", "id": "Piping and Instrumentation Diagram." },
  { "en": "Fungsi P&ID?", "id": "Menggambarkan alur proses dan instrumentasi." },
  { "en": "Apa itu 'latching relay'?", "id": "Relay yang terkunci setelah diaktifkan." },
  { "en": "Apa itu PAC?", "id": "Programmable Automation Controller." },
  { "en": "Perbedaan PLC dan PAC?", "id": "PAC lebih canggih dan fleksibel." },
  { "en": "Apa itu 'soft PLC'?", "id": "PLC berbasis perangkat lunak di PC." },
  { "en": "Kelemahan 'soft PLC'?", "id": "Tergantung pada stabilitas sistem operasi PC." },
  { "en": "Apa itu 'program checksum'?", "id": "Nilai untuk verifikasi integritas program." },
  { "en": "Apa itu 'cascading timers'?", "id": "Menghubungkan beberapa timer secara seri." },
  { "en": "Tujuan 'cascading timers'?", "id": "Mendapatkan durasi waktu yang panjang." },
  { "en": "Instruksi TON?", "id": "Timer On-Delay." },
  { "en": "Instruksi TOF?", "id": "Timer Off-Delay." },
  { "en": "Instruksi CTU?", "id": "Count Up (menghitung naik)." },
  { "en": "Instruksi CTD?", "id": "Count Down (menghitung turun)." },
  { "en": "Apa itu 'fail-safe'?", "id": "Sistem dirancang untuk gagal aman." },
  { "en": "Contoh kondisi 'fail-safe'?", "id": "Lampu lalu lintas berkedip kuning." },
  { "en": "Apa itu SIL?", "id": "Safety Integrity Level." },
  { "en": "Fungsi SIL?", "id": "Mengukur tingkat keandalan fungsi keselamatan." },
  { "en": "Apa itu 'loop control'?", "id": "Proses menjaga variabel pada setpoint." },
  { "en": "Apa itu 'setpoint'?", "id": "Nilai target yang diinginkan." },
  { "en": "Apa itu 'process variable'?", "id": "Nilai aktual yang diukur." },
  { "en": "Apa itu 'error' dalam loop?", "id": "Perbedaan antara setpoint dan process variable." },
  { "en": "Apa itu 'indirect addressing'?", "id": "Alamat yang menunjuk ke alamat lain." },
  { "en": "Tegangan kontrol umum di AS?", "id": "120 VAC." },
  { "en": "Tegangan kontrol umum di Eropa?", "id": "24 VDC." },
  { "en": "Peralatan Pelindung Diri (APD)?", "id": "Kacamata pengaman, sarung tangan." },
  { "en": "Apa itu 'lockout/tagout' (LOTO)?", "id": "Prosedur keselamatan isolasi energi." },
  { "en": "Mengapa LOTO penting?", "id": "Mencegah cedera saat perbaikan mesin." },
  { "en": "Apa itu 'field device'?", "id": "Perangkat di lapangan (sensor/aktuator)." },
  { "en": "Apa itu 'scaling' sinyal analog?", "id": "Mengubah nilai mentah menjadi unit rekayasa." },
  { "en": "Contoh 'scaling'?", "id": "Mengubah 4-20mA menjadi 0-100 PSI." },
  { "en": "Apa itu IIoT?", "id": "Industrial Internet of Things." },
  { "en": "Peran PLC dalam IIoT?", "id": "Sebagai sumber data dari mesin." },
  { "en": "Apa itu 'open-loop control'?", "id": "Kontrol tanpa umpan balik (feedback)." },
  { "en": "Apa itu 'closed-loop control'?", "id": "Kontrol dengan umpan balik (feedback)." },
  { "en": "PID termasuk loop kontrol apa?", "id": "Closed-loop control." },
  { "en": "Apa fungsi term 'Proportional' (P)?", "id": "Memberi aksi kontrol proporsional error." },
  { "en": "Apa fungsi term 'Integral' (I)?", "id": "Menghilangkan error kondisi stabil." },
  { "en": "Apa fungsi term 'Derivative' (D)?", "id": "Memprediksi dan meredam error masa depan." },
  { "en": "Apa itu 'PID tuning'?", "id": "Proses optimasi parameter P, I, D." },
  { "en": "Apa itu 'autotuning' PID?", "id": "PLC menentukan parameter PID otomatis." },
  { "en": "Apa itu 'integral windup'?", "id": "Akumulasi error integral berlebihan." },
  { "en": "Apa itu 'feed-forward control'?", "id": "Kontrol proaktif sebelum error terjadi." },
  { "en": "Apa itu 'cascade control'?", "id": "Satu loop kontrol mengontrol loop lain." },
  { "en": "Apa itu VFD?", "id": "Variable Frequency Drive." },
  { "en": "Fungsi utama VFD?", "id": "Mengatur kecepatan putaran motor AC." },
  { "en": "Bagaimana PLC mengontrol VFD?", "id": "Melalui output analog atau jaringan." },
  { "en": "Apa itu motor servo?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Apa itu 'motion control'?", "id": "Otomasi pergerakan mekanis yang presisi." },
  { "en": "Apa itu instruksi sequencer (SQO)?", "id": "Mengontrol urutan output secara sekuensial." },
  { "en": "Aplikasi instruksi sequencer?", "id": "Mesin perakitan, lampu lalu lintas." },
  { "en": "Apa itu User-Defined Type (UDT)?", "id": "Tipe data kustom buatan pengguna." },
  { "en": "Keuntungan UDT?", "id": "Mengelompokkan data terkait jadi satu." },
  { "en": "Apa itu Add-On Instruction (AOI)?", "id": "Instruksi kustom buatan pengguna." },
  { "en": "Keuntungan AOI?", "id": "Menyederhanakan logika yang sering dipakai." },
  { "en": "Apa itu 'state machine'?", "id": "Model program berdasarkan status/kondisi." },
  { "en": "Apa itu 'baud rate'?", "id": "Kecepatan transfer data serial." },
  { "en": "Perbedaan RS-232 dan RS-485?", "id": "RS-485 mendukung lebih banyak perangkat." },
  { "en": "Jarak maksimum kabel RS-232?", "id": "Sekitar 15 meter." },
  { "en": "Jarak maksimum kabel RS-485?", "id": "Bisa mencapai 1200 meter." },
  { "en": "Apa itu 'parity check'?", "id": "Metode deteksi eror data sederhana." },
  { "en": "Tipe 'parity check'?", "id": "Even (genap) dan Odd (ganjil)." },
  { "en": "Apa itu OPC?", "id": "Standar komunikasi perangkat lunak industri." },
  { "en": "Kepanjangan OPC?", "id": "OLE for Process Control." },
  { "en": "Apa itu OPC UA?", "id": "OPC Unified Architecture (lebih modern)." },
  { "en": "Keunggulan OPC UA?", "id": "Lebih aman dan lintas platform." },
  { "en": "Apa itu 'fieldbus'?", "id": "Jaringan digital untuk perangkat lapangan." },
  { "en": "Contoh 'fieldbus'?", "id": "DeviceNet, ControlNet, Foundation Fieldbus." },
  { "en": "Apa itu alamat IP?", "id": "Alamat unik di jaringan TCP/IP." },
  { "en": "Apa itu 'subnet mask'?", "id": "Menentukan bagian network dari alamat IP." },
  { "en": "Apa itu alamat MAC?", "id": "Alamat fisik unik dari hardware." },
  { "en": "Switch vs Hub?", "id": "Switch lebih cerdas, mengirim data tertarget." },
  { "en": "Apa itu 'gateway' jaringan?", "id": "Penghubung dua jaringan berbeda protokol." },
  { "en": "Apa itu 'inrush current'?", "id": "Lonjakan arus saat perangkat dinyalakan." },
  { "en": "Peringkat IP pada enklosur?", "id": "Ingress Protection rating." },
  { "en": "Arti dari IP67?", "id": "Tahan debu total dan tahan air." },
  { "en": "Peringkat NEMA?", "id": "Standar enklosur Amerika Utara." },
  { "en": "Apa itu 'intrinsically safe barrier'?", "id": "Membatasi energi ke area berbahaya." },
  { "en": "Tujuan 'intrinsically safe'?", "id": "Mencegah ledakan di area gas/debu." },
  { "en": "Fungsi 'ferrite core'?", "id": "Meredam gangguan frekuensi tinggi." },
  { "en": "Apa itu 'legacy system'?", "id": "Sistem PLC tua yang usang." },
  { "en": "Apa itu 'PLC migration'?", "id": "Proses upgrade dari sistem legacy." },
  { "en": "Apa itu 'commissioning'?", "id": "Proses pengujian akhir sistem di lokasi." },
  { "en": "Apa itu FAT?", "id": "Factory Acceptance Test." },
  { "en": "Di mana FAT dilakukan?", "id": "Di pabrik pembuat sebelum pengiriman." },
  { "en": "Apa itu SAT?", "id": "Site Acceptance Test." },
  { "en": "Di mana SAT dilakukan?", "id": "Di lokasi akhir (site) pelanggan." },
  { "en": "Apa itu FDS?", "id": "Functional Design Specification." },
  { "en": "Isi dari FDS?", "id": "Deskripsi cara kerja sistem." },
  { "en": "Apa itu 'version control'?", "id": "Manajemen perubahan pada kode program." },
  { "en": "Mengapa 'version control' penting?", "id": "Melacak histori dan mencegah konflik." },
  { "en": "Instruksi FIFO?", "id": "First-In, First-Out." },
  { "en": "Aplikasi FIFO?", "id": "Antrian data atau produk." },
  { "en": "Instruksi LIFO?", "id": "Last-In, First-Out." },
  { "en": "Apa itu 'interrupt routine'?", "id": "Program yang dieksekusi oleh trigger." },
  { "en": "Jenis 'interrupt'?", "id": "Hardware, timed, fault." },
  { "en": "Apa itu 'floating-point number'?", "id": "Angka dengan koma desimal." },
  { "en": "Tipe data untuk 'floating-point'?", "id": "REAL atau FLOAT." },
  { "en": "Instruksi manipulasi string?", "id": "Mencari, menggabungkan, memotong teks." },
  { "en": "Fungsi jam internal PLC?", "id": "Menyediakan waktu dan tanggal nyata." },
  { "en": "Apa itu 'data logging'?", "id": "Pencatatan data proses secara periodik." },
  { "en": "Tujuan 'data logging'?", "id": "Analisis, pelacakan, dan optimasi." },
  { "en": "Apa itu 'recipe management'?", "id": "Manajemen set parameter untuk produk." },
  { "en": "Aplikasi 'recipe management'?", "id": "Industri makanan, farmasi, cat." },
  { "en": "Modul input 'thermistor'?", "id": "Membaca sensor suhu thermistor." },
  { "en": "Modul input 'strain gauge'?", "id": "Membaca sensor berat atau regangan." },
  { "en": "Apa itu 'ground loop'?", "id": "Masalah karena ada banyak jalur ground." },
  { "en": "Dampak 'ground loop'?", "id": "Menimbulkan noise pada sinyal analog." },
  { "en": "Apa itu 'shielded cable'?", "id": "Kabel dengan lapisan pelindung noise." },
  { "en": "Kapan 'shielded cable' digunakan?", "id": "Untuk transmisi sinyal analog." },
  { "en": "Apa itu 'aliasing' sinyal?", "id": "Kesalahan pembacaan sinyal frekuensi tinggi." },
  { "en": "Apa itu 'cybersecurity' industri?", "id": "Perlindungan sistem kontrol dari siber." },
  { "en": "Mengapa 'cybersecurity' PLC penting?", "id": "Mencegah sabotase dan pencurian data." },
  { "en": "Apa itu 'firewall' industri?", "id": "Melindungi jaringan kontrol dari luar." },
  { "en": "Apa itu 'peer-to-peer communication'?", "id": "PLC berkomunikasi langsung satu sama lain." },
  { "en": "Apa itu 'client-server model'?", "id": "Model komunikasi HMI/SCADA dengan PLC." },
  { "en": "Apa itu 'tag-based addressing'?", "id": "Menggunakan nama (tag) bukan alamat memori." },
  { "en": "Kelebihan 'tag-based'?", "id": "Program lebih mudah dibaca manusia." },
  { "en": "Apa itu 'alias tag'?", "id": "Nama lain untuk sebuah tag." },
  { "en": "Apa itu 'program scope tag'?", "id": "Tag yang hanya ada di satu program." },
  { "en": "Apa itu 'controller scope tag'?", "id": "Tag yang bisa diakses semua program." },
  { "en": "Apa itu 'HART protocol'?", "id": "Komunikasi digital di atas sinyal 4-20mA." },
  { "en": "Kepanjangan HART?", "id": "Highway Addressable Remote Transducer." },
  { "en": "Apa itu 'process variable' (PV)?", "id": "Nilai terukur dari suatu proses." },
  { "en": "Apa itu 'manipulated variable' (MV)?", "id": "Output kontroler yang diatur." },
  { "en": "Apa itu 'bumpless transfer'?", "id": "Perpindahan mode kontrol tanpa lonjakan." },
  { "en": "Dampak 'derivative kick'?", "id": "Menyebabkan lonjakan output saat setpoint berubah." },
  { "en": "Apa itu 'gain scheduling'?", "id": "Mengubah parameter PID sesuai kondisi." },
  { "en": "Apa itu standar IEC 61131-3?", "id": "Standar internasional untuk bahasa program PLC." },
  { "en": "Apa itu POU?", "id": "Program Organization Unit." },
  { "en": "Tiga jenis POU?", "id": "Program, Function, Function Block." },
  { "en": "Beda 'Function' dan 'Function Block'?", "id": "Function block memiliki memori internal." },
  { "en": "Apa itu 'multitasking' pada PLC?", "id": "Menjalankan beberapa program atau tugas." },
  { "en": "Apa itu 'task priority'?", "id": "Tingkat prioritas eksekusi sebuah tugas." },
  { "en": "Apa itu 'event-driven task'?", "id": "Tugas yang dipicu oleh suatu kejadian." },
  { "en": "Apa itu 'cyclic task'?", "id": "Tugas yang berjalan secara periodik." },
  { "en": "Apa itu 'dry contact'?", "id": "Kontak yang tidak menyediakan tegangan." },
  { "en": "Apa itu 'wet contact'?", "id": "Kontak yang menyediakan tegangan sendiri." },
  { "en": "Apa itu 'hysteresis' atau 'deadband'?", "id": "Rentang mati untuk mencegah osilasi." },
  { "en": "Aplikasi 'deadband'?", "id": "Kontrol level tangki, kontrol suhu." },
  { "en": "Apa itu 'debounce timer'?", "id": "Timer untuk mengabaikan sinyal palsu." },
  { "en": "Kapan 'debounce timer' digunakan?", "id": "Pada input tombol mekanis." },
  { "en": "Apa itu logika 'toggle'?", "id": "Mengubah status output setiap kali input aktif." },
  { "en": "Apa itu DCS?", "id": "Distributed Control System." },
  { "en": "Kapan memilih DCS daripada PLC?", "id": "Untuk kontrol proses skala besar." },
  { "en": "Apa itu 'alarm management'?", "id": "Pengelolaan sistem alarm agar efektif." },
  { "en": "Apa itu 'alarm flood'?", "id": "Terlalu banyak alarm muncul serentak." },
  { "en": "Apa itu 'alarm rationalization'?", "id": "Proses meninjau dan mengurangi alarm." },
  { "en": "Apa itu 'first-out alarm'?", "id": "Mengetahui alarm mana yang pertama terjadi." },
  { "en": "Apa itu MTBF?", "id": "Mean Time Between Failures." },
  { "en": "Apa itu MTTR?", "id": "Mean Time To Repair." },
  { "en": "Bagaimana menghitung 'availability'?", "id": "MTBF dibagi (MTBF + MTTR)." },
  { "en": "Apa itu 'preventive maintenance'?", "id": "Perawatan terjadwal untuk mencegah kerusakan." },
  { "en": "Apa itu 'predictive maintenance'?", "id": "Perawatan berdasarkan prediksi kondisi aset." },
  { "en": "Apa itu 'HAZOP study'?", "id": "Studi bahaya dan operabilitas sistem." },
  { "en": "Apa itu LOPA?", "id": "Layer of Protection Analysis." },
  { "en": "Tujuan LOPA?", "id": "Mengevaluasi lapisan perlindungan keselamatan." },
  { "en": "Apa itu SIF?", "id": "Safety Instrumented Function." },
  { "en": "Apa itu 'proof test' SIF?", "id": "Pengujian periodik fungsi keselamatan." },
  { "en": "Apa itu 'spurious trip'?", "id": "Penghentian proses yang tidak perlu." },
  { "en": "Apa itu 'bypass' sistem keselamatan?", "id": "Menonaktifkan fungsi keselamatan sementara." },
  { "en": "Apa itu MOC?", "id": "Management of Change." },
  { "en": "Mengapa MOC penting?", "id": "Mengelola risiko dari perubahan sistem." },
  { "en": "Apa itu 'vision system'?", "id": "Sistem kamera untuk inspeksi otomatis." },
  { "en": "Bagaimana 'vision system' terhubung PLC?", "id": "Melalui Ethernet atau I/O digital." },
  { "en": "Apa itu RFID?", "id": "Radio-Frequency Identification." },
  { "en": "Aplikasi RFID dengan PLC?", "id": "Pelacakan palet, identifikasi produk." },
  { "en": "Apa itu modul 'load cell'?", "id": "Modul untuk membaca sensor berat." },
  { "en": "Apa itu modul output PWM?", "id": "Pulse Width Modulation output." },
  { "en": "Aplikasi output PWM?", "id": "Mengontrol kecerahan lampu, kecepatan motor DC." },
  { "en": "Apa itu CIP?", "id": "Common Industrial Protocol." },
  { "en": "Jaringan yang menggunakan CIP?", "id": "EtherNet/IP, DeviceNet, ControlNet." },
  { "en": "Apa itu 'explicit messaging'?", "id": "Komunikasi data yang tidak terjadwal." },
  { "en": "Apa itu 'implicit messaging' (I/O)?", "id": "Komunikasi data I/O secara periodik." },
  { "en": "Apa itu DLR?", "id": "Device Level Ring." },
  { "en": "Keuntungan topologi DLR?", "id": "Redundansi jaringan jika satu koneksi putus." },
  { "en": "Apa itu MQTT?", "id": "Protokol perpesanan ringan untuk IoT." },
  { "en": "Model komunikasi MQTT?", "id": "Publish-Subscribe (Pub/Sub)." },
  { "en": "Apa itu 'broker' MQTT?", "id": "Server pusat yang menerima dan mengirim pesan." },
  { "en": "Apa itu 'topic' MQTT?", "id": "Label atau alamat untuk sebuah pesan." },
  { "en": "Apa itu 'Quality of Service' (QoS)?", "id": "Tingkat jaminan pengiriman pesan." },
  { "en": "Apa itu 'edge computing'?", "id": "Pemrosesan data dekat dengan sumbernya." },
  { "en": "Peran PLC di 'edge computing'?", "id": "Sebagai perangkat edge itu sendiri." },
  { "en": "Apa itu 'cloud computing' otomasi?", "id": "Menyimpan dan menganalisis data di cloud." },
  { "en": "Apa itu 'Time-Sensitive Networking' (TSN)?", "id": "Standar Ethernet untuk komunikasi deterministik." },
  { "en": "Apa itu 'pointer' dalam pemrograman?", "id": "Variabel yang berisi alamat memori." },
  { "en": "Apa itu 'FOR loop'?", "id": "Perulangan dengan jumlah iterasi tertentu." },
  { "en": "Apa itu 'WHILE loop'?", "id": "Perulangan selama kondisi terpenuhi." },
  { "en": "Apa itu 'CASE statement'?", "id": "Memilih blok kode berdasarkan nilai." },
  { "en": "Apa itu 'golden batch'?", "id": "Catatan data dari batch produksi ideal." },
  { "en": "Apa itu 'truth table'?", "id": "Tabel yang menunjukkan output logika." },
  { "en": "Apa itu 'comment out'?", "id": "Menonaktifkan baris kode tanpa menghapusnya." },
  { "en": "Bahan isolator umum kabel?", "id": "PVC (Polyvinyl Chloride)." },
  { "en": "Apa itu 'bus bar'?", "id": "Konduktor untuk distribusi daya listrik." },
  { "en": "Apa itu 'cascade control'?", "id": "Satu loop kontrol menjadi setpoint lain." },
  { "en": "Apa itu 'ratio control'?", "id": "Menjaga rasio dua aliran proses." },
  { "en": "Apa itu 'selective control'?", "id": "Memilih satu sinyal dari beberapa sinyal." },
  { "en": "Apa itu 'override control'?", "id": "Kontroler sekunder mengambil alih primer." },
  { "en": "Tujuan 'override control'?", "id": "Untuk proteksi atau batasan operasi." },
  { "en": "Apa itu 'split range control'?", "id": "Satu output mengontrol dua aktuator." },
  { "en": "Contoh 'split range'?", "id": "Kontrol katup pemanas dan pendingin." },
  { "en": "Apa itu 'air supply' untuk pneumatik?", "id": "Sumber udara bertekanan untuk aktuator." },
  { "en": "Apa itu FRL unit?", "id": "Filter, Regulator, Lubricator." },
  { "en": "Fungsi 'filter' di FRL?", "id": "Membersihkan udara dari kotoran." },
  { "en": "Fungsi 'regulator' di FRL?", "id": "Mengatur tekanan udara kerja." },
  { "en": "Fungsi 'lubricator' di FRL?", "id": "Memberi pelumas ke udara." },
  { "en": "Apa itu 'pneumatic cylinder'?", "id": "Aktuator yang bergerak lurus (linear)." },
  { "en": "Apa itu 'hydraulic system'?", "id": "Sistem yang menggunakan fluida cair." },
  { "en": "Kelebihan hidrolik dari pneumatik?", "id": "Dapat menghasilkan gaya yang lebih besar." },
  { "en": "Apa itu 'I/P converter'?", "id": "Mengubah sinyal arus (I) ke tekanan (P)." },
  { "en": "Apa itu 'valve positioner'?", "id": "Memastikan katup mencapai posisi yang benar." },
  { "en": "Apa itu 'latching alarm'?", "id": "Alarm yang tetap aktif hingga di-reset." },
  { "en": "Apa itu 'process simulation'?", "id": "Meniru perilaku proses industri." },
  { "en": "Tujuan 'process simulation'?", "id": "Pelatihan operator dan pengujian strategi." },
  { "en": "Apa itu 'digital twin'?", "id": "Model virtual dari aset fisik." },
  { "en": "Manfaat 'digital twin'?", "id": "Monitoring, simulasi, dan prediksi." },
  { "en": "Apa itu 'checksum error'?", "id": "Kesalahan yang terdeteksi pada data." },
  { "en": "Apa itu 'memory fragmentation'?", "id": "Memori menjadi tidak efisien terbagi-bagi." },
  { "en": "Apa itu 'handshaking'?", "id": "Sinyal untuk sinkronisasi komunikasi." },
  { "en": "Contoh 'handshaking'?", "id": "Request-to-Send (RTS), Clear-to-Send (CTS)." },
  { "en": "Apa itu 'polling' I/O?", "id": "CPU secara aktif memeriksa status I/O." },
  { "en": "Apa itu 'change-of-state' (COS)?", "id": "Transmisi data hanya saat nilai berubah." },
  { "en": "Apa itu 'deterministic network'?", "id": "Jaringan dengan waktu transmisi terprediksi." },
  { "en": "Mengapa determinisme penting?", "id": "Untuk aplikasi kontrol gerak (motion)." },
  { "en": "Apa itu 'machine learning' di industri?", "id": "AI untuk optimasi dan deteksi anomali." },
  { "en": "Apa itu 'API'?", "id": "Application Programming Interface." },
  { "en": "Apa itu 'flyback diode'?", "id": "Diode pelindung dari lonjakan tegangan." },
  { "en": "Di mana 'flyback diode' dipasang?", "id": "Paralel dengan beban induktif DC." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Sirkuit peredam lonjakan tegangan AC." },
  { "en": "Apa itu 'leakage current'?", "id": "Arus kecil saat perangkat OFF." },
  { "en": "Apa itu 'pull-up resistor'?", "id": "Menarik sinyal ke tegangan tinggi." },
  { "en": "Apa itu 'pull-down resistor'?", "id": "Menarik sinyal ke ground (0V)." },
  { "en": "Apa itu 'galvanic isolation'?", "id": "Isolasi sirkuit tanpa koneksi langsung." },
  { "en": "Apa itu 'common terminal'?", "id": "Terminal referensi bersama untuk beberapa channel." },
  { "en": "Beda channel terisolasi vs tidak?", "id": "Terisolasi punya common sendiri-sendiri." },
  { "en": "Apa itu 'ripple' catu daya DC?", "id": "Variasi tegangan AC sisa." },
  { "en": "Apa itu 'derating' power supply?", "id": "Mengurangi kapasitas beban karena suhu." },
  { "en": "Apa itu 'wire ferrule'?", "id": "Selongsong logam ujung kabel serabut." },
  { "en": "Tujuan memakai 'ferrule'?", "id": "Mencegah kerusakan dan koneksi lebih baik." },
  { "en": "Apa itu 'wire duct'?", "id": "Saluran untuk merapikan kabel panel." },
  { "en": "Apa itu 'panel layout diagram'?", "id": "Gambar tata letak komponen panel." },
  { "en": "Apa itu 'emulator' PLC?", "id": "Software yang meniru CPU PLC." },
  { "en": "Beda simulator dan emulator?", "id": "Emulator lebih akurat meniru hardware." },
  { "en": "Apa itu 'source code protection'?", "id": "Melindungi logika program agar tidak dilihat." },
  { "en": "Apa itu 'library' program?", "id": "Kumpulan kode yang dapat digunakan kembali." },
  { "en": "Fungsi 'search and replace'?", "id": "Mencari dan mengganti teks/tag." },
  { "en": "Apa itu 'logic comparison tool'?", "id": "Membandingkan dua versi program berbeda." },
  { "en": "Apa itu 'tag database'?", "id": "Daftar semua tag dalam proyek." },
  { "en": "Apa itu tipe data DINT?", "id": "Double Integer (32-bit)." },
  { "en": "Apa itu tipe data SINT?", "id": "Short Integer (8-bit)." },
  { "en": "Apa itu tipe data REAL?", "id": "Angka floating-point presisi tunggal." },
  { "en": "Representasi bilangan heksadesimal?", "id": "Menggunakan awalan seperti 16#." },
  { "en": "Apa itu 'instrument calibration'?", "id": "Menyesuaikan instrumen dengan standar." },
  { "en": "Apa itu 'control valve'?", "id": "Katup untuk mengatur aliran fluida." },
  { "en": "Apa itu 'valve stiction'?", "id": "Katup 'lengket' saat akan bergerak." },
  { "en": "Apa itu 'instrument loop diagram'?", "id": "Gambar detail satu loop kontrol." },
  { "en": "Apa itu 'flowmeter'?", "id": "Alat untuk mengukur laju aliran." },
  { "en": "Apa itu 'level transmitter'?", "id": "Alat untuk mengukur ketinggian level." },
  { "en": "Apa itu 'pH sensor'?", "id": "Mengukur tingkat keasaman atau kebasaan." },
  { "en": "Apa itu standar ISA-88 (S88)?", "id": "Standar untuk kontrol proses batch." },
  { "en": "Apa itu 'phase' dalam S88?", "id": "Elemen terkecil dari kontrol prosedur." },
  { "en": "Apa itu 'unit' dalam S88?", "id": "Kumpulan peralatan untuk satu proses." },
  { "en": "Apa itu MES?", "id": "Manufacturing Execution System." },
  { "en": "Fungsi MES?", "id": "Menjembatani sistem pabrik dan bisnis." },
  { "en": "Apa itu ERP?", "id": "Enterprise Resource Planning." },
  { "en": "Hubungan PLC ke MES?", "id": "PLC menyediakan data produksi ke MES." },
  { "en": "Apa itu 'Purdue Model'?", "id": "Model hierarki arsitektur kontrol perusahaan." },
  { "en": "Level terendah Purdue Model?", "id": "Proses fisik (Level 0)." },
  { "en": "Level tertinggi Purdue Model?", "id": "Jaringan perusahaan (Level 5)." },
  { "en": "Apa itu OEE?", "id": "Overall Equipment Effectiveness." },
  { "en": "Tiga komponen OEE?", "id": "Availability, Performance, Quality." },
  { "en": "Apa itu 'downtime'?", "id": "Waktu saat mesin tidak berproduksi." },
  { "en": "Apa itu 'changeover time'?", "id": "Waktu untuk beralih produksi produk." },
  { "en": "Apa itu 'master/slave communication'?", "id": "Satu perangkat (master) mengontrol lainnya." },
  { "en": "Apa itu 'token ring'?", "id": "Metode akses jaringan dengan 'token'." },
  { "en": "Apa itu 'packet sniffing'?", "id": "Merekam dan menganalisis trafik jaringan." },
  { "en": "Apa itu 'firewall'?", "id": "Perangkat keamanan untuk membatasi akses." },
  { "en": "Apa itu 'VPN'?", "id": "Virtual Private Network." },
  { "en": "Fungsi VPN untuk PLC?", "id": "Akses remote yang aman ke jaringan." },
  { "en": "Apa itu 'DHCP'?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Fungsi DHCP server?", "id": "Memberikan alamat IP secara otomatis." },
  { "en": "Apa itu 'static IP'?", "id": "Alamat IP yang diatur manual." },
  { "en": "Kapan pakai 'static IP'?", "id": "Untuk server, printer, dan PLC." },
  { "en": "Apa itu 'firmware flash'?", "id": "Proses memperbarui firmware perangkat." },
  { "en": "Risiko 'firmware flash'?", "id": "Perangkat bisa rusak jika gagal." },
  { "en": "Apa itu 'cold boot'?", "id": "Menyalakan PLC dari kondisi mati total." },
  { "en": "Apa itu 'warm boot'?", "id": "Merestart PLC tanpa mematikan daya." },
  { "en": "Apa itu 'latch' bit?", "id": "Bit internal yang mempertahankan statusnya." },
  { "en": "Apa itu 'unlatch' bit?", "id": "Merestart atau mematikan 'latch' bit." },
  { "en": "Apa itu 'I/O delay'?", "id": "Waktu tunda pada modul I/O." },
  { "en": "Apa itu 'filter time'?", "id": "Waktu filter untuk menolak sinyal palsu." },
  { "en": "Apa itu 'proportional band'?", "id": "Kebalikan dari proportional gain (P)." },
  { "en": "Apa itu 'derivative time'?", "id": "Parameter D dalam unit waktu." },
  { "en": "Apa itu 'integral time'?", "id": "Parameter I dalam unit waktu." },
  { "en": "Apa itu 'bias' dalam kontrol PID?", "id": "Nilai output saat error nol." },
  { "en": "Apa itu 'direct acting' control?", "id": "Output meningkat saat input meningkat." },
  { "en": "Apa itu 'reverse acting' control?", "id": "Output menurun saat input meningkat." },
  { "en": "Contoh 'reverse acting'?", "id": "Kontrol pendingin, setpoint 25°, suhu 30°." },
  { "en": "Apa itu 'motion profile'?", "id": "Grafik posisi, kecepatan, percepatan." },
  { "en": "Tipe 'motion profile'?", "id": "Trapezoidal (trapesium) dan S-curve." },
  { "en": "Keuntungan S-curve profile?", "id": "Pergerakan lebih halus, mengurangi getaran." },
  { "en": "Apa itu 'absolute encoder'?", "id": "Encoder yang tahu posisi absolutnya." },
  { "en": "Apa itu 'incremental encoder'?", "id": "Encoder yang hanya melaporkan perubahan." },
  { "en": "Apa itu 'homing sequence'?", "id": "Proses mencari posisi nol (home)." },
  { "en": "Apa itu 'z-pulse' atau 'index'?", "id": "Sinyal satu pulsa per putaran." },
  { "en": "Apa itu 'boolean algebra'?", "id": "Aljabar untuk operasi logika." },
  { "en": "Apa itu 'De Morgan's theorems'?", "id": "Aturan untuk menyederhanakan logika NOT." },
  { "en": "Apa itu 'Karnaugh map'?", "id": "Metode grafis untuk menyederhanakan logika." },
  { "en": "Apa itu 'array' data?", "id": "Kumpulan data sejenis dalam satu variabel." },
  { "en": "Apa itu 'index' array?", "id": "Nomor posisi elemen dalam array." },
  { "en": "Aplikasi array di PLC?", "id": "Menyimpan data resep atau historis." },
  { "en": "Apa itu 'FIFO' dan 'LIFO'?", "id": "Instruksi untuk mengelola antrian data." },
  { "en": "Apa itu 'masking'?", "id": "Mengabaikan atau memilih bit tertentu." },
  { "en": "Apa itu 'scope'?", "id": "Alat untuk melihat bentuk gelombang sinyal." },
  { "en": "Apa itu 'data logger'?", "id": "Perangkat untuk merekam data dari waktu ke waktu." },
  { "en": "Apa itu 'trending'?", "id": "Membuat grafik data terhadap waktu." },
  { "en": "Apa itu 'actuator signature'?", "id": "Profil unik kinerja sebuah aktuator." },
  { "en": "Apa itu 'machine learning model'?", "id": "Model matematis hasil pembelajaran mesin." },
  { "en": "Apa itu 'anomaly detection'?", "id": "Mendeteksi pola data yang tidak normal." },
  { "en": "Apa itu 'data historian'?", "id": "Database untuk data proses historis." },
  { "en": "Apa itu 'asset management'?", "id": "Pengelolaan aset fisik perusahaan." },
  { "en": "Apa itu 'remote I/O' (RIO)?", "id": "Modul I/O yang terhubung via jaringan." },
  { "en": "Keuntungan 'remote I/O'?", "id": "Mengurangi biaya dan kompleksitas pengkabelan." },
  { "en": "Apa itu 'greenfield project'?", "id": "Proyek yang dibangun dari nol." },
  { "en": "Apa itu 'brownfield project'?", "id": "Proyek yang memodifikasi sistem yang ada." },
  { "en": "Apa itu 'vendor lock-in'?", "id": "Ketergantungan pada satu pemasok." },
  { "en": "Apa itu 'backward compatibility'?", "id": "Kompatibel dengan versi perangkat keras/lunak lama." },
  { "en": "Apa itu 'scalability' sistem?", "id": "Kemampuan sistem untuk tumbuh." },
  { "en": "Apa itu 'interoperability'?", "id": "Kemampuan sistem berbeda bekerja sama." },
  { "en": "Apa itu 'real-time operating system' (RTOS)?", "id": "Sistem operasi dengan respons waktu terjamin." },
  { "en": "Apa itu 'hard real-time'?", "id": "Tenggat waktu yang tidak boleh terlewat." },
  { "en": "Apa itu 'soft real-time'?", "id": "Tenggat waktu bisa terlewat sesekali." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi waktu eksekusi sebuah tugas." },
  { "en": "Apa itu 'endianness'?", "id": "Urutan byte dalam data word." },
  { "en": "Apa itu 'type casting'?", "id": "Mengubah satu tipe data ke tipe lain." },
  { "en": "Apa itu 'semaphore'?", "id": "Mekanisme untuk mengontrol akses sumber daya." },
  { "en": "Apa itu 'memory leak' di PLC?", "id": "Memori yang tidak dibebaskan setelah dipakai." },
  { "en": "Apa itu 'audit trail'?", "id": "Catatan kronologis tindakan dan perubahan." },
  { "en": "Apa itu 'electronic signature'?", "id": "Tanda tangan digital untuk persetujuan." },
  { "en": "Apa itu FDA 21 CFR Part 11?", "id": "Aturan AS untuk data elektronik." },
  { "en": "Industri apa yang memakai 21 CFR Part 11?", "id": "Farmasi dan makanan." },
  { "en": "Apa itu 'validation' di farmasi?", "id": "Proses pembuktian sistem bekerja benar." },
  { "en": "Apa itu sistem CIP?", "id": "Clean-In-Place." },
  { "en": "Tujuan sistem CIP?", "id": "Membersihkan peralatan proses secara otomatis." },
  { "en": "Apa itu sistem SIP?", "id": "Sterilization-In-Place." },
  { "en": "Apa itu 'bus terminator'?", "id": "Resistor di ujung jaringan bus." },
  { "en": "Fungsi 'bus terminator'?", "id": "Mencegah pantulan sinyal di kabel." },
  { "en": "Apa itu 'repeater'?", "id": "Memperkuat dan mengirim ulang sinyal jaringan." },
  { "en": "Apa itu I/O-Link?", "id": "Standar komunikasi point-to-point cerdas." },
  { "en": "Keuntungan I/O-Link?", "id": "Konfigurasi dan diagnostik sensor cerdas." },
  { "en": "Apa itu 'conformal coating'?", "id": "Lapisan pelindung pada papan sirkuit." },
  { "en": "Tujuan 'conformal coating'?", "id": "Melindungi dari kelembaban dan korosi." },
  { "en": "Apa itu 'UL Listing'?", "id": "Sertifikasi keamanan produk oleh UL." },
  { "en": "Apa itu 'CE Marking'?", "id": "Tanda kesesuaian produk standar Eropa." },
  { "en": "Apa itu EMC?", "id": "Electromagnetic Compatibility." },
  { "en": "Apa itu EMI?", "id": "Electromagnetic Interference." },
  { "en": "Apa itu 'radiated EMI'?", "id": "Gangguan yang dipancarkan melalui udara." },
  { "en": "Apa itu 'conducted EMI'?", "id": "Gangguan yang merambat melalui kabel." },
  { "en": "Apa itu MOV (Varistor)?", "id": "Komponen pelindung dari lonjakan tegangan." },
  { "en": "Apa itu 'zener diode'?", "id": "Diode untuk regulasi tegangan." },
  { "en": "Terminal 'spring clamp' vs 'screw'?", "id": "Spring clamp lebih cepat dan tahan getaran." },
  { "en": "Apa itu 'Power over Ethernet' (PoE)?", "id": "Daya listrik melalui kabel Ethernet." },
  { "en": "Aplikasi PoE di industri?", "id": "Kamera IP, sensor, telepon VoIP." },
  { "en": "Apa itu CNC?", "id": "Computer Numerical Control." },
  { "en": "Fungsi CNC?", "id": "Mengontrol mesin perkakas secara otomatis." },
  { "en": "Apa itu BMS?", "id": "Building Management System." },
  { "en": "Fungsi BMS?", "id": "Mengontrol sistem gedung (HVAC, lampu)." },
  { "en": "Apa itu aplikasi 'pick and place'?", "id": "Mengambil dan menempatkan objek." },
  { "en": "Apa itu 'tension control'?", "id": "Menjaga ketegangan material lembaran." },
  { "en": "Apa itu 'web handling'?", "id": "Proses material fleksibel seperti kertas." },
  { "en": "Apa itu 'batch traceability'?", "id": "Kemampuan melacak riwayat batch produk." },
  { "en": "Apa itu 'historian'?", "id": "Database yang dioptimalkan untuk data historis." },
  { "en": "Apa itu 'asset' dalam industri?", "id": "Peralatan fisik seperti pompa, motor." },
  { "en": "Apa itu 'single point of failure'?", "id": "Satu komponen yang jika gagal, sistem mati." },
  { "en": "Apa itu 'graceful degradation'?", "id": "Sistem menurun fungsinya, tidak mati total." },
  { "en": "Apa itu 'human error'?", "id": "Kesalahan yang disebabkan oleh manusia." },
  { "en": "Apa itu 'switchover time'?", "id": "Waktu perpindahan ke sistem cadangan." },
  { "en": "Apa itu 'load sharing'?", "id": "Dua unit berbagi beban kerja." },
  { "en": "Apa itu 'diagnostic buffer'?", "id": "Memori yang menyimpan pesan kesalahan." },
  { "en": "Apa itu 'force table'?", "id": "Daftar I/O yang sedang di-force." },
  { "en": "Apa itu 'scripting' di HMI?", "id": "Menulis kode untuk fungsi kustom." },
  { "en": "Bahasa skrip HMI umum?", "id": "VBScript, JavaScript." },
  { "en": "Apa itu 'global variable'?", "id": "Variabel yang bisa diakses dari mana saja." },
  { "en": "Apa itu 'local variable'?", "id": "Variabel yang hanya ada di satu POU." },
  { "en": "Apa itu 'pointer' dereferencing?", "id": "Mengakses nilai di alamat yang ditunjuk." },
  { "en": "Apa itu 'checksum' program?", "id": "Nilai untuk memverifikasi integritas program." },
  { "en": "Apa itu 'watch table'?", "id": "Tabel untuk memantau nilai variabel." },
  { "en": "Apa itu 'trending view'?", "id": "Tampilan grafik nilai terhadap waktu." },
  { "en": "Apa itu 'user authentication'?", "id": "Verifikasi identitas pengguna (login)." },
  { "en": "Apa itu 'user authorization'?", "id": "Pemberian hak akses kepada pengguna." },
  { "en": "Apa itu 'alarm shelving'?", "id": "Menyembunyikan alarm untuk sementara waktu." },
  { "en": "Apa itu 'alarm prioritization'?", "id": "Memberi tingkat prioritas pada alarm." },
  { "en": "Contoh prioritas alarm?", "id": "Kritis, tinggi, sedang, rendah." },
  { "en": "Apa itu 'chattering alarm'?", "id": "Alarm yang aktif-nonaktif sangat cepat." },
  { "en": "Apa itu 'system variable'?", "id": "Variabel yang disediakan oleh sistem PLC." },
  { "en": "Contoh 'system variable'?", "id": "Waktu scan, status baterai, waktu." },
  { "en": "Apa itu 'bump test'?", "id": "Pengujian singkat sensor gas." },
  { "en": "Apa itu 'SIL verification'?", "id": "Perhitungan untuk membuktikan level SIL." },
  { "en": "Apa itu 'common cause failure'?", "id": "Satu penyebab merusak beberapa komponen." },
  { "en": "Apa itu 'diversity' dalam keselamatan?", "id": "Menggunakan teknologi berbeda untuk redundansi." },
  { "en": "Contoh 'diversity'?", "id": "Sensor tekanan beda merek atau prinsip." },
  { "en": "Apa itu 'voting system' (2oo3)?", "id": "Sistem butuh 2 dari 3 input setuju." },
  { "en": "Tujuan 'voting system'?", "id": "Meningkatkan keselamatan dan ketersediaan." },
  { "en": "Apa itu 'proof test coverage'?", "id": "Persentase kegagalan yang terdeteksi tes." },
  { "en": "Apa itu 'mission time'?", "id": "Umur pakai yang direncanakan untuk sistem." },
  { "en": "Apa itu 'spurious trip rate' (STR)?", "id": "Tingkat trip palsu per tahun." },
  { "en": "Apa itu 'factory talk'?", "id": "Rangkaian software dari Allen-Bradley." },
  { "en": "Apa itu 'TIA Portal'?", "id": "Rangkaian software dari Siemens." },
  { "en": "Apa itu 'Sysmac Studio'?", "id": "Rangkaian software dari Omron." },
  { "en": "Apa itu 'hot standby'?", "id": "Sistem cadangan menyala dan sinkron." },
  { "en": "Apa itu 'cold standby'?", "id": "Sistem cadangan mati, perlu dinyalakan." },
  { "en": "Apa itu 'fail-active' system?", "id": "Sistem tetap beroperasi saat gagal." },
  { "en": "Apa itu 'fail-operational' system?", "id": "Serupa dengan 'fail-active'." },
  { "en": "Apa itu 'simplex system'?", "id": "Sistem tanpa redundansi." },
  { "en": "Apa itu 'duplex system'?", "id": "Sistem dengan satu lapisan redundansi." },
  { "en": "Apa itu 'machine state'?", "id": "Kondisi operasi mesin (misal: idle, running)." },
  { "en": "Apa itu 'sequential control'?", "id": "Kontrol berdasarkan urutan langkah." },
  { "en": "Apa itu 'regulatory control'?", "id": "Kontrol untuk menjaga variabel proses." },
  { "en": "Apa itu 'supervisory control'?", "id": "Kontrol yang mengawasi kontroler lain." },
  { "en": "Apa itu 'optimization'?", "id": "Menemukan kondisi operasi terbaik." },
  { "en": "Apa itu 'loop check'?", "id": "Verifikasi fungsional seluruh loop kontrol." },
  { "en": "Apa itu tes 'continuity'?", "id": "Memeriksa apakah ada jalur listrik terhubung." },
  { "en": "Apa itu 'megger test'?", "id": "Tes untuk mengukur resistansi isolasi." },
  { "en": "Kapan 'megger test' dilakukan?", "id": "Pada motor atau kabel tegangan tinggi." },
  { "en": "Apa itu 'hot spot' di panel?", "id": "Area dengan suhu berlebih." },
  { "en": "Alat untuk mendeteksi 'hot spot'?", "id": "Kamera thermal (thermal imager)." },
  { "en": "Apa itu 'phase sequence meter'?", "id": "Memeriksa urutan fasa R-S-T." },
  { "en": "Apa itu 'earth fault'?", "id": "Hubungan singkat ke ground (bumi)." },
  { "en": "Apa itu 'signal conditioner'?", "id": "Mengubah sinyal menjadi format standar." },
  { "en": "Apa itu 'trip amplifier'?", "id": "Membandingkan sinyal proses dengan setpoint." },
  { "en": "Apa itu 'solid-state relay' (SSR)?", "id": "Relay tanpa komponen bergerak." },
  { "en": "Apa itu 'zero-crossing' switching?", "id": "SSR aktif saat tegangan mendekati nol." },
  { "en": "Keuntungan 'zero-crossing'?", "id": "Mengurangi lonjakan arus dan noise." },
  { "en": "Apa itu 'crosstalk'?", "id": "Interferensi sinyal antar kabel berdekatan." },
  { "en": "Apa itu 'keying' pada modul PLC?", "id": "Fitur fisik pencegah salah pasang." },
  { "en": "Apa itu 'pickup voltage'?", "id": "Tegangan minimum untuk mengaktifkan relay." },
  { "en": "Apa itu 'dropout voltage'?", "id": "Tegangan maksimum saat relay nonaktif." },
  { "en": "Apa itu standar ISA-95?", "id": "Standar integrasi MES dan ERP." },
  { "en": "Apa itu PackML?", "id": "Packaging Machine Language." },
  { "en": "Tujuan PackML?", "id": "Standarisasi antarmuka mesin pengemasan." },
  { "en": "Apa itu direktif ATEX?", "id": "Regulasi Eropa untuk area berbahaya." },
  { "en": "Apa itu skema IECEx?", "id": "Sertifikasi global untuk area berbahaya." },
  { "en": "Apa itu proteksi 'Ex d'?", "id": "Enklosur tahan api (flameproof)." },
  { "en": "Apa itu proteksi 'Ex e'?", "id": "Keamanan yang ditingkatkan (increased safety)." },
  { "en": "Apa itu 'Functional Safety Management'?", "id": "Sistem manajemen untuk keselamatan fungsional." },
  { "en": "Apa itu 'race condition'?", "id": "Hasil tergantung urutan eksekusi tak terduga." },
  { "en": "Apa itu 'deadlock'?", "id": "Dua proses saling menunggu tak terbatas." },
  { "en": "Apa itu 'unit testing'?", "id": "Pengujian unit kode terkecil (misal: AOI)." },
  { "en": "Apa itu 'regression testing'?", "id": "Menguji ulang setelah ada perubahan." },
  { "en": "Apa itu 'sandbox' environment?", "id": "Lingkungan terisolasi untuk pengujian aman." },
  { "en": "Apa itu 'serialization'?", "id": "Mengubah objek data menjadi format serial." },
  { "en": "Apa itu 'deserialization'?", "id": "Mengubah data serial kembali menjadi objek." },
  { "en": "Apa itu 'stack' memori?", "id": "Memori untuk variabel lokal dan pemanggilan fungsi." },
  { "en": "Apa itu 'heap' memori?", "id": "Memori untuk alokasi data dinamis." },
  { "en": "Apa itu 'garbage collection'?", "id": "Proses membebaskan memori yang tak terpakai." },
  { "en": "Apa itu FPGA?", "id": "Field-Programmable Gate Array." },
  { "en": "Beda FPGA dan PLC?", "id": "FPGA lebih cepat, pemrosesan paralel." },
  { "en": "Apa itu 'microcontroller'?", "id": "Komputer kecil dalam satu chip." },
  { "en": "Beda 'microcontroller' dan PLC?", "id": "PLC lebih tangguh dan modular." },
  { "en": "Apa itu LabVIEW?", "id": "Lingkungan pengembangan pemrograman grafis." },
  { "en": "Apa itu MATLAB/Simulink?", "id": "Software untuk komputasi teknis dan simulasi." },
  { "en": "Fungsi 'code generation'?", "id": "Membuat kode PLC dari model Simulink." },
  { "en": "Apa itu 'augmented reality' (AR)?", "id": "Menggabungkan dunia nyata dan virtual." },
  { "en": "Aplikasi AR di pabrik?", "id": "Panduan perawatan, visualisasi data." },
  { "en": "Apa itu 'lights-out manufacturing'?", "id": "Pabrik beroperasi tanpa campur tangan manusia." },
  { "en": "Apa itu 'total cost of ownership'?", "id": "Total biaya selama umur aset." },
  { "en": "Apa itu 'RAMS'?", "id": "Reliability, Availability, Maintainability, Safety." },
  { "en": "Apa itu 'root cause analysis'?", "id": "Analisis untuk menemukan akar penyebab masalah." },
  { "en": "Teknik '5 Whys'?", "id": "Bertanya 'mengapa' lima kali berturut-turut." },
  { "en": "Apa itu 'Pareto chart'?", "id": "Grafik untuk identifikasi masalah utama." },
  { "en": "Prinsip Pareto?", "id": "80% masalah disebabkan 20% penyebab." },
  { "en": "Apa itu 'Gantt chart'?", "id": "Bagan untuk visualisasi jadwal proyek." },
  { "en": "Apa itu 'agile methodology'?", "id": "Metodologi proyek yang iteratif dan fleksibel." },
  { "en": "Apa itu 'OPC DA'?", "id": "OPC Data Access (versi klasik)." },
  { "en": "Apa itu 'OPC HDA'?", "id": "OPC Historical Data Access." },
  { "en": "Apa itu 'OPC A&E'?", "id": "OPC Alarms & Events." },
  { "en": "Apa itu 'time-series data'?", "id": "Data dengan stempel waktu (timestamp)." },
  { "en": "Apa itu 'metadata'?", "id": "Data yang mendeskripsikan data lain." },
  { "en": "Contoh 'metadata'?", "id": "Unit pengukuran, lokasi sensor." },
  { "en": "Apa itu 'air gap' (keamanan)?", "id": "Mengisolasi jaringan secara fisik total." },
  { "en": "Apa itu 'demilitarized zone' (DMZ)?", "id": "Sub-jaringan antara jaringan internal dan eksternal." },
  { "en": "Apa itu 'whitelisting' aplikasi?", "id": "Hanya mengizinkan aplikasi terpercaya berjalan." },
  { "en": "Apa itu 'blacklisting' aplikasi?", "id": "Melarang aplikasi yang tidak dipercaya." },
  { "en": "Apa itu 'penetration testing'?", "id": "Simulasi serangan siber untuk uji keamanan." },
  { "en": "Apa itu 'latent failure'?", "id": "Kegagalan tersembunyi yang belum terdeteksi." },
  { "en": "Apa itu 'diagnostic coverage'?", "id": "Kemampuan sistem mendeteksi kegagalan internal." },
  { "en": "Apa itu 'fault tolerance'?", "id": "Kemampuan sistem beroperasi meski ada fault." },
  { "en": "Apa itu 'fault injection testing'?", "id": "Sengaja memasukkan fault untuk menguji respons." },
  { "en": "Apa itu 'deterministic logic solver'?", "id": "Istilah lain untuk CPU PLC." },
  { "en": "Apa itu 'analog to digital converter' (ADC)?", "id": "Mengubah sinyal analog ke digital." },
  { "en": "Komponen utama modul input analog?", "id": "ADC (Analog to Digital Converter)." },
  { "en": "Apa itu 'digital to analog converter' (DAC)?", "id": "Mengubah sinyal digital ke analog." },
  { "en": "Komponen utama modul output analog?", "id": "DAC (Digital to Analog Converter)." },
  { "en": "Apa itu 'sampling rate'?", "id": "Seberapa sering sinyal analog diukur." },
  { "en": "Apa itu 'Nyquist theorem'?", "id": "Teorema tentang sampling rate minimum." },
  { "en": "Apa itu 'quantization error'?", "id": "Error karena konversi analog ke digital." },
  { "en": "Apa itu 'electrical schematic'?", "id": "Diagram yang menunjukkan fungsi sirkuit." },
  { "en": "Beda 'schematic' dan 'wiring diagram'?", "id": "Schematic untuk fungsi, wiring untuk koneksi." },
  { "en": "Apa itu 'bill of materials' (BOM)?", "id": "Daftar semua komponen yang dibutuhkan." },
  { "en": "Apa itu 'single-line diagram' (SLD)?", "id": "Diagram sederhana sistem distribusi listrik." },
  { "en": "Apa itu 'bus topology'?", "id": "Semua perangkat terhubung ke satu kabel." },
  { "en": "Apa itu 'star topology'?", "id": "Perangkat terhubung ke satu titik pusat." },
  { "en": "Apa itu 'ring topology'?", "id": "Perangkat terhubung membentuk lingkaran." },
  { "en": "Apa itu 'mesh topology'?", "id": "Perangkat terhubung ke banyak perangkat lain." },
  { "en": "Topologi jaringan paling andal?", "id": "Mesh topology." },
  { "en": "Apa itu 'AS-i bus'?", "id": "Actuator Sensor Interface." },
  { "en": "Karakteristik 'AS-i bus'?", "id": "Kabel pipih kuning untuk sensor/aktuator." },
  { "en": "Apa itu 'CAN bus'?", "id": "Controller Area Network." },
  { "en": "Aplikasi umum 'CAN bus'?", "id": "Otomotif dan sistem kontrol bergerak." },
  { "en": "Apa itu 'data mapping'?", "id": "Memetakan data dari satu format ke lain." },
  { "en": "Apa itu 'bit-level access'?", "id": "Kemampuan membaca atau menulis satu bit." },
  { "en": "Apa itu 'structured binding'?", "id": "Mengurai struktur data menjadi variabel." },
  { "en": "Apa itu 'token' dalam komunikasi?", "id": "Paket data khusus untuk hak transmisi." },
  { "en": "Apa itu 'polling cycle'?", "id": "Waktu untuk master menanyai semua slave." },
  { "en": "Apa itu 'exception-based reporting'?", "id": "Melaporkan data hanya jika ada perubahan." },
  { "en": "Keuntungan 'exception-based reporting'?", "id": "Mengurangi beban trafik jaringan." },
  { "en": "Apa itu URS?", "id": "User Requirement Specification." },
  { "en": "Tujuan URS?", "id": "Menjelaskan kebutuhan pengguna dari sistem." },
  { "en": "Apa itu 'scope creep'?", "id": "Penambahan fitur di luar lingkup awal." },
  { "en": "Apa itu 'stakeholder' proyek?", "id": "Pihak yang berkepentingan dalam proyek." },
  { "en": "Apa itu 'critical path'?", "id": "Urutan tugas terpanjang dalam proyek." },
  { "en": "Metodologi 'waterfall' vs 'agile'?", "id": "Waterfall sekuensial, agile iteratif." },
  { "en": "Apa itu 'lessons learned'?", "id": "Evaluasi setelah proyek selesai." },
  { "en": "Apa itu CAPEX?", "id": "Capital Expenditure (belanja modal)." },
  { "en": "Apa itu OPEX?", "id": "Operational Expenditure (biaya operasional)." },
  { "en": "Apa itu ROI?", "id": "Return on Investment." },
  { "en": "Apa itu 'high-performance HMI'?", "id": "Desain HMI yang efektif dan minimalis." },
  { "en": "Prinsip utama HMI performa tinggi?", "id": "Gunakan warna abu-abu, warna untuk alarm." },
  { "en": "Apa itu 'situational awareness'?", "id": "Pemahaman operator terhadap kondisi proses." },
  { "en": "Apa itu 'alarm fatigue'?", "id": "Kelelahan operator karena terlalu banyak alarm." },
  { "en": "Apa itu 'style guide' HMI?", "id": "Panduan konsistensi desain antarmuka." },
  { "en": "Apa itu SOP?", "id": "Standard Operating Procedure." },
  { "en": "Peran PLC dalam SOP?", "id": "Mengotomatiskan dan menegakkan langkah-langkah SOP." },
  { "en": "Apa itu 'virtual commissioning'?", "id": "Pengujian kode dengan simulasi digital." },
  { "en": "Keuntungan 'virtual commissioning'?", "id": "Mengurangi waktu pengujian di lapangan." },
  { "en": "Apa itu 'containerization' (Docker)?", "id": "Membungkus aplikasi dalam lingkungan portabel." },
  { "en": "Potensi 5G di pabrik?", "id": "Komunikasi nirkabel latensi sangat rendah." },
  { "en": "Apa itu 'low-code platform'?", "id": "Platform pengembangan dengan sedikit koding." },
  { "en": "Apa itu 'generative AI'?", "id": "AI yang dapat membuat konten baru." },
  { "en": "Potensi AI untuk kode PLC?", "id": "Membantu menulis dan men-debug kode." },
  { "en": "Apa itu 'control theory'?", "id": "Ilmu matematika tentang perilaku sistem." },
  { "en": "Apa itu 'Laplace transform'?", "id": "Transformasi matematika untuk analisis sistem." },
  { "en": "Apa itu 'Bode plot'?", "id": "Grafik respons frekuensi suatu sistem." },
  { "en": "Apa itu 'gain margin'?", "id": "Ukuran stabilitas relatif sistem." },
  { "en": "Apa itu 'phase margin'?", "id": "Ukuran lain untuk stabilitas sistem." },
  { "en": "Apa itu 'state-space representation'?", "id": "Model matematis sistem dengan variabel state." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi dan penyaringan sinyal." },
  { "en": "Apa itu OEM?", "id": "Original Equipment Manufacturer." },
  { "en": "Peran OEM?", "id": "Membuat mesin atau peralatan asli." },
  { "en": "Apa itu 'system integrator' (SI)?", "id": "Perusahaan yang mengintegrasikan berbagai sistem." },
  { "en": "Peran SI?", "id": "Membangun sistem kontrol lengkap untuk klien." },
  { "en": "Apa itu 'proof of concept' (PoC)?", "id": "Proyek kecil untuk membuktikan kelayakan ide." },
  { "en": "Apa itu 'deprecation' software?", "id": "Menandai fitur sebagai usang." },
  { "en": "Lisensi 'runtime' vs 'development'?", "id": "Runtime untuk operasi, development untuk rekayasa." },
  { "en": "Apa itu 'dongle' lisensi?", "id": "Kunci perangkat keras untuk aktivasi software." },
  { "en": "Apa itu 'intellectual property' (IP)?", "id": "Kekayaan intelektual seperti kode program." },
  { "en": "Apa itu 'open-source software'?", "id": "Perangkat lunak dengan kode sumber terbuka." },
  { "en": "Apa itu EPLAN?", "id": "Perangkat lunak untuk desain skematik listrik." },
  { "en": "Apa itu 'datasheet'?", "id": "Dokumen spesifikasi teknis suatu produk." },
  { "en": "Apa itu 'beta testing'?", "id": "Pengujian produk oleh pengguna eksternal." },
  { "en": "Apa itu 'release candidate'?", "id": "Versi final yang siap dirilis." },
  { "en": "Apa itu 'patch' atau 'hotfix'?", "id": "Perbaikan cepat untuk bug spesifik." },
  { "en": "Apa itu 'service pack'?", "id": "Kumpulan pembaruan dan perbaikan." },
  { "en": "Apa itu 'time synchronization'?", "id": "Sinkronisasi waktu di seluruh perangkat jaringan." },
  { "en": "Protokol sinkronisasi waktu?", "id": "NTP (Network Time Protocol)." },
  { "en": "Mengapa sinkronisasi waktu penting?", "id": "Untuk urutan kejadian yang akurat." },
  { "en": "Apa itu 'fatal fault'?", "id": "Kesalahan yang menghentikan CPU PLC." },
  { "en": "Apa itu 'non-fatal fault'?", "id": "Kesalahan yang tidak menghentikan CPU." },
  { "en": "Apa itu 'CPU load'?", "id": "Persentase utilisasi prosesor PLC." },
  { "en": "Apa itu 'memory utilization'?", "id": "Persentase penggunaan memori PLC." },
  { "en": "Apa itu 'communication timeout'?", "id": "Error karena tidak ada respons komunikasi." },
  { "en": "Apa itu 'acknowledgement' (ACK)?", "id": "Sinyal konfirmasi penerimaan data." },
  { "en": "Apa itu 'negative acknowledgement' (NAK)?", "id": "Sinyal penolakan atau kegagalan data." },
  { "en": "Apa itu 'keep-alive signal'?", "id": "Sinyal periodik untuk memeriksa koneksi." },
  { "en": "Apa itu 'API key'?", "id": "Kunci otentikasi untuk menggunakan API." },
  { "en": "Apa itu 'REST API'?", "id": "Gaya arsitektur untuk layanan web." },
  { "en": "Apa itu 'JSON'?", "id": "JavaScript Object Notation." },
  { "en": "Fungsi JSON di otomasi?", "id": "Format pertukaran data yang ringan." },
  { "en": "Apa itu 'XML'?", "id": "eXtensible Markup Language." },
  { "en": "Apa itu 'CSV'?", "id": "Comma-Separated Values." },
  { "en": "Aplikasi file CSV?", "id": "Ekspor data log atau resep." },
  { "en": "Apa itu 'binary' file?", "id": "File yang tidak berbasis teks." },
  { "en": "Apa itu 'proprietary protocol'?", "id": "Protokol komunikasi milik satu vendor." },
  { "en": "Apa itu 'open protocol'?", "id": "Protokol standar yang terbuka untuk umum." },
  { "en": "Apa itu 'bus cycle time'?", "id": "Waktu untuk semua perangkat di bus berkomunikasi." },
  { "en": "Apa itu 'collision' di jaringan?", "id": "Tabrakan data saat dua perangkat mengirim bersamaan." },
  { "en": "Metode pencegah 'collision'?", "id": "CSMA/CD (Carrier Sense Multiple Access)." },
  { "en": "Apa itu 'simplex communication'?", "id": "Komunikasi satu arah saja." },
  { "en": "Apa itu 'half-duplex communication'?", "id": "Komunikasi dua arah, tapi bergantian." },
  { "en": "Apa itu 'full-duplex communication'?", "id": "Komunikasi dua arah secara bersamaan." },
  { "en": "Contoh 'half-duplex'?", "id": "Walkie-talkie." },
  { "en": "Contoh 'full-duplex'?", "id": "Telepon." },
  { "en": "Apa itu 'optical fiber'?", "id": "Kabel yang mentransmisikan data dengan cahaya." },
  { "en": "Keuntungan 'optical fiber'?", "id": "Kebal terhadap gangguan EMI." },
  { "en": "Apa itu 'multimode fiber'?", "id": "Serat optik untuk jarak pendek." },
  { "en": "Apa itu 'singlemode fiber'?", "id": "Serat optik untuk jarak jauh." },
  { "en": "Apa itu 'media converter'?", "id": "Mengubah satu jenis media ke lain." },
  { "en": "Contoh 'media converter'?", "id": "Tembaga (Ethernet) ke serat optik." },
  { "en": "Apa itu 'managed switch'?", "id": "Switch jaringan dengan fitur konfigurasi." },
  { "en": "Apa itu 'unmanaged switch'?", "id": "Switch jaringan 'plug-and-play' sederhana." },
  { "en": "Apa itu VLAN?", "id": "Virtual Local Area Network." },
  { "en": "Fungsi VLAN?", "id": "Memisahkan jaringan secara logis." },
  { "en": "Apa itu 'port mirroring'?", "id": "Menyalin trafik satu port ke port lain." },
  { "en": "Fungsi 'port mirroring'?", "id": "Untuk analisis dan monitoring jaringan." },
  { "en": "Apa itu 'power cycle'?", "id": "Mematikan dan menyalakan kembali perangkat." },
  { "en": "Apa itu 'checksum'?", "id": "Nilai untuk memverifikasi integritas data." },
  { "en": "Apa itu 'human-centric design'?", "id": "Desain yang berpusat pada kebutuhan manusia." },
  { "en": "Apa itu 'change management'?", "id": "Proses mengelola perubahan dalam organisasi." },
  { "en": "Apa itu 'upskilling'?", "id": "Meningkatkan keterampilan tenaga kerja." },
  { "en": "Apa itu 'servitization'?", "id": "Model bisnis menjual layanan, bukan produk." },
  { "en": "Apa itu 'cyber-physical system'?", "id": "Sistem yang mengintegrasikan komputasi dan proses fisik." }


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
            }, 8000);
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
