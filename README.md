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


{ "en": "Komponen Komputer Untuk Menyimpan Data?", "id": "Memori." },
{ "en": "Dua Tipe Utama Penyimpanan Memori?", "id": "Volatile Dan Non-Volatile." },
{ "en": "Apa Arti Dari Memori Volatile?", "id": "Data Hilang Saat Listrik Padam." },
{ "en": "Contoh Utama Memori Volatile?", "id": "RAM (Random Access Memory)." },
{ "en": "Apa Arti Dari Memori Non-Volatile?", "id": "Data Tersimpan Tanpa Aliran Listrik." },
{ "en": "Contoh Memori Non-Volatile?", "id": "HDD (Hard Disk Drive), SSD (Solid State Drive)." },
{ "en": "Satuan Informasi Digital Terkecil?", "id": "Bit." },
{ "en": "Satu Bit Dapat Bernilai Angka?", "id": "Nol Atau Satu." },
{ "en": "Satu Grup Terdiri Dari Delapan Bit?", "id": "Byte." },
{ "en": "Apa Kepanjangan Dari RAM?", "id": "Random Access Memory." },
{ "en": "Mengapa Disebut 'Random Access'?", "id": "Dapat Mengakses Data Di Alamat Manapun." },
{ "en": "Apa Kepanjangan Dari ROM?", "id": "Read-Only Memory." },
{ "en": "Apa Karakteristik Utama ROM (Read-Only Memory)?", "id": "Hanya Dapat Dibaca, Tidak Dapat Ditulis." },
{ "en": "Fungsi Utama RAM (Random Access Memory)?", "id": "Menyimpan Data Aplikasi Yang Sedang Berjalan." },
{ "en": "Satu Kilobyte (KB) Sama Dengan?", "id": "1024 Byte." },
{ "en": "Satu Megabyte (MB) Sama Dengan?", "id": "1024 Kilobyte (KB)." },
{ "en": "Satu Gigabyte (GB) Sama Dengan?", "id": "1024 Megabyte (MB)." },
{ "en": "Satu Terabyte (TB) Sama Dengan?", "id": "1024 Gigabyte (GB)." },
{ "en": "Memori Yang Bekerja Sebagai Ruang Kerja CPU?", "id": "RAM (Random Access Memory)." },
{ "en": "Penyimpanan Untuk Data Jangka Panjang?", "id": "Penyimpanan Sekunder." },
{ "en": "RAM (Random Access Memory) Adalah Memori?", "id": "Primer." },
{ "en": "Lokasi Unik Setiap Data Di Memori?", "id": "Alamat Memori." },
{ "en": "Proses Menyimpan Data Ke Memori?", "id": "Operasi Tulis (Write Operation)." },
{ "en": "Proses Mengambil Data Dari Memori?", "id": "Operasi Baca (Read Operation)." },
{ "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Di Perangkat Keras." },
{ "en": "Firmware Komputer Disimpan Di?", "id": "ROM (Read-Only Memory)." },
{ "en": "Contoh Firmware Komputer?", "id": "BIOS (Basic Input/Output System)." },
{ "en": "Tipe Memori Yang Digunakan Oleh USB Drive?", "id": "Flash Memory." },
{ "en": "Flash Memory Adalah Tipe Dari?", "id": "EEPROM (Electrically Erasable Programmable Read-Only Memory)." },
{ "en": "Dua Jenis Mendasar RAM (Random Access Memory)?", "id": "SRAM Dan DRAM." },
{ "en": "Apa Kepanjangan Dari SRAM?", "id": "Static Random Access Memory." },
{ "en": "Apa Kepanjangan Dari DRAM?", "id": "Dynamic Random Access Memory." },
{ "en": "Mana Yang Lebih Cepat Antara SRAM Dan DRAM?", "id": "SRAM (Static Random Access Memory)." },
{ "en": "Mana Yang Punya Kapasitas Lebih Besar?", "id": "DRAM (Dynamic Random Access Memory)." },
{ "en": "Cache CPU (Central Processing Unit) Menggunakan?", "id": "SRAM (Static Random Access Memory)." },
{ "en": "RAM (Random Access Memory) Utama Komputer Menggunakan?", "id": "DRAM (Dynamic Random Access Memory)." },
{ "en": "Mengapa DRAM (Dynamic Random Access Memory) Perlu Refresh?", "id": "Karena Muatan Kapasitornya Bocor." },
{ "en": "Apakah SRAM (Static Random Access Memory) Perlu Refresh?", "id": "Tidak, Selama Ada Daya Listrik." },
{ "en": "SRAM (Static Random Access Memory) Menggunakan Apa?", "id": "Sirkuit Flip-Flop Untuk Menyimpan Bit." },
{ "en": "DRAM (Dynamic Random Access Memory) Menggunakan Apa?", "id": "Transistor Dan Kapasitor." },
{ "en": "Hirarki Memori Mengurutkan Berdasarkan Apa?", "id": "Kecepatan, Ukuran, Dan Biaya." },
{ "en": "Level Teratas Hirarki Memori?", "id": "Register CPU (Central Processing Unit)." },
{ "en": "Semakin Ke Atas Hirarki, Memori Semakin?", "id": "Cepat Dan Kecil." },
{ "en": "Semakin Ke Bawah Hirarki, Memori Semakin?", "id": "Lambat Dan Besar." },
{ "en": "Prinsip Kerja Dibalik Hirarki Memori?", "id": "Prinsip Lokalitas." },
{ "en": "Apa Itu Lokalitas Temporal?", "id": "Data Yang Sama Akan Sering Diakses." },
{ "en": "Apa Itu Lokalitas Spasial?", "id": "Data Berdekatan Akan Diakses Bersamaan." },
{ "en": "Buffer Adalah Area Memori Untuk?", "id": "Penyimpanan Sementara Saat Transfer Data." },
{ "en": "Unit Penyimpanan Satu Bit Disebut?", "id": "Sel Memori." },
{ "en": "Apa Itu Word Size?", "id": "Ukuran Data Asli Prosesor." },
{ "en": "Bus Yang Membawa Alamat Memori?", "id": "Bus Alamat (Address Bus)." },
{ "en": "Bus Yang Membawa Data Aktual?", "id": "Bus Data (Data Bus)." },
{ "en": "Lebar Bus Alamat Menentukan Apa?", "id": "Jumlah Maksimum RAM Yang Didukung." },
{ "en": "Sistem 32-bit Mendukung RAM Hingga?", "id": "4 Gigabyte (GB)." },
{ "en": "Memori Penyimpanan Untuk Kartu Grafis?", "id": "VRAM (Video Random Access Memory)." },
{ "en": "Penyimpanan Yang Menggunakan Piringan Magnetik?", "id": "HDD (Hard Disk Drive)." },
{ "en": "Penyimpanan Yang Menggunakan Laser?", "id": "Penyimpanan Optik (CD, DVD)." },
{ "en": "Penyimpanan Tanpa Bagian Bergerak?", "id": "SSD (Solid State Drive)." },
{ "en": "Apa Itu Waktu Akses Memori?", "id": "Waktu Tunda Untuk Mengambil Data." },
{ "en": "Apa Itu Bandwidth Memori?", "id": "Kecepatan Transfer Data Maksimal." },
{ "en": "Memori Primer Bersifat?", "id": "Volatile (Umumnya)." },
{ "en": "Memori Sekunder Bersifat?", "id": "Non-Volatile." },
{ "en": "Bagaimana Cara Kerja Memori Magnetik?", "id": "Mengubah Arah Magnetik Partikel Kecil." },
{ "en": "Bagaimana Cara Kerja Memori Optik?", "id": "Membaca Pola Titik Dengan Laser." },
{ "en": "Bagaimana Cara Kerja Flash Memory?", "id": "Menjebak Elektron Di Dalam Sel." },
{ "en": "Siapa Yang Mengontrol Akses Ke RAM?", "id": "Memory Controller." },
{ "en": "Apa Itu Memori Cache?", "id": "Memori Cepat Antara CPU Dan RAM." },
{ "en": "Data Di Cache Adalah Salinan Dari?", "id": "Data Di RAM (Random Access Memory)." },
{ "en": "Apa Kepanjangan Dari SDRAM?", "id": "Synchronous Dynamic Random Access Memory." },
{ "en": "Apa Artinya 'Synchronous'?", "id": "Sinkron Dengan Clock Sistem." },
{ "en": "Apa Kepanjangan Dari DDR?", "id": "Double Data Rate." },
{ "en": "Apa Maksud Dari 'Double Data Rate'?", "id": "Transfer Data Dua Kali Per Siklus." },
{ "en": "DDR Mentransfer Pada Tepi Sinyal Clock?", "id": "Naik Dan Turun." },
{ "en": "RAM (Random Access Memory) Untuk Desktop?", "id": "DIMM (Dual In-line Memory Module)." },
{ "en": "RAM (Random Access Memory) Untuk Laptop?", "id": "SO-DIMM (Small Outline DIMM)." },
{ "en": "Apa Itu Konfigurasi 'Dual Channel'?", "id": "Menggunakan Dua Keping RAM Secara Paralel." },
{ "en": "Keuntungan 'Dual Channel'?", "id": "Menggandakan Bandwidth Memori." },
{ "en": "Apa Itu Konfigurasi 'Quad Channel'?", "id": "Menggunakan Empat Keping RAM Secara Paralel." },
{ "en": "Syarat Untuk 'Multi-Channel'?", "id": "Modul RAM Identik Atau Serupa." },
{ "en": "Apa Itu Latensi CAS (CL)?", "id": "Waktu Tunda Kolom Alamat Strobe." },
{ "en": "Nilai CL (CAS Latency) Yang Lebih Rendah?", "id": "Berarti Latensi Lebih Cepat." },
{ "en": "Apa Itu Timing RAM?", "id": "Serangkaian Angka Latensi (Contoh: CL16-18-18-38)." },
{ "en": "Kecepatan RAM Diukur Dalam Apa?", "id": "Megatransfer Per Detik (MT/s)." },
{ "en": "DDR4-3200 Memiliki Kecepatan Transfer?", "id": "3200 Megatransfer Per Detik." },
{ "en": "Clock Asli Dari DDR4-3200?", "id": "1600 Megahertz (MHz)." },
{ "en": "Apa Itu RAM ECC?", "id": "RAM Dengan Kode Koreksi Kesalahan." },
{ "en": "Apa Kepanjangan Dari ECC?", "id": "Error-Correcting Code." },
{ "en": "Fungsi RAM ECC (Error-Correcting Code)?", "id": "Mendeteksi Dan Memperbaiki Error Satu Bit." },
{ "en": "Di Mana RAM ECC (Error-Correcting Code) Digunakan?", "id": "Server Dan Workstation." },
{ "en": "Apa Itu RAM Registered (Buffered)?", "id": "Memiliki Register Untuk Menstabilkan Sinyal." },
{ "en": "RAM Registered (Buffered) Disebut Juga?", "id": "RDIMM." },
{ "en": "RAM Biasa Disebut?", "id": "UDIMM (Unbuffered DIMM)." },
{ "en": "Apa Itu XMP?", "id": "Extreme Memory Profile." },
{ "en": "Apa Fungsi Dari XMP (Extreme Memory Profile)?", "id": "Profil Overclocking RAM Otomatis." },
{ "en": "XMP (Extreme Memory Profile) Adalah Teknologi Dari?", "id": "Intel." },
{ "en": "Teknologi Serupa XMP Dari AMD?", "id": "EXPO (Extended Profiles for Overclocking)." },
{ "en": "Apa Itu SPD?", "id": "Serial Presence Detect." },
{ "en": "Apa Fungsi Dari SPD (Serial Presence Detect)?", "id": "Menyimpan Informasi Timing Standar RAM." },
{ "en": "Saat Booting, Komputer Membaca?", "id": "Chip SPD (Serial Presence Detect)." },
{ "en": "Memori Untuk Perangkat Mobile?", "id": "LPDDR (Low Power Double Data Rate)." },
{ "en": "Keunggulan LPDDR?", "id": "Konsumsi Daya Sangat Rendah." },
{ "en": "Generasi Memori DDR Tidak Kompatibel?", "id": "Benar, DDR3 Tidak Cocok Di Slot DDR4." },
{ "en": "Apa Itu Sel Memori DRAM?", "id": "Unit Dasar Penyimpanan Satu Bit." },
{ "en": "Terdiri Dari Apa Sel DRAM Itu?", "id": "Satu Kapasitor Dan Satu Transistor." },
{ "en": "Apa Itu Sel Memori SRAM?", "id": "Unit Penyimpanan Satu Bit Di SRAM." },
{ "en": "Terdiri Dari Apa Sel SRAM Itu?", "id": "Empat Hingga Enam Transistor (Flip-Flop)." },
{ "en": "Siklus Refresh Pada DRAM Terjadi?", "id": "Setiap Beberapa Milidetik." },
{ "en": "Operasi Refresh Mengkonsumsi?", "id": "Daya Dan Waktu." },
{ "en": "Apa Itu 'Bank' Memori?", "id": "Grup Chip Memori Di Dalam Modul." },
{ "en": "Apa Itu 'Rank' Memori?", "id": "Blok Data 64-bit Di Dalam Modul." },
{ "en": "Memori 'Single Rank' vs 'Dual Rank'?", "id": "Jumlah Rank Dalam Satu Modul." },
{ "en": "Dual Rank Bisa Memberi Performa?", "id": "Sedikit Lebih Baik." },
{ "en": "Apa Itu 'Memory Interleaving'?", "id": "Teknik Untuk Meningkatkan Kinerja Memori." },
{ "en": "Bagaimana Cara Kerja 'Interleaving'?", "id": "Menyebar Akses Ke Beberapa Bank Atau Channel." },
{ "en": "DDR Adalah Evolusi Dari?", "id": "SDRAM (Synchronous Dynamic Random Access Memory)." },
{ "en": "Tegangan Standar DDR5?", "id": "1.1 Volt." },
{ "en": "Tegangan Standar DDR4?", "id": "1.2 Volt." },
{ "en": "Tegangan Standar DDR3?", "id": "1.5 Volt." },
{ "en": "Apa Itu DDR3L?", "id": "Varian DDR3 Tegangan Rendah (1.35V)." },
{ "en": "Modul RAM Memiliki Kontak Di?", "id": "Kedua Sisi (Dual In-line)." },
{ "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Untuk Deteksi Error Sederhana." },
{ "en": "Apakah Parity Bisa Memperbaiki Error?", "id": "Tidak, Hanya Mendeteksi." },
{ "en": "ECC (Error-Correcting Code) Bisa Memperbaiki Error?", "id": "Ya, Error Satu Bit." },
{ "en": "Bagaimana RAM Dipasang Di Motherboard?", "id": "Dengan Menekan Ke Dalam Slot Hingga Klip Mengunci." },
{ "en": "Panas Adalah Musuh Dari?", "id": "RAM (Random Access Memory)." },
{ "en": "Penyebar Panas Pada RAM Disebut?", "id": "Heatspreader." },
{ "en": "Apakah Semua RAM Memiliki Heatspreader?", "id": "Tidak, Terutama RAM Standar." },
{ "en": "RAM Dengan Lampu RGB (Red Green Blue) Untuk?", "id": "Estetika." },
{ "en": "Kapasitas RAM Umum Untuk PC Modern?", "id": "16 GB Hingga 32 GB." },
{ "en": "Berapa Banyak Bit Dalam Satu 'Word' DDR?", "id": "64 Bit." },
{ "en": "Berapa Banyak Bit Termasuk ECC?", "id": "72 Bit." },
{ "en": "Chip Memori Dibuat Oleh Perusahaan Seperti?", "id": "Samsung, Micron, SK Hynix." },
{ "en": "Modul RAM Dirakit Oleh Perusahaan Seperti?", "id": "Corsair, Kingston, G.Skill." },
{ "en": "Mereka Membeli Chip Lalu Merakitnya?", "id": "Benar." },
{ "en": "RAM Yang Disolder Langsung Ke Papan Sirkuit?", "id": "Umum Di Laptop Tipis Dan Smartphone." },
{ "en": "Kelemahannya?", "id": "Tidak Dapat Di-Upgrade." },
{ "en": "Apa Itu 'Row Hammer'?", "id": "Isu Keamanan Di Mana Sel DRAM Bisa Berubah." },
{ "en": "Penyebabnya?", "id": "Mengakses Satu Baris Berulang Kali Terlalu Cepat." },
{ "en": "Memori Yang Isinya Permanen Dari Pabrik?", "id": "ROM (Read-Only Memory)." },
{ "en": "ROM (Read-Only Memory) Jenis Mask ROM?", "id": "Data Ditulis Saat Fabrikasi Chip." },
{ "en": "Apa Kepanjangan Dari PROM?", "id": "Programmable Read-Only Memory." },
{ "en": "Siapa Yang Memprogram PROM (Programmable ROM)?", "id": "Pengguna, Bukan Pabrik." },
{ "en": "PROM (Programmable ROM) Bisa Diprogram Ulang?", "id": "Tidak, Hanya Satu Kali." },
{ "en": "Prosesnya Disebut 'Burning a PROM'?", "id": "Benar." },
{ "en": "Apa Kepanjangan Dari EPROM?", "id": "Erasable Programmable Read-Only Memory." },
{ "en": "Bagaimana Cara Menghapus Data Di EPROM?", "id": "Dengan Paparan Sinar Ultraviolet (UV)." },
{ "en": "Bagian Khas Pada Chip EPROM?", "id": "Jendela Kaca Kuarsa Transparan." },
{ "en": "Setelah Dihapus, EPROM Bisa Diprogram?", "id": "Kembali." },
{ "en": "Apa Kepanjangan Dari EEPROM?", "id": "Electrically Erasable Programmable Read-Only Memory." },
{ "en": "Bagaimana Cara Menghapus Data Di EEPROM?", "id": "Menggunakan Sinyal Listrik." },
{ "en": "Keunggulan Utama EEPROM?", "id": "Dapat Dihapus Tanpa Dilepas Dari Sirkuit." },
{ "en": "Flash Memory Adalah Jenis Dari?", "id": "EEPROM (Electrically Erasable Programmable Read-Only Memory)." },
{ "en": "Apa Yang Disimpan Di ROM Komputer?", "id": "Firmware Seperti BIOS Atau UEFI." },
{ "en": "Apa Kepanjangan BIOS?", "id": "Basic Input/Output System." },
{ "en": "Fungsi Utama BIOS (Basic Input/Output System)?", "id": "Memulai Dan Menguji Hardware Saat Boot." },
{ "en": "Apa Itu POST?", "id": "Power-On Self-Test." },
{ "en": "Siapa Yang Menjalankan POST (Power-On Self-Test)?", "id": "BIOS (Basic Input/Output System)." },
{ "en": "Apa Pengganti Modern Untuk BIOS?", "id": "UEFI (Unified Extensible Firmware Interface)." },
{ "en": "Apa Kepanjangan UEFI?", "id": "Unified Extensible Firmware Interface." },
{ "en": "Firmware Adalah Kombinasi Dari?", "id": "Perangkat Keras Dan Perangkat Lunak." },
{ "en": "Firmware Di Router Menyimpan?", "id": "Sistem Operasi Router." },
{ "en": "Pembaruan Firmware Dilakukan Untuk?", "id": "Menambah Fitur Dan Keamanan." },
{ "en": "Memori Yang Menyimpan Pengaturan BIOS?", "id": "CMOS (Complementary Metal-Oxide-Semiconductor) RAM." },
{ "en": "Apa Yang Memberi Daya Pada CMOS RAM?", "id": "Baterai Koin Di Motherboard." },
{ "en": "Jika Baterai CMOS Habis?", "id": "Pengaturan BIOS Kembali Ke Default." },
{ "en": "Flash Memory NAND Digunakan Untuk?", "id": "Penyimpanan Data (SSD, Kartu SD)." },
{ "en": "Flash Memory NOR Digunakan Untuk?", "id": "Eksekusi Kode (Firmware)." },
{ "en": "Mana Yang Lebih Cepat Untuk Membaca?", "id": "NOR Flash." },
{ "en": "Mana Yang Lebih Cepat Untuk Menulis Dan Menghapus?", "id": "NAND Flash." },
{ "en": "SSD (Solid State Drive) Menggunakan?", "id": "NAND Flash Memory." },
{ "en": "Apa Kepanjangan Dari SLC?", "id": "Single-Level Cell." },
{ "en": "SLC (Single-Level Cell) Menyimpan Berapa Bit?", "id": "Satu Bit Per Sel." },
{ "en": "Keunggulan SLC (Single-Level Cell)?", "id": "Paling Cepat Dan Tahan Lama." },
{ "en": "Apa Kepanjangan Dari MLC?", "id": "Multi-Level Cell." },
{ "en": "MLC (Multi-Level Cell) Menyimpan Berapa Bit?", "id": "Dua Bit Per Sel." },
{ "en": "Apa Kepanjangan Dari TLC?", "id": "Triple-Level Cell." },
{ "en": "TLC (Triple-Level Cell) Menyimpan Berapa Bit?", "id": "Tiga Bit Per Sel." },
{ "en": "Apa Kepanjangan Dari QLC?", "id": "Quad-Level Cell." },
{ "en": "QLC (Quad-Level Cell) Menyimpan Berapa Bit?", "id": "Empat Bit Per Sel." },
{ "en": "Semakin Banyak Level Per Sel?", "id": "Semakin Murah Dan Padat." },
{ "en": "Tetapi Juga Semakin?", "id": "Lambat Dan Kurang Tahan Lama." },
{ "en": "Daya Tahan Flash Memory Diukur Dalam?", "id": "Siklus Program/Hapus (P/E Cycles)." },
{ "en": "Teknik Untuk Meratakan Penggunaan Sel Flash?", "id": "Wear Leveling." },
{ "en": "Siapa Yang Melakukan Wear Leveling?", "id": "Kontroler SSD (Solid State Drive)." },
{ "en": "Apa Itu 'Floating Gate'?", "id": "Tempat Elektron Dijebak Di Sel Flash." },
{ "en": "Bagaimana Data Dibaca Dari Sel Flash?", "id": "Dengan Mengukur Level Muatan Di Gate." },
{ "en": "ROM Juga Ditemukan Di Perangkat Elektronik?", "id": "Ya, Seperti Microwave Dan Mesin Cuci." },
{ "en": "Firmware Di Perangkat Ini Mengontrol?", "id": "Operasi Dasar Perangkat." },
{ "en": "MRAM Singkatan Dari?", "id": "Magnetoresistive Random Access Memory." },
{ "en": "FeRAM Singkatan Dari?", "id": "Ferroelectric Random Access Memory." },
{ "en": "MRAM Dan FeRAM Adalah Memori?", "id": "Non-Volatile Yang Cepat." },
{ "en": "Mereka Berpotensi Menggantikan?", "id": "DRAM Dan Flash." },
{ "en": "Apa Itu 'Shadow RAM'?", "id": "Menyalin Isi ROM Ke RAM Saat Boot." },
{ "en": "Tujuannya?", "id": "Akses Lebih Cepat." },
{ "en": "Apakah ROM Bisa Rusak?", "id": "Sangat Jarang, Karena Solid-State." },
{ "en": "Kerusakan Firmware Disebut?", "id": "Bricking (Perangkat Jadi Batu Bata)." },
{ "en": "Bisa Terjadi Saat?", "id": "Proses Update Firmware Gagal." },
{ "en": "Apa Itu Hirarki Memori?", "id": "Sistem Level Memori Berdasarkan Kinerja." },
{ "en": "Tujuan Dari Hirarki Memori?", "id": "Kombinasi Kecepatan, Kapasitas, Dan Biaya." },
{ "en": "Level Teratas Dan Tercepat?", "id": "Register CPU (Central Processing Unit)." },
{ "en": "Di Mana Lokasi Register?", "id": "Langsung Di Dalam Inti CPU." },
{ "en": "Level Di Bawah Register?", "id": "Cache CPU (Central Processing Unit)." },
{ "en": "Bahan Pembuat Cache?", "id": "SRAM (Static Random Access Memory)." },
{ "en": "Cache Level 1 (L1)?", "id": "Cache Terkecil Dan Tercepat." },
{ "en": "Cache L1 (Level 1) Biasanya Terpisah Untuk?", "id": "Data Dan Instruksi." },
{ "en": "Cache Level 2 (L2)?", "id": "Lebih Besar Dan Lambat Dari L1." },
{ "en": "Cache Level 3 (L3)?", "id": "Paling Besar, Paling Lambat, Dibagi Antar Core." },
{ "en": "Tujuan Cache?", "id": "Mengurangi Waktu Tunggu CPU Ke RAM." },
{ "en": "Level Di Bawah Cache?", "id": "Memori Utama Atau RAM (Random Access Memory)." },
{ "en": "Bahan Pembuat RAM?", "id": "DRAM (Dynamic Random Access Memory)." },
{ "en": "Level Di Bawah Memori Utama?", "id": "Penyimpanan Sekunder (Mass Storage)." },
{ "en": "Contoh Penyimpanan Sekunder?", "id": "SSD (Solid State Drive), HDD (Hard Disk Drive)." },
{ "en": "Level Paling Bawah?", "id": "Penyimpanan Tersier Atau Offline." },
{ "en": "Contoh Penyimpanan Tersier?", "id": "Pita Magnetik (Magnetic Tape)." },
{ "en": "Prinsip Utama Dibalik Hirarki?", "id": "Prinsip Lokalitas Referensi." },
{ "en": "Dua Jenis Lokalitas?", "id": "Temporal Dan Spasial." },
{ "en": "Lokalitas Temporal Berarti?", "id": "Data Yang Baru Dipakai Akan Dipakai Lagi." },
{ "en": "Lokalitas Spasial Berarti?", "id": "Data Yang Berdekatan Akan Dipakai Bersamaan." },
{ "en": "Cache Mengeksploitasi Kedua Lokalitas?", "id": "Benar." },
{ "en": "Apa Itu 'Cache Hit'?", "id": "Data Yang Dicari Ditemukan Di Cache." },
{ "en": "Apa Itu 'Cache Miss'?", "id": "Data Tidak Ditemukan, Harus Mengambil Dari RAM." },
{ "en": "Rasio Hit (Hit Ratio)?", "id": "Persentase Hit Terhadap Total Akses." },
{ "en": "Rasio Hit Yang Tinggi Berarti?", "id": "Kinerja Sistem Yang Baik." },
{ "en": "Penalti Untuk Cache Miss?", "id": "Waktu Tunggu Yang Signifikan." },
{ "en": "CPU (Central Processing Unit) Berbicara Dengan?", "id": "Cache." },
{ "en": "Cache Berbicara Dengan?", "id": "RAM (Random Access Memory)." },
{ "en": "RAM (Random Access Memory) Berbicara Dengan?", "id": "Penyimpanan Sekunder." },
{ "en": "Semakin Ke Bawah Hirarki, Biaya Per Bit?", "id": "Semakin Murah." },
{ "en": "Semakin Ke Bawah Hirarki, Waktu Akses?", "id": "Semakin Lambat." },
{ "en": "Semakin Ke Bawah Hirarki, Kapasitas?", "id": "Semakin Besar." },
{ "en": "Register Menyimpan Data Untuk Operasi?", "id": "Yang Sedang Berlangsung Saat Ini." },
{ "en": "Cache Menyimpan Data Untuk Operasi?", "id": "Yang Kemungkinan Segera Dibutuhkan." },
{ "en": "RAM (Random Access Memory) Menyimpan Data?", "id": "Program Dan Data Yang Aktif." },
{ "en": "SSD/HDD Menyimpan Data?", "id": "Semua Program Dan File Pengguna." },
{ "en": "Kesenjangan Kecepatan Antara CPU Dan RAM?", "id": "Sangat Besar Dan Terus Melebar." },
{ "en": "Cache Sangat Penting Untuk Menjembatani?", "id": "Kesenjangan Kecepatan Ini." },
{ "en": "Apa Itu 'Cache Line'?", "id": "Unit Data Terkecil Yang Ditransfer Cache." },
{ "en": "Ukuran Cache Line Umumnya?", "id": "64 Byte." },
{ "en": "Saat Cache Miss, Apa Yang Dibawa Dari RAM?", "id": "Satu Cache Line Penuh." },
{ "en": "Ini Mengeksploitasi Lokalitas Apa?", "id": "Lokalitas Spasial." },
{ "en": "Kebijakan Penulisan Cache (Write Policy)?", "id": "Write-Through Dan Write-Back." },
{ "en": "Apa Itu 'Write-Through'?", "id": "Data Ditulis Ke Cache Dan RAM Sekaligus." },
{ "en": "Apa Itu 'Write-Back'?", "id": "Data Ditulis Ke RAM Nanti Saat Dibutuhkan." },
{ "en": "Mana Yang Lebih Cepat?", "id": "Write-Back." },
{ "en": "Kebijakan Penggantian Cache (Replacement Policy)?", "id": "LRU (Least Recently Used), FIFO (First-In, First-Out)." },
{ "en": "Apa Itu LRU (Least Recently Used)?", "id": "Mengganti Data Yang Paling Lama Tidak Digunakan." },
{ "en": "Hirarki Memori Adalah Kompromi?", "id": "Antara Kinerja Dan Biaya." },
{ "en": "Tanpa Cache, Komputer Modern Akan?", "id": "Sangat Lambat." },
{ "en": "Memori Cepat Antara CPU Dan RAM?", "id": "Cache." },
{ "en": "Tujuan Utama Cache?", "id": "Mempercepat Akses Data." },
{ "en": "Cache Terbuat Dari Apa?", "id": "SRAM (Static Random Access Memory)." },
{ "en": "Cache Terkecil Dan Tercepat?", "id": "Cache L1 (Level 1)." },
{ "en": "Di Mana Lokasi Cache L1?", "id": "Di Dalam Setiap Core CPU." },
{ "en": "Cache L1 Dibagi Menjadi?", "id": "L1 Data Cache Dan L1 Instruction Cache." },
{ "en": "Cache Di Level Selanjutnya?", "id": "Cache L2 (Level 2)." },
{ "en": "Cache L2 Lebih Besar Dari?", "id": "L1, Tapi Lebih Lambat." },
{ "en": "Cache L2 Biasanya Milik?", "id": "Setiap Core Secara Individual." },
{ "en": "Cache Terbesar Di CPU?", "id": "Cache L3 (Level 3)." },
{ "en": "Cache L3 Dibagi Oleh?", "id": "Semua Core Dalam CPU." },
{ "en": "Cache L3 Disebut Juga?", "id": "Last-Level Cache (LLC)." },
{ "en": "Ketika CPU Mencari Data, Pertama Kali?", "id": "Mencari Di Cache L1." },
{ "en": "Jika Data Ditemukan Di Cache?", "id": "Disebut 'Cache Hit'." },
{ "en": "Jika Data Tidak Ditemukan Di Cache?", "id": "Disebut 'Cache Miss'." },
{ "en": "Jika Miss Di L1, CPU Mencari Di?", "id": "Cache L2." },
{ "en": "Jika Miss Di L2, CPU Mencari Di?", "id": "Cache L3." },
{ "en": "Jika Miss Di L3, CPU Harus?", "id": "Mengambil Dari RAM (Random Access Memory)." },
{ "en": "Penalti Waktu Untuk Cache Miss?", "id": "Sangat Besar." },
{ "en": "Satuan Transfer Data Antara Cache Dan RAM?", "id": "Cache Line Atau Cache Block." },
{ "en": "Ukuran Cache Line Umumnya?", "id": "64 Byte." },
{ "en": "Apa Itu Asosiativitas Cache?", "id": "Fleksibilitas Di Mana Data Dapat Disimpan." },
{ "en": "Jenis Asosiativitas Cache?", "id": "Direct Mapped, Fully Associative, Set-Associative." },
{ "en": "Cache 'Direct Mapped'?", "id": "Setiap Blok Memori Hanya Bisa Ke Satu Lokasi Cache." },
{ "en": "Keuntungan Direct Mapped?", "id": "Sederhana Dan Cepat." },
{ "en": "Kelemahan Direct Mapped?", "id": "Rasio Miss Bisa Tinggi." },
{ "en": "Cache 'Fully Associative'?", "id": "Setiap Blok Memori Bisa Ke Lokasi Manapun." },
{ "en": "Keuntungan Fully Associative?", "id": "Rasio Miss Paling Rendah." },
{ "en": "Kelemahan Fully Associative?", "id": "Sangat Kompleks Dan Lambat." },
{ "en": "Cache 'N-way Set-Associative'?", "id": "Kompromi Antara Keduanya." },
{ "en": "Ini Adalah Jenis Cache Yang Paling?", "id": "Umum Digunakan." },
{ "en": "Apa Itu 'Cache Coherence'?", "id": "Menjaga Konsistensi Data Antar Cache Core." },
{ "en": "Masalah Ini Muncul Di Sistem?", "id": "Multi-Core." },
{ "en": "Protokol Untuk Menjaga Koherensi?", "id": "MESI (Modified, Exclusive, Shared, Invalid)." },
{ "en": "Kebijakan Penggantian Blok Cache?", "id": "Menentukan Blok Mana Yang Harus Dikeluarkan." },
{ "en": "Kebijakan LRU (Least Recently Used)?", "id": "Mengeluarkan Blok Yang Paling Lama Tidak Digunakan." },
{ "en": "Kebijakan FIFO (First-In, First-Out)?", "id": "Mengeluarkan Blok Yang Paling Lama Masuk." },
{ "en": "Kebijakan Penulisan 'Write-Through'?", "id": "Menulis Ke Cache Dan RAM Secara Bersamaan." },
{ "en": "Kebijakan Penulisan 'Write-Back'?", "id": "Menulis Ke RAM Hanya Saat Blok Diganti." },
{ "en": "Mana Yang Lebih Kompleks?", "id": "Write-Back." },
{ "en": "Mana Yang Punya Kinerja Tulis Lebih Baik?", "id": "Write-Back." },
{ "en": "Cache Inklusif Berarti?", "id": "Isi Cache Level Bawah Ada Di Level Atas." },
{ "en": "Cache Eksklusif Berarti?", "id": "Isi Setiap Level Cache Adalah Unik." },
{ "en": "Apa Itu 'Victim Cache'?", "id": "Cache Kecil Untuk Menyimpan Blok Yang Dikeluarkan." },
{ "en": "Tujuannya?", "id": "Mengurangi Penalti Miss." },
{ "en": "AMD 3D V-Cache Adalah?", "id": "Cache L3 Tambahan Yang Ditumpuk Vertikal." },
{ "en": "Apakah GPU (Graphics Processing Unit) Memiliki Cache?", "id": "Ya, L1 Dan L2." },
{ "en": "Cache GPU Dioptimalkan Untuk?", "id": "Throughput Tinggi, Bukan Latensi Rendah." },
{ "en": "Apa Itu 'Prefetching'?", "id": "Mengambil Data Ke Cache Sebelum Dibutuhkan." },
{ "en": "Didasarkan Pada Prediksi?", "id": "Pola Akses Memori." },
{ "en": "Ukuran Cache L3 Modern?", "id": "Puluhan Hingga Ratusan Megabyte." },
{ "en": "Jenis Penyimpanan Sekunder Yang Umum?", "id": "HDD (Hard Disk Drive) Dan SSD (Solid State Drive)." },
{ "en": "Apa Kepanjangan HDD?", "id": "Hard Disk Drive." },
{ "en": "HDD (Hard Disk Drive) Menggunakan Teknologi?", "id": "Penyimpanan Magnetik." },
{ "en": "Komponen Utama HDD?", "id": "Platter, Spindle, Read/Write Head, Actuator." },
{ "en": "Apa Itu Platter?", "id": "Piringan Logam Atau Kaca Yang Dilapisi Bahan Magnetik." },
{ "en": "Platter Berputar Dengan Kecepatan Tinggi?", "id": "Benar." },
{ "en": "Motor Yang Memutar Platter?", "id": "Spindle." },
{ "en": "Kecepatan Putar HDD Diukur Dalam?", "id": "RPM (Rotations Per Minute)." },
{ "en": "Kecepatan RPM Yang Umum?", "id": "5400 RPM Atau 7200 RPM." },
{ "en": "Semakin Tinggi RPM, Kinerja HDD?", "id": "Semakin Cepat." },
{ "en": "Komponen Untuk Membaca Dan Menulis Data?", "id": "Read/Write Head." },
{ "en": "Apakah Head Menyentuh Platter?", "id": "Tidak, Mengambang Sangat Dekat." },
{ "en": "Lengan Yang Menggerakkan Head?", "id": "Actuator Arm." },
{ "en": "Data Disimpan Di Platter Dalam Bentuk?", "id": "Jejak Konsentris Yang Disebut Track." },
{ "en": "Setiap Track Dibagi Lagi Menjadi?", "id": "Sektor (Sector)." },
{ "en": "Ukuran Satu Sektor Biasanya?", "id": "512 Byte Atau 4 Kilobyte." },
{ "en": "Apa Itu 'Seek Time'?", "id": "Waktu Untuk Menggerakkan Head Ke Track Yang Benar." },
{ "en": "Apa Itu 'Rotational Latency'?", "id": "Waktu Tunggu Platter Berputar Ke Sektor Yang Benar." },
{ "en": "Waktu Akses HDD Adalah Penjumlahan Dari?", "id": "Seek Time Dan Rotational Latency." },
{ "en": "HDD (Hard Disk Drive) Bersifat Non-Volatile?", "id": "Benar." },
{ "en": "HDD (Hard Disk Drive) Rentan Terhadap?", "id": "Guncangan Fisik Dan Magnet Kuat." },
{ "en": "Mengapa Rentan Guncangan?", "id": "Karena Memiliki Bagian Yang Bergerak." },
{ "en": "Suara 'Klik' Dari HDD Adalah Tanda?", "id": "Kerusakan Fisik (Click of Death)." },
{ "en": "Apa Itu Defragmentasi?", "id": "Menyusun Ulang File Yang Terfragmentasi." },
{ "en": "Apa Itu Fragmentasi File?", "id": "Bagian File Tersimpan Di Lokasi Terpisah." },
{ "en": "Fragmentasi Memperlambat HDD Karena?", "id": "Head Harus Bergerak Lebih Banyak." },
{ "en": "Faktor Bentuk (Form Factor) HDD Desktop?", "id": "3.5 Inci." },
{ "en": "Faktor Bentuk HDD Laptop?", "id": "2.5 Inci." },
{ "en": "Antarmuka Koneksi HDD Yang Umum?", "id": "SATA (Serial AT Attachment)." },
{ "en": "Apa Kepanjangan Dari S.M.A.R.T.?", "id": "Self-Monitoring, Analysis, and Reporting Technology." },
{ "en": "Fungsi S.M.A.R.T.?", "id": "Memprediksi Kegagalan Hard Disk." },
{ "en": "Cache Pada HDD Disebut?", "id": "Disk Buffer." },
{ "en": "Fungsi Disk Buffer?", "id": "Menyimpan Data Yang Sering Diakses Sementara." },
{ "en": "Teknologi Perekaman HDD?", "id": "CMR (Conventional) Dan SMR (Shingled)." },
{ "en": "Apa Itu SMR (Shingled Magnetic Recording)?", "id": "Track Saling Tumpang Tindih Seperti Sirap." },
{ "en": "Keuntungan SMR (Shingled Magnetic Recording)?", "id": "Kepadatan Data Lebih Tinggi." },
{ "en": "Kelemahan SMR (Shingled Magnetic Recording)?", "id": "Kinerja Tulis Acak Lebih Lambat." },
{ "en": "HDD (Hard Disk Drive) Helium-Filled?", "id": "Mengurangi Gesekan Internal, Lebih Efisien." },
{ "en": "HDD (Hard Disk Drive) Lebih Cocok Untuk?", "id": "Penyimpanan Data Kapasitas Besar Dan Murah." },
{ "en": "Contoh Penggunaan HDD?", "id": "Arsip Data, Server Media, Backup." },
{ "en": "Apa Kepanjangan Dari SSD?", "id": "Solid State Drive." },
{ "en": "SSD (Solid State Drive) Menggunakan Memori?", "id": "Flash Memory Tipe NAND." },
{ "en": "Apakah SSD Memiliki Bagian Bergerak?", "id": "Tidak." },
{ "en": "Keunggulan Utama SSD Dibanding HDD?", "id": "Jauh Lebih Cepat." },
{ "en": "Keunggulan Lain SSD?", "id": "Lebih Tahan Guncangan, Hening, Hemat Daya." },
{ "en": "SSD (Solid State Drive) Bersifat Non-Volatile?", "id": "Benar." },
{ "en": "Waktu Akses SSD Sangat?", "id": "Rendah." },
{ "en": "Antarmuka Koneksi Untuk SSD?", "id": "SATA Dan NVMe." },
{ "en": "Apa Kepanjangan Dari SATA?", "id": "Serial AT Attachment." },
{ "en": "SATA (Serial AT Attachment) SSD Memiliki Faktor Bentuk?", "id": "2.5 Inci." },
{ "en": "Kecepatan SATA (Serial AT Attachment) 3 Terbatas Hingga?", "id": "Sekitar 550 Megabyte Per Detik." },
{ "en": "Apa Kepanjangan Dari NVMe?", "id": "Non-Volatile Memory Express." },
{ "en": "NVMe Adalah Protokol Yang Dirancang Untuk?", "id": "Flash Memory." },
{ "en": "NVMe (Non-Volatile Memory Express) Menggunakan Jalur?", "id": "PCIe (Peripheral Component Interconnect Express)." },
{ "en": "Kecepatan NVMe (Non-Volatile Memory Express) Dibanding SATA?", "id": "Jauh Lebih Cepat (Bisa 10x Lipat)." },
{ "en": "Faktor Bentuk SSD NVMe Yang Umum?", "id": "M.2." },
{ "en": "Slot M.2 Terhubung Langsung Ke?", "id": "Motherboard." },
{ "en": "Komponen Utama SSD?", "id": "Kontroler Dan Chip Flash NAND." },
{ "en": "Fungsi Kontroler SSD?", "id": "Manajemen Data, Wear Leveling, Garbage Collection." },
{ "en": "Apa Itu 'Garbage Collection' Di SSD?", "id": "Membersihkan Blok Data Yang Tidak Terpakai." },
{ "en": "Apa Itu Perintah TRIM?", "id": "OS Memberi Tahu SSD Blok Mana Yang Kosong." },
{ "en": "Tujuan TRIM?", "id": "Menjaga Kinerja Tulis SSD Tetap Cepat." },
{ "en": "Apakah SSD Perlu Defragmentasi?", "id": "Tidak, Justru Buruk Untuk Umur SSD." },
{ "en": "Mengapa Defrag Buruk Untuk SSD?", "id": "Karena Menyebabkan Siklus Tulis Yang Tidak Perlu." },
{ "en": "SSD (Solid State Drive) Memiliki Cache?", "id": "Ya, Biasanya Menggunakan Chip DRAM." },
{ "en": "Fungsi Cache DRAM Di SSD?", "id": "Menyimpan Peta Alamat Data, Mempercepat Penulisan." },
{ "en": "SSD (Solid State Drive) Tanpa DRAM Disebut?", "id": "DRAM-less SSD." },
{ "en": "Kinerjanya?", "id": "Lebih Lambat, Terutama Saat Beban Berat." },
{ "en": "Apa Itu SLC Caching?", "id": "Menggunakan Sebagian TLC/QLC Sebagai SLC Cepat." },
{ "en": "Tujuannya?", "id": "Meningkatkan Kinerja Tulis Jangka Pendek." },
{ "en": "Kelemahan Flash Memory?", "id": "Jumlah Siklus Tulis Terbatas." },
{ "en": "Daya Tahan SSD Diukur Dalam?", "id": "TBW (Terabytes Written)." },
{ "en": "Apa Arti 300 TBW?", "id": "Drive Tahan Ditulisi 300 Terabyte Data." },
{ "en": "Untuk Pengguna Biasa, TBW SSD?", "id": "Sangat Cukup Untuk Bertahun-tahun." },
{ "en": "SSD (Solid State Drive) Lebih Mahal Per Gigabyte?", "id": "Ya, Dibandingkan HDD (Hard Disk Drive)." },
{ "en": "Penggunaan Ideal SSD?", "id": "Drive Sistem Operasi Dan Aplikasi." },
{ "en": "Penggunaan Ideal HDD?", "id": "Penyimpanan File Besar Seperti Film." },
{ "en": "Kombinasi SSD Dan HDD Disebut?", "id": "Penyimpanan Hibrida." },
{ "en": "Apa Itu SSHD (Solid State Hybrid Drive)?", "id": "HDD Dengan Cache NAND Flash Kecil." },
{ "en": "Jenis NAND Flash Paling Tahan Lama?", "id": "SLC (Single-Level Cell)." },
{ "en": "Jenis NAND Flash Paling Murah?", "id": "QLC (Quad-Level Cell)." },
{ "en": "SSD (Solid State Drive) Optane Dari Intel Menggunakan?", "id": "Teknologi 3D XPoint." },
{ "en": "Keunggulan Optane?", "id": "Latensi Sangat Rendah Dan Daya Tahan Tinggi." },
{ "en": "Bentuk Fisik Modul RAM Disebut?", "id": "Form Factor." },
{ "en": "Modul RAM Untuk PC Desktop?", "id": "DIMM (Dual In-line Memory Module)." },
{ "en": "Modul RAM Untuk Laptop?", "id": "SO-DIMM (Small Outline DIMM)." },
{ "en": "Berapa Pin Pada DIMM DDR4?", "id": "288 Pin." },
{ "en": "Berapa Pin Pada DIMM DDR5?", "id": "288 Pin (Tapi Notch Berbeda)." },
{ "en": "Berapa Pin Pada SO-DIMM DDR4?", "id": "260 Pin." },
{ "en": "Apa Itu 'Memory Channel'?", "id": "Jalur Komunikasi Antara CPU Dan RAM." },
{ "en": "Satu Modul RAM Berjalan Dalam Mode?", "id": "Single-Channel." },
{ "en": "Dua Modul RAM Bisa Berjalan Dalam?", "id": "Dual-Channel." },
{ "en": "Empat Modul RAM Bisa Berjalan Dalam?", "id": "Quad-Channel." },
{ "en": "Dual-Channel Menggandakan?", "id": "Bandwidth Memori." },
{ "en": "Bagaimana Cara Mengaktifkan Dual-Channel?", "id": "Pasang RAM Pada Slot Berwarna Sama." },
{ "en": "Motherboard Konsumen Umumnya Mendukung?", "id": "Dual-Channel." },
{ "en": "Motherboard HEDT (High-End Desktop) Mendukung?", "id": "Quad-Channel." },
{ "en": "Faktor Bentuk HDD (Hard Disk Drive) Desktop?", "id": "3.5 Inci." },
{ "en": "Faktor Bentuk HDD Laptop?", "id": "2.5 Inci." },
{ "en": "Faktor Bentuk SSD (Solid State Drive) SATA?", "id": "2.5 Inci." },
{ "en": "Faktor Bentuk SSD (Solid State Drive) Modern?", "id": "M.2." },
{ "en": "Bentuk M.2 Seperti?", "id": "Sebuah Kepingan Permen Karet." },
{ "en": "Slot M.2 Terpasang Langsung Di?", "id": "Motherboard." },
{ "en": "Apakah Semua Slot M.2 Sama?", "id": "Tidak (Ada Kunci B, M, B+M)." },
{ "en": "Kunci M.2 Menentukan Antarmuka?", "id": "SATA Atau PCIe (NVMe)." },
{ "en": "Panjang Modul M.2 Bervariasi?", "id": "Ya (Contoh: 2280, 2242)." },
{ "en": "Angka '2280' Berarti?", "id": "Lebar 22mm, Panjang 80mm." },
{ "en": "Kartu Memori Seperti SD Card Menggunakan?", "id": "Flash Memory." },
{ "en": "Apa Kepanjangan SD Card?", "id": "Secure Digital Card." },
{ "en": "Ukuran Kartu SD?", "id": "Standar, Mini, Dan Micro." },
{ "en": "USB Flash Drive Adalah?", "id": "Flash Memory Dengan Antarmuka USB." },
{ "en": "Apa Kepanjangan USB?", "id": "Universal Serial Bus." },
{ "en": "Apa Itu 'Memory Stick'?", "id": "Format Kartu Memori Propietari Sony." },
{ "en": "Memori Yang Disolder Langsung Disebut?", "id": "Onboard Memory." },
{ "en": "Modul RAM Memiliki Chip Memori Di?", "id": "Satu Sisi Atau Kedua Sisi." },
{ "en": "Ini Berhubungan Dengan Konsep?", "id": "Rank Memori (Single/Dual Rank)." },
{ "en": "Kartu Grafis Adalah Contoh Papan?", "id": "PCB (Printed Circuit Board)." },
{ "en": "Chip VRAM (Video RAM) Terletak Di?", "id": "Sekeliling Chip GPU." },
{ "en": "Memori HBM (High Bandwidth Memory) Ditempatkan Di?", "id": "Substrat Yang Sama Dengan GPU." },
{ "en": "Ini Disebut 'Packaging'?", "id": "2.5D Packaging." },
{ "en": "Menumpuk Chip Secara Vertikal Disebut?", "id": "3D Packaging." },
{ "en": "Contoh 3D Packaging Dalam Memori?", "id": "3D V-Cache AMD." },
{ "en": "Parameter Utama Kinerja RAM?", "id": "Kecepatan Dan Latensi." },
{ "en": "Kecepatan RAM Dinyatakan Dalam?", "id": "MT/s (Megatransfer per detik)." },
{ "en": "Latensi RAM Dinyatakan Dalam?", "id": "Timing (Contoh: CL16)." },
{ "en": "Apa Kepanjangan CL?", "id": "CAS Latency." },
{ "en": "CAS (Column Address Strobe) Latency Adalah?", "id": "Waktu Tunda Akses Kolom Memori." },
{ "en": "Apakah CL Yang Lebih Rendah Lebih Baik?", "id": "Ya, Lebih Cepat." },
{ "en": "Kecepatan Atau Latensi Yang Lebih Penting?", "id": "Keduanya Penting, Merupakan Keseimbangan." },
{ "en": "Bagaimana Mengukur Latensi Sebenarnya (Real Latency)?", "id": "Dalam Nanodetik." },
{ "en": "Rumus Real Latency?", "id": "(CL * 2000) / Kecepatan MT/s." },
{ "en": "RAM 3200MHz CL16 vs 3600MHz CL18?", "id": "Latensi Sebenarnya Hampir Sama." },
{ "en": "Rangkaian Angka Timing RAM (16-18-18-38)?", "id": "CL, tRCD, tRP, tRAS." },
{ "en": "Tegangan Standar Untuk RAM DDR4?", "id": "1.2 Volt." },
{ "en": "Tegangan RAM Saat Overclock (Menggunakan XMP)?", "id": "Bisa Naik Menjadi 1.35V Atau Lebih." },
{ "en": "Apa Itu Memori ECC?", "id": "Memori Dengan Kode Koreksi Kesalahan." },
{ "en": "Apa Kepanjangan ECC?", "id": "Error-Correcting Code." },
{ "en": "Fungsi ECC (Error-Correcting Code)?", "id": "Mendeteksi Dan Memperbaiki Error Memori." },
{ "en": "Berapa Bit Error Yang Bisa Diperbaiki?", "id": "Satu Bit Error Per Word." },
{ "en": "ECC (Error-Correcting Code) Penting Untuk?", "id": "Sistem Yang Membutuhkan Keandalan Maksimal." },
{ "en": "Contohnya?", "id": "Server, Workstation Ilmiah." },
{ "en": "Apakah Motherboard Konsumen Mendukung ECC?", "id": "Jarang, Tergantung Model." },
{ "en": "Secara Fisik, Modul ECC Memiliki?", "id": "Jumlah Chip Ganjil (Biasanya 9)." },
{ "en": "Chip Tambahan Digunakan Untuk?", "id": "Menyimpan Bit Paritas." },
{ "en": "Kinerja RAM ECC Sedikit Lebih?", "id": "Lambat Karena Pengecekan Error." },
{ "en": "RAM Unbuffered Adalah?", "id": "RAM Standar Tanpa Buffer." },
{ "en": "RAM Registered (Buffered) Memiliki?", "id": "Register Antara Chip DRAM Dan Controller." },
{ "en": "Tujuannya?", "id": "Mengurangi Beban Listrik Pada Controller." },
{ "en": "Ini Memungkinkan Pemasangan Modul RAM?", "id": "Dengan Kapasitas Sangat Besar Per Channel." },
{ "en": "Registered RAM Digunakan Di?", "id": "Server." },
{ "en": "Registered RAM Disebut Juga?", "id": "RDIMM." },
{ "en": "Unbuffered RAM Disebut Juga?", "id": "UDIMM." },
{ "en": "Apakah RDIMM Dan UDIMM Bisa Dicampur?", "id": "Tidak." },
{ "en": "Spesifikasi Kecepatan VRAM Diukur Dalam?", "id": "Gbps (Gigabits per detik)." },
{ "en": "Spesifikasi Bandwidth VRAM Diukur Dalam?", "id": "GB/s (Gigabytes per detik)." },
{ "en": "Bandwidth VRAM Dihitung Dari?", "id": "Kecepatan Dan Lebar Bus Memori." },
{ "en": "Apa Itu Memori Virtual?", "id": "Menggunakan Penyimpanan Sekunder Seolah-olah RAM." },
{ "en": "Tujuan Utama Memori Virtual?", "id": "Menjalankan Program Lebih Besar Dari RAM Fisik." },
{ "en": "File Di Hard Disk Untuk Memori Virtual?", "id": "Page File Atau Swap File." },
{ "en": "Proses Memindahkan Data Antara RAM Dan Disk?", "id": "Paging Atau Swapping." },
{ "en": "Siapa Yang Mengelola Memori Virtual?", "id": "Sistem Operasi (OS) Dan MMU." },
{ "en": "Apa Kepanjangan Dari MMU?", "id": "Memory Management Unit." },
{ "en": "MMU (Memory Management Unit) Adalah Bagian Dari?", "id": "CPU (Central Processing Unit)." },
{ "en": "Fungsi MMU?", "id": "Menerjemahkan Alamat Virtual Ke Alamat Fisik." },
{ "en": "Setiap Program Memiliki Ruang Alamat?", "id": "Virtualnya Sendiri." },
{ "en": "Keuntungan Ruang Alamat Virtual?", "id": "Isolasi Memori Antar Program." },
{ "en": "Memori Dibagi Menjadi Blok Ukuran Tetap?", "id": "Page (Halaman)." },
{ "en": "Penyimpanan Sekunder Dibagi Menjadi?", "id": "Blok Dengan Ukuran Sama." },
{ "en": "Tabel Yang Memetakan Alamat Virtual Ke Fisik?", "id": "Page Table." },
{ "en": "Setiap Proses Memiliki Page Table?", "id": "Sendiri." },
{ "en": "Apa Itu 'Page Fault'?", "id": "Saat Page Yang Dibutuhkan Tidak Ada Di RAM." },
{ "en": "Apa Yang Terjadi Saat Page Fault?", "id": "OS Mengambil Page Dari Disk Ke RAM." },
{ "en": "Proses Page Fault Sangat?", "id": "Lambat." },
{ "en": "Terlalu Banyak Page Fault Disebut?", "id": "Thrashing." },
{ "en": "Thrashing Menyebabkan Kinerja Sistem?", "id": "Sangat Menurun." },
{ "en": "Penyebab Thrashing?", "id": "Kekurangan RAM Fisik." },
{ "en": "Solusi Untuk Thrashing?", "id": "Menambah RAM Atau Menutup Aplikasi." },
{ "en": "Cache Di MMU (Memory Management Unit) Untuk Mempercepat Translasi?", "id": "TLB (Translation Lookaside Buffer)." },
{ "en": "Apa Kepanjangan TLB?", "id": "Translation Lookaside Buffer." },
{ "en": "TLB (Translation Lookaside Buffer) Menyimpan?", "id": "Terjemahan Alamat Yang Baru Saja Digunakan." },
{ "en": "Apa Itu 'Working Set'?", "id": "Set Halaman Yang Dibutuhkan Program Saat Ini." },
{ "en": "Sistem Berusaha Menjaga Working Set Di?", "id": "RAM (Random Access Memory)." },
{ "en": "Memori Virtual Juga Memungkinkan?", "id": "Berbagi Memori Antar Proses." },
{ "en": "Contohnya?", "id": "Library Sistem Yang Sama." },
{ "en": "Ukuran Page Yang Umum?", "id": "4 Kilobyte (KB)." },
{ "en": "Selain Paging, Ada Teknik?", "id": "Segmentasi." },
{ "en": "Apa Itu Segmentasi?", "id": "Membagi Memori Menjadi Blok Ukuran Variabel." },
{ "en": "Sistem Modern Menggunakan Kombinasi?", "id": "Paging Dan Segmentasi." },
{ "en": "Alokasi Memori Adalah Tugas?", "id": "Sistem Operasi." },
{ "en": "Apa Itu 'Memory Leak'?", "id": "Program Gagal Melepas Memori Yang Tidak Dipakai." },
{ "en": "Akibat Memory Leak?", "id": "Memori Yang Tersedia Berkurang Seiring Waktu." },
{ "en": "Apa Itu 'Garbage Collection'?", "id": "Proses Otomatis Untuk Melepas Memori." },
{ "en": "Bahasa Pemrograman Yang Menggunakan Garbage Collection?", "id": "Java, Python, C#." },
{ "en": "Bahasa Seperti C/C++ Memerlukan?", "id": "Manajemen Memori Manual." },
{ "en": "Sirkuit Yang Mengatur Aliran Data Ke/Dari Memori?", "id": "Memory Controller." },
{ "en": "Di Komputer Modern, Memory Controller Terletak Di?", "id": "Dalam Chip CPU (Central Processing Unit)." },
{ "en": "Jalur Fisik Untuk Transfer Data Memori?", "id": "Memory Bus." },
{ "en": "Memory Bus Terdiri Dari?", "id": "Bus Alamat, Bus Data, Bus Kontrol." },
{ "en": "Bus Kontrol Membawa Sinyal Seperti?", "id": "Read Enable, Write Enable." },
{ "en": "Apa Itu 'Memory Address Space'?", "id": "Total Rentang Alamat Memori Yang Bisa Diakses." },
{ "en": "Apa Itu 'Memory-Mapped I/O'?", "id": "Menggunakan Alamat Memori Untuk Berkomunikasi Dengan Perangkat." },
{ "en": "I/O Singkatan Dari?", "id": "Input/Output." },
{ "en": "Buffer Adalah Memori Sementara Untuk?", "id": "Menyamakan Perbedaan Kecepatan Transfer." },
{ "en": "Contoh Buffer?", "id": "Buffer Video Saat Streaming." },
{ "en": "Cache Adalah Jenis Buffer?", "id": "Benar." },
{ "en": "Memori Inti Magnetik (Magnetic Core Memory)?", "id": "Jenis RAM Lama Sebelum IC (Integrated Circuit)." },
{ "en": "Bentuknya?", "id": "Cincin Ferit Kecil Yang Dirangkai." },
{ "en": "Keuntungannya?", "id": "Non-Volatile (Menyimpan Data Tanpa Listrik)." },
{ "en": "Memori Gelembung (Bubble Memory)?", "id": "Teknologi Non-Volatile Eksperimental Lama." },
{ "en": "Apa Itu 'Interleaving' Memori?", "id": "Membagi Memori Menjadi Bank Untuk Akses Paralel." },
{ "en": "Tujuannya?", "id": "Meningkatkan Throughput Memori." },
{ "en": "Dual-Channel Adalah Bentuk Dari?", "id": "Interleaving." },
{ "en": "Apa Itu 'Memory Scrambling'?", "id": "Mengacak Pola Data Untuk Mengurangi Interferensi." },
{ "en": "Apa Itu 'Memory Scrubbing'?", "id": "Proses Berkala Memeriksa Dan Memperbaiki Error ECC." },
{ "en": "Penting Di Sistem Apa?", "id": "Server Yang Beroperasi Non-Stop." },
{ "en": "Apa Itu 'Endianness'?", "id": "Urutan Byte Disimpan Di Memori." },
{ "en": "Dua Jenis Endianness?", "id": "Big-Endian Dan Little-Endian." },
{ "en": "Prosesor x86 Menggunakan?", "id": "Little-Endian." },
{ "en": "Apa Itu 'Direct Memory Access' (DMA)?", "id": "Perangkat Mengakses RAM Tanpa Melewati CPU." },
{ "en": "Tujuan DMA (Direct Memory Access)?", "id": "Membebaskan CPU Dari Tugas Transfer Data." },
{ "en": "Kontroler DMA Mengelola Operasi?", "id": "Benar." },
{ "en": "Memori Yang Berbagi Antara CPU Dan GPU?", "id": "Unified Memory Architecture (UMA)." },
{ "en": "Umum Ditemukan Di?", "id": "SoC (System on a Chip) Dan Konsol Game." },
{ "en": "Alamat Memori Fisik Adalah?", "id": "Alamat Aktual Di Chip RAM." },
{ "en": "Alamat Memori Logikal/Virtual Adalah?", "id": "Alamat Yang Dilihat Oleh Program." },
{ "en": "Penerjemahan Dilakukan Oleh?", "id": "MMU (Memory Management Unit)." },
{ "en": "Memori Volatile Di Smartphone?", "id": "RAM (Random Access Memory) Tipe LPDDR." },
{ "en": "Memori Non-Volatile Di Smartphone?", "id": "Internal Storage (Berbasis Flash NAND)." },
{ "en": "Firmware Smartphone (Sistem Operasi) Disimpan Di?", "id": "Flash Memory." },
{ "en": "Memori Di Kamera Digital Untuk Menyimpan Foto?", "id": "Kartu Memori (SD Card, CFexpress)." },
{ "en": "Jenis Memori Di Kartu SD (Secure Digital)?", "id": "Flash Memory Tipe NAND." },
{ "en": "Buffer Internal Di Kamera Menggunakan?", "id": "DRAM (Dynamic Random Access Memory) Cepat." },
{ "en": "Memori Di Router Jaringan?", "id": "RAM (Untuk Operasi) Dan Flash (Untuk Firmware)." },
{ "en": "Memori Di Smart TV?", "id": "RAM (Untuk Aplikasi) Dan Flash (Untuk OS)." },
{ "en": "Memori Di Konsol Game (Playstation/Xbox)?", "id": "RAM Terunifikasi (Biasanya GDDR)." },
{ "en": "Apa Itu RAM Terunifikasi?", "id": "RAM Yang Dibagi Antara CPU Dan GPU." },
{ "en": "Penyimpanan Game Di Konsol?", "id": "Internal SSD (Solid State Drive) NVMe." },
{ "en": "Memori Di Cartridge Game Lama?", "id": "ROM (Read-Only Memory)." },
{ "en": "Memori Untuk Menyimpan Save Game Di Cartridge?", "id": "EEPROM Atau SRAM Dengan Baterai." },
{ "en": "Memori Di Sistem GPS (Global Positioning System)?", "id": "Flash (Untuk Peta) Dan RAM (Untuk Operasi)." },
{ "en": "Memori Di Kalkulator Sederhana?", "id": "Register Dan Sedikit RAM Statis." },
{ "en": "Memori Di Server Komputer?", "id": "RAM ECC (Error-Correcting Code) Kapasitas Besar." },
{ "en": "Penyimpanan Di Server?", "id": "HDD (Hard Disk Drive) Atau SSD (Solid State Drive) Enterprise." },
{ "en": "Memori Di Superkomputer?", "id": "Jumlah RAM Sangat Besar (Terabyte/Petabyte)." },
{ "en": "Memori Di Sistem Embedded (Tertanam)?", "id": "Flash NOR (Untuk Kode) Dan SRAM." },
{ "en": "Mengapa Flash NOR Untuk Kode?", "id": "Karena Mendukung Eksekusi Langsung (XIP)." },
{ "en": "Memori Di Kartu SIM (Subscriber Identity Module)?", "id": "EEPROM (Electrically Erasable Programmable Read-Only Memory)." },
{ "en": "Memori Di Kartu Kredit (Chip)?", "id": "EEPROM (Electrically Erasable Programmable Read-Only Memory)." },
{ "en": "Papan Sirkuit Hijau Tempat RAM Dipasang?", "id": "Motherboard." },
{ "en": "Slot Untuk RAM Di Motherboard?", "id": "Slot DIMM Atau SO-DIMM." },
{ "en": "Memori Di Dalam Prosesor Itu Sendiri?", "id": "Register Dan Cache." },
{ "en": "Memori Yang Digunakan Printer?", "id": "RAM (Untuk Menampung Dokumen Cetak)." },
{ "en": "Memori Di Pemutar MP3 (Moving Picture Experts Group Layer-3 Audio)?", "id": "Flash Memory Tipe NAND." },
{ "en": "Memori Di Perekam Suara Digital?", "id": "Flash Memory Tipe NAND." },
{ "en": "Jenis Memori Dalam 'Black Box' Pesawat?", "id": "Flash Memory Yang Sangat Tahan Banting." },
{ "en": "Memori Di Sistem Mobil Modern (ECU)?", "id": "Flash, EEPROM, Dan SRAM." },
{ "en": "Kepanjangan ECU?", "id": "Electronic Control Unit." },
{ "en": "Memori Di Perangkat IoT (Internet of Things)?", "id": "Flash Memory Kecil Dan RAM Hemat Daya." },
{ "en": "Memori Yang Menyimpan Pola Sidik Jari?", "id": "Flash Memory Aman." },
{ "en": "Pita Magnetik Digunakan Untuk Apa Sekarang?", "id": "Arsip Data Jangka Panjang (Backup)." },
{ "en": "Mengapa Pita Magnetik Untuk Arsip?", "id": "Murah Per Gigabyte Dan Tahan Lama." },
{ "en": "Memori Di Osiloskop Digital?", "id": "RAM Cepat Untuk Menangkap Bentuk Gelombang." },
{ "en": "Memori Di Perangkat Medis Implan?", "id": "Sangat Andal Dan Hemat Daya." },
{ "en": "Apa Itu 'Volatile' Dalam Konteks Memori?", "id": "Membutuhkan Daya Untuk Menyimpan Data." },
{ "en": "Apa Itu 'Non-Volatile' Dalam Konteks Memori?", "id": "Tidak Butuh Daya Untuk Menyimpan Data." },
{ "en": "Perbedaan Utama SRAM Dan DRAM?", "id": "SRAM Cepat Dan Mahal, DRAM Lambat Dan Murah." },
{ "en": "Hubungan Antara Cache Dan RAM?", "id": "Cache Adalah Salinan Cepat Dari Data RAM." },
{ "en": "Mana Yang Lebih Dekat Ke CPU?", "id": "Cache." },
{ "en": "Perbedaan Utama RAM Dan ROM?", "id": "RAM Bisa Ditulis, ROM Hanya Bisa Dibaca." },
{ "en": "Perbedaan Utama RAM Dan SSD?", "id": "RAM Volatile Dan Cepat, SSD Non-Volatile Dan Lambat." },
{ "en": "Perbedaan Utama HDD Dan SSD?", "id": "HDD Mekanis, SSD Elektronik (Solid-State)." },
{ "en": "Mana Yang Lebih Cepat, L1 Atau L2 Cache?", "id": "L1 (Level 1) Cache." },
{ "en": "Mana Yang Lebih Besar, L2 Atau L3 Cache?", "id": "L3 (Level 3) Cache." },
{ "en": "Hubungan Antara Register Dan Cache?", "id": "Register Lebih Cepat Dan Lebih Kecil." },
{ "en": "Perbedaan Antara Volatile Dan Non-Volatile?", "id": "Ketergantungan Pada Daya Listrik." },
{ "en": "Perbedaan Antara Memori Primer Dan Sekunder?", "id": "Kecepatan Dan Aksesibilitas Oleh CPU." },
{ "en": "Mana Yang Langsung Diakses CPU?", "id": "Memori Primer." },
{ "en": "Perbedaan Antara DDR4 Dan DDR5?", "id": "DDR5 Lebih Cepat Dan Efisien." },
{ "en": "Perbedaan Antara DIMM Dan SO-DIMM?", "id": "Ukuran Fisik (Desktop Vs Laptop)." },
{ "en": "Hubungan Antara Kecepatan (MT/s) Dan Latensi (CL)?", "id": "Keduanya Menentukan Kinerja Nyata RAM." },
{ "en": "Memori Cepat Dengan Latensi Tinggi Bisa Jadi?", "id": "Sama Lambatnya Dengan Memori Lambat Latensi Rendah." },
{ "en": "Perbedaan Antara SLC, MLC, TLC, QLC?", "id": "Jumlah Bit Yang Disimpan Per Sel." },
{ "en": "Mana Yang Paling Andal?", "id": "SLC (Single-Level Cell)." },
{ "en": "Mana Yang Paling Padat Kapasitasnya?", "id": "QLC (Quad-Level Cell)." },
{ "en": "Perbedaan Antara SATA Dan NVMe?", "id": "Protokol Komunikasi Dan Kecepatan." },
{ "en": "Mana Yang Menggunakan Jalur PCIe?", "id": "NVMe (Non-Volatile Memory Express)." },
{ "en": "Hubungan Antara Firmware Dan ROM?", "id": "Firmware Disimpan Di Dalam Chip ROM." },
{ "en": "Perbedaan Antara BIOS Dan UEFI?", "id": "UEFI Adalah Antarmuka Firmware Yang Lebih Modern." },
{ "en": "Hubungan Antara Alamat Virtual Dan Fisik?", "id": "MMU Menerjemahkan Virtual Ke Fisik." },
{ "en": "Perbedaan Antara 'Write-Through' Dan 'Write-Back' Cache?", "id": "Waktu Penulisan Data Kembali Ke RAM." },
{ "en": "Mana Yang Punya Risiko Kehilangan Data?", "id": "Write-Back (Jika Listrik Mati Tiba-Tiba)." },
{ "en": "Perbedaan Antara RAM ECC Dan Non-ECC?", "id": "Kemampuan Mendeteksi Dan Memperbaiki Error." },
{ "en": "Perbedaan Antara RAM Registered Dan Unbuffered?", "id": "Adanya Chip Register Di Modul." },
{ "en": "Hubungan Antara Bus Alamat Dan Kapasitas RAM?", "id": "Lebar Bus Menentukan Kapasitas Maksimal." },
{ "en": "Perbedaan Antara Bit Dan Byte?", "id": "Satu Byte Sama Dengan Delapan Bit." },
{ "en": "Mana Satuan Yang Lebih Besar, MB Atau GB?", "id": "GB (Gigabyte)." },
{ "en": "Perbedaan Antara EPROM Dan EEPROM?", "id": "Cara Menghapus Data (UV Vs Listrik)." },
{ "en": "Hubungan Antara Flash Memory Dan EEPROM?", "id": "Flash Adalah Jenis EEPROM Yang Lebih Efisien." },
{ "en": "Fungsi Dari RAM (Random Access Memory)?", "id": "Menyimpan Data Program Yang Aktif." },
{ "en": "Fungsi Dari ROM (Read-Only Memory)?", "id": "Menyimpan Firmware Untuk Booting." },
{ "en": "Fungsi Dari Cache?", "id": "Mempercepat Akses CPU Ke Data RAM." },
{ "en": "Fungsi Dari HDD (Hard Disk Drive)?", "id": "Penyimpanan Massal Jangka Panjang." },
{ "en": "Fungsi Dari SSD (Solid State Drive)?", "id": "Penyimpanan Cepat Untuk OS Dan Aplikasi." },
{ "en": "Fungsi Dari VRAM (Video RAM)?", "id": "Menyimpan Data Grafis Untuk GPU." },
{ "en": "Fungsi Dari BIOS (Basic Input/Output System)?", "id": "Inisialisasi Perangkat Keras Saat Startup." },
{ "en": "Fungsi Dari MMU (Memory Management Unit)?", "id": "Mengelola Dan Menerjemahkan Alamat Memori." },
{ "en": "Fungsi Dari ECC (Error-Correcting Code)?", "id": "Mendeteksi Dan Mengkoreksi Kesalahan Memori." },
{ "en": "Fungsi Dari DMA (Direct Memory Access)?", "id": "Transfer Data Tanpa Melibatkan CPU." },
{ "en": "Fungsi Dari Memory Controller?", "id": "Mengatur Lalu Lintas Data Ke Dan Dari RAM." },
{ "en": "Fungsi Dari 'Wear Leveling'?", "id": "Meratakan Penggunaan Sel Flash SSD." },
{ "en": "Fungsi Dari Perintah TRIM?", "id": "Memberitahu SSD Blok Mana Yang Sudah Kosong." },
{ "en": "Fungsi Dari 'Garbage Collection'?", "id": "Membersihkan Blok Di SSD Untuk Penulisan Baru." },
{ "en": "Fungsi Dari XMP (Extreme Memory Profile)?", "id": "Memuat Pengaturan Kinerja Tinggi RAM." },
{ "en": "Fungsi Dari SPD (Serial Presence Detect)?", "id": "Menyimpan Spesifikasi Standar Modul RAM." },
{ "en": "Fungsi Dari Page File?", "id": "Bertindak Sebagai Perluasan RAM Di Disk." },
{ "en": "Fungsi Dari TLB (Translation Lookaside Buffer)?", "id": "Cache Untuk Terjemahan Alamat Memori." },
{ "en": "Fungsi Dari Heatspreader Pada RAM?", "id": "Membantu Mendinginkan Chip Memori." },
{ "en": "Fungsi Baterai CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Menjaga Pengaturan BIOS Tetap Tersimpan." },
{ "en": "Apa Arti Volatile?", "id": "Membutuhkan Daya Untuk Menyimpan Informasi." },
{ "en": "Apa Arti Non-Volatile?", "id": "Menyimpan Informasi Tanpa Membutuhkan Daya." },
{ "en": "Apa Arti 'Random Access'?", "id": "Waktu Akses Sama Untuk Semua Lokasi." },
{ "en": "Apa Arti 'Sequential Access'?", "id": "Harus Membaca Data Secara Berurutan." },
{ "en": "Contoh 'Sequential Access'?", "id": "Pita Magnetik." },
{ "en": "Apa Arti 'Solid-State'?", "id": "Tidak Memiliki Bagian Mekanis Bergerak." },
{ "en": "Apa Arti 'Firmware'?", "id": "Perangkat Lunak Yang Tertanam Dalam Hardware." },
{ "en": "Apa Arti 'Cache Hit'?", "id": "Data Yang Dicari Ditemukan Di Cache." },
{ "en": "Apa Arti 'Cache Miss'?", "id": "Data Tidak Ditemukan Di Cache." },
{ "en": "Apa Arti 'Latency'?", "id": "Waktu Tunda Awal." },
{ "en": "Apa Arti 'Bandwidth'?", "id": "Laju Transfer Data." },
{ "en": "Apa Arti 'Dual Channel'?", "id": "Menggunakan Dua Jalur Memori Secara Paralel." },
{ "en": "Apa Yang Terjadi Jika Komputer Kehabisan RAM?", "id": "Menggunakan Memori Virtual Secara Intensif." },
{ "en": "Konsekuensi Dari Kehabisan RAM?", "id": "Sistem Menjadi Sangat Lambat (Thrashing)." },
{ "en": "Mengapa Menambah RAM Bisa Mempercepat Komputer?", "id": "Mengurangi Ketergantungan Pada Page File." },
{ "en": "Apa Yang Terjadi Jika Cache Dihilangkan?", "id": "CPU Akan Selalu Menunggu RAM, Sangat Lambat." },
{ "en": "Konsekuensi Menggunakan HDD Untuk Sistem Operasi?", "id": "Waktu Boot Dan Muat Aplikasi Lambat." },
{ "en": "Mengapa SSD (Solid State Drive) Membuat Komputer Terasa Cepat?", "id": "Karena Waktu Aksesnya Sangat Rendah." },
{ "en": "Apa Yang Terjadi Jika Baterai CMOS Mati?", "id": "Jam Dan Pengaturan BIOS Selalu Reset." },
{ "en": "Konsekuensi Menjalankan Game Dengan VRAM Kurang?", "id": "Stuttering Dan Tekstur Gagal Dimuat." },
{ "en": "Apa Yang Terjadi Jika Anda Mendefrag SSD?", "id": "Mengurangi Umur Pakai Tanpa Manfaat Kinerja." },
{ "en": "Konsekuensi Listrik Mati Saat Menulis Ke HDD?", "id": "Potensi Data Rusak (Corrupt)." },
{ "en": "Konsekuensi Mencampur RAM Beda Kecepatan?", "id": "Semua RAM Akan Berjalan Pada Kecepatan Terendah." },
{ "en": "Mengapa Memori Terorganisir Dalam Hirarki?", "id": "Untuk Menyeimbangkan Biaya Dan Kecepatan." },
{ "en": "Jika RAM Secepat Register, Apa Yang Terjadi?", "id": "Komputer Akan Sangat Mahal." },
{ "en": "Jika Hanya Ada HDD (Tanpa RAM)?", "id": "Komputer Tidak Akan Bisa Berfungsi." },
{ "en": "Mengapa ROM Dibutuhkan Saat Boot?", "id": "Karena RAM Kosong Saat Komputer Dinyalakan." },
{ "en": "Apa Yang Terjadi Jika Flash Drive Dicabut Saat Transfer?", "id": "File Yang Ditransfer Kemungkinan Besar Rusak." },
{ "en": "Konsekuensi Dari 'Memory Leak'?", "id": "Kinerja Sistem Menurun Seiring Waktu." },
{ "en": "Apa Yang Terjadi Jika Overclock RAM Gagal?", "id": "Sistem Mungkin Gagal Boot Atau Tidak Stabil." },
{ "en": "Mengapa Server Butuh RAM ECC?", "id": "Satu Bit Error Bisa Menyebabkan Kerugian Besar." },
{ "en": "Apa Yang Terjadi Jika Data Di Cache Tidak Koheren?", "id": "Core CPU Bisa Bekerja Dengan Data Salah." },
{ "en": "Konsekuensi Dari 'Cache Miss' Yang Sering?", "id": "CPU Menjadi 'Stalled' (Menunggu)." },
{ "en": "Mengapa Memori Volatile Lebih Cepat?", "id": "Teknologinya Lebih Sederhana Dan Langsung." },
{ "en": "Apa Yang Terjadi Jika Anda Mengisi SSD Penuh?", "id": "Kinerjanya Akan Menurun Drastis." },
{ "en": "Mengapa Kinerjanya Turun?", "id": "Karena Kontroler Kesulitan Menemukan Blok Kosong." },
{ "en": "Aturan Praktis Untuk Mengisi SSD?", "id": "Sisakan Sekitar 10-20% Ruang Kosong." },
{ "en": "Apa Yang Terjadi Jika Anda Meletakkan Magnet Kuat Dekat HDD?", "id": "Data Bisa Terhapus Atau Rusak." },
{ "en": "Apakah Magnet Mempengaruhi SSD Atau RAM?", "id": "Tidak." },
{ "en": "Konsekuensi Dari 'Page Fault'?", "id": "Penundaan Singkat Saat Data Dibaca Dari Disk." },
{ "en": "Jika Terjadi 'Double-Bit Error' Pada RAM ECC?", "id": "Error Terdeteksi Tapi Tidak Bisa Diperbaiki." },
{ "en": "Siapa Yang Meminta Data Dari Memori?", "id": "CPU (Central Processing Unit)." },
{ "en": "Siapa Yang Mengelola Aliran Data Ke RAM?", "id": "Memory Controller." },
{ "en": "Siapa Yang Menyimpan Data Program Aktif?", "id": "RAM (Random Access Memory)." },
{ "en": "Siapa Yang Menyimpan Salinan Cepat Data RAM?", "id": "Cache." },
{ "en": "Siapa Yang Menerjemahkan Alamat Virtual Ke Fisik?", "id": "MMU (Memory Management Unit)." },
{ "en": "Siapa Yang Mengelola Memori Virtual Dan Paging?", "id": "Sistem Operasi (OS)." },
{ "en": "Siapa Yang Menyimpan Firmware Untuk Booting?", "id": "ROM (Read-Only Memory)." },
{ "en": "Siapa Yang Melakukan Proses Wear Leveling?", "id": "Kontroler SSD (Solid State Drive)." },
{ "en": "Siapa Yang Menjalankan Perintah TRIM?", "id": "Sistem Operasi (OS)." },
{ "en": "Siapa Yang Melakukan Garbage Collection Di SSD?", "id": "Kontroler SSD (Solid State Drive)." },
{ "en": "Siapa Yang Membaca Chip SPD Saat Boot?", "id": "BIOS Atau UEFI." },
{ "en": "Siapa Yang Menerapkan Pengaturan XMP?", "id": "Pengguna (Melalui BIOS/UEFI)." },
{ "en": "Siapa Yang Melakukan Pengecekan Error ECC?", "id": "Sirkuit Khusus Di Memori Controller." },
{ "en": "Siapa Yang Menyimpan Data Jangka Panjang?", "id": "Penyimpanan Sekunder (HDD/SSD)." },
{ "en": "Siapa Yang Menjalankan Operasi Aritmatika?", "id": "ALU (Arithmetic Logic Unit) Di CPU." },
{ "en": "Siapa Yang Menyimpan Hasil Sementara ALU?", "id": "Register." },
{ "en": "Siapa Yang Membutuhkan VRAM Paling Banyak?", "id": "GPU (Graphics Processing Unit)." },
{ "en": "Siapa Yang Memutuskan Blok Mana Yang Diganti Di Cache?", "id": "Logika Replacement Policy." },
{ "en": "Siapa Yang Menjaga Koherensi Data Antar Cache?", "id": "Protokol Cache Coherence (MESI)." },
{ "en": "Siapa Yang Melakukan Proses Refresh Pada DRAM?", "id": "Memory Controller." },
{ "en": "Siapa Yang Memberi Daya Pada CMOS RAM?", "id": "Baterai Motherboard." },
{ "en": "Siapa Yang Memproduksi Chip Flash NAND?", "id": "Pabrikan Semikonduktor (Samsung, Micron)." },
{ "en": "Siapa Yang Merakit Modul DIMM?", "id": "Merek Pihak Ketiga (Corsair, Kingston)." },
{ "en": "Siapa Yang Membutuhkan RAM Registered?", "id": "Server Dengan Banyak Slot Memori." },
{ "en": "Siapa Yang Mengalami Page Fault?", "id": "Proses Atau Aplikasi." },
{ "en": "Siapa Yang Menangani Page Fault?", "id": "Sistem Operasi (OS)." },
{ "en": "Siapa Yang Menggunakan Memori HBM?", "id": "Kartu Grafis High-End." },
{ "en": "Siapa Yang Menggunakan Memori LPDDR?", "id": "Smartphone Dan Laptop Tipis." },
{ "en": "Siapa Yang Melakukan Proses Defragmentasi?", "id": "Utilitas Disk Di Sistem Operasi." },
{ "en": "Defragmentasi Hanya Perlu Untuk Siapa?", "id": "HDD (Hard Disk Drive)." },
{ "en": "Siapa Yang Menggunakan Penyimpanan Pita Magnetik?", "id": "Pusat Data Untuk Arsip." },
{ "en": "Siapa Yang Membaca Kode QR (Quick Response)?", "id": "Kamera Dan Perangkat Lunak." },
{ "en": "Memori Kode QR Bersifat?", "id": "Read-Only (Setelah Dicetak)." },
{ "en": "Siapa Yang Menulis Data Ke Kartu Memori?", "id": "Perangkat Seperti Kamera Atau Ponsel." },
{ "en": "Benarkah SRAM Perlu Di-Refresh?", "id": "Salah, DRAM Yang Perlu Di-Refresh." },
{ "en": "Benarkah SSD Perlu Didefragmentasi?", "id": "Salah, Itu Justru Memperpendek Umurnya." },
{ "en": "Benarkah Menambah RAM Selalu Mempercepat Game?", "id": "Salah, Hanya Jika RAM Sebelumnya Kurang." },
{ "en": "Benarkah ROM Adalah Memori Volatile?", "id": "Salah, ROM Adalah Non-Volatile." },
{ "en": "Benarkah Cache Lebih Cepat Dari Register?", "id": "Salah, Register Adalah Yang Tercepat." },
{ "en": "Benarkah DDR4 Kompatibel Dengan Slot DDR5?", "id": "Salah, Bentuk Fisiknya Berbeda." },
{ "en": "Benarkah Dual-Channel Menggandakan Kapasitas RAM?", "id": "Salah, Menggandakan Bandwidth Bukan Kapasitas." },
{ "en": "Benarkah HDD Lebih Tahan Guncangan Dari SSD?", "id": "Salah, SSD Jauh Lebih Tahan Guncangan." },
{ "en": "Benarkah Latensi CAS (CL) Lebih Tinggi Lebih Baik?", "id": "Salah, CL Lebih Rendah Lebih Baik." },
{ "en": "Benarkah Memori Virtual Secepat RAM Fisik?", "id": "Salah, Jauh Lebih Lambat." },
{ "en": "Benarkah RAM ECC Hanya Untuk Mendeteksi Error?", "id": "Salah, Bisa Mendeteksi Dan Memperbaiki." },
{ "en": "Benarkah Semua Motherboard Mendukung RAM ECC?", "id": "Salah, Hanya Motherboard Server/Workstation." },
{ "en": "Benarkah Flash Memory Punya Siklus Tulis Tak Terbatas?", "id": "Salah, Siklus Tulisnya Terbatas." },
{ "en": "Benarkah Lokalitas Temporal Artinya Mengakses Data Berdekatan?", "id": "Salah, Itu Adalah Lokalitas Spasial." },
{ "en": "Benarkah 'Cache Hit' Adalah Hal Yang Buruk?", "id": "Salah, 'Cache Hit' Sangat Baik Untuk Kinerja." },
{ "en": "Benarkah Data Di RAM Hilang Saat Restart?", "id": "Benar, Karena RAM Adalah Volatile." },
{ "en": "Benarkah BIOS Disimpan Di Hard Disk?", "id": "Salah, Disimpan Di Chip ROM Di Motherboard." },
{ "en": "Benarkah Flash NAND Lebih Cepat Dari Flash NOR?", "id": "Benar, Untuk Penulisan Dan Penghapusan." },
{ "en": "Benarkah SLC NAND Paling Murah?", "id": "Salah, SLC Adalah Yang Paling Mahal." },
{ "en": "Benarkah Wear Leveling Memperpendek Umur SSD?", "id": "Salah, Justru Memperpanjang Umur SSD." },
{ "en": "Benarkah Antarmuka NVMe Lebih Lambat Dari SATA?", "id": "Salah, NVMe Jauh Lebih Cepat." },
{ "en": "Benarkah Kapasitas Memori Diukur Dalam Hertz?", "id": "Salah, Diukur Dalam Byte (GB, TB)." },
{ "en": "Benarkah Satu Byte Terdiri Dari 16 Bit?", "id": "Salah, Terdiri Dari 8 Bit." },
{ "en": "Benarkah Hirarki Memori Bertujuan Membuat Semuanya Cepat?", "id": "Benar, Dengan Menyeimbangkan Biaya." },
{ "en": "Benarkah VRAM Adalah Memori Untuk CPU?", "id": "Salah, VRAM Untuk GPU." },
{ "en": "Benarkah XMP Adalah Standar Kecepatan RAM?", "id": "Salah, Ini Adalah Profil Overclock." },
{ "en": "Benarkah Baterai CMOS Memberi Daya Pada RAM?", "id": "Salah, Memberi Daya Pada Chip CMOS." },
{ "en": "Benarkah Memori Registered Lebih Cepat Dari Unbuffered?", "id": "Salah, Sedikit Lebih Lambat Karena Ada Register." },
{ "en": "Benarkah LPDDR Mengkonsumsi Lebih Banyak Daya?", "id": "Salah, LPDDR Didesain Untuk Daya Rendah." }



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
